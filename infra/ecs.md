# Setting up ECS

This guide walks you through setting up your application servers on ECS with deployment using AWS CodeDeploy. The guide assumes you have basic knowledge of AWS and have deployed applications to ElasticBeanstalk (widely used for deploying application servers in OGP).

## Network Preparation
- 3 public subnets (one in each availability zone in ap-southeast-1)
  - This is where the load balancer is going to be. So this subnets should have a internet gateway attached via their route table
- 3 hybrid subnets (one in each availability zone in ap-southeast-1)
  - For lack of a better term, by hybrid subnet we refer to subnets which have a NAT gateway attached via their route table. They allow outbound connections to the internet but not inbound. You could also use a egress-only internet gateway.
  - This is where the containers are going to live. Application containers usually need internet access to call external services like Stripe or Twilio. You might also need internet connection if you use a error logging service like Sentry
- Security groups
  - Security group for the load balancer
    - Allow connections on **port** `443` from the internet `0.0.0.0/0`
    - e.g. `projectname-env-api-lb-sg`
  - Security group for the containers (ECS tasks)
    - Allow connections on **port** `10000` from the load balancer security group
    - e.g. `projectname-env-api-task-sg`

## IAM Preparation
- Task role (e.g. `projectname-env-ecs-api-task-role`)
  - Used by the containers to call AWS serviced
  - Steps:
    - IAM > Create Role > Elastic Container Service > Elastic Container Service Task
- Task execution role (e.g. `projectname-env-ecs-task-execution-role`)
  - Used by ECS to pull container images and publish container logs
- Policy with write access to CloudWatch logs
  - actions:
    - `logs:CreateLogStream`
    - `logs:PutLogEvents`
  - resources:
    - `log-group:/ecs/projectname-env-*:log-stream:*`
    - `log-group:/ecs/projectname-env-*`
- Policy with read access to ECR
    - actions:
      - `ecr:GetDownloadUrlForLayer`
      - `ecr:BatchGetImage`
      - `ecr:DescribeImages`
      - `ecr:BatchCheckLayerAvailability`
    - resources:
      - `arn:aws:ecr:ap-southeast-1:accountnumber:repository/projectname-env-*`
- Policy with login access to ECR
    - action:
      - `ecr:GetAuthorizationToken`
    - resources:
      - `*`
- Attach policies to task execution role

## ECS Setup
- Create ECS Task Definition
  - Task definition name
    - e.g. `projectname-env-api`
  - Task role
    - Use the task role crated earlier
  - Task execution role
    - Use the task execution role created earlier
  - Task memory
    - Use atleast 2048MB for a Node application.
  - Task CPU
    - Use atleast 1024 for a Node application.
- Create temporary target group to be used during the creation of the load balancer
  - Use "instances" type
  - Select the right VPC
- Create the load balancer for ECS
  - Select `Application Load Balancer`
  - Name the load balancer
    - e.g. `projectname-env-api-lb`
  - Select `Internet-facing`
  - Select the appropriate VPC
  - Select the public subnets created earlier
  - Select the load balancer security group created earlier
  - Select the temporary target group created earlier
  - Create load balancer
- Remove temporary target group from load balancer
- Create ECS Service
  - Switch to `launch` type
  - Select `FARGATE`
  - Select `Blue/green deployment (powered by AWS CodeDeploy)`
  - Select the appropriate VPC
  - Select the hybrid subnets
  - Select the security group for tasks
  - Disable auto-assign public IP
  - Select the application load balancer created in previous steps
  - Create "Production" listener port 443:HTTPS
  - Disable "Test" listener
  - Configure target groups:
    - Target group 1
      - Name: `projectname-env-api-tg-a`
      - Path pattern: `/`
      - Evaluation order: `1`
      - Health check path: `/`
    - Target group 2
      - Name: `projectname-env-api-tg-b`
      - Path pattern: `/`
      - Evaluation order: `1`
      - Health check path: `/`
# Github Action for ECS Deployment
```
name: ecs-deployment-action
# Enironment variables defined here are avaialble on all jobs, steps in this workflow
concurrency:
  # Only one instance of this workflow will run at a time
  group: ${{ github.ref }}
  cancel-in-progress: true
env:
  STAGING_BRANCH: 'develop'
  PRODUCTION_BRANCH: 'master'
  AWS_REGION: 'ap-southeast-1'
  # The container name is the same for both staging and production ECS tasks
  ECS_TASK_CONTAINER_NAME: 'projectname-api'
on:
  push:
    branches: ['develop', 'master']
jobs:
  ecs-deployment:
    name: Deploy to AWS ECS
    runs-on: ubuntu-latest
    steps:
        # This steps checks out the code (Not done by default)
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Configure deployment environment name for staging
        # This step configures environment variables based on current branch
        # Write envvars to $GITHUB_ENV so that they are available in the following steps too
        if: ${{ github.ref == format('refs/heads/{0}', env.STAGING_BRANCH) }}
        run: |
          echo 'Set deployment env to STAGING'
          echo "DEPLOYMENT_ENV=STAGING" >> $GITHUB_ENV
          echo "ECR_REPOSITORY=projectname-env-api" >> $GITHUB_ENV
          echo "ECS_TASK_DEFINITION_NAME=projectname-env-api" >> $GITHUB_ENV
          echo "ECS_CLUSTER_NAME=projectname-env-ecs-cluster" >> $GITHUB_ENV
          echo "ECS_SERVICE_NAME=projectname-env-api" >> $GITHUB_ENV
          echo "CODEDEPLOY_APPLICATION=AppECS-projectname-env-ecs-cluster-projectname-env-api" >> $GITHUB_ENV
          echo "CODEDEPLOY_DEPLOYMENT_GROUP=DgpECS-projectname-env-ecs-cluster-projectname-env-api" >> $GITHUB_ENV
      - name: Configure deployment environment name for production
        # This step configures environment variables based on current branch
        # Write envvars to $GITHUB_ENV so that they are available in the following steps too
        if: ${{ github.ref == format('refs/heads/{0}', env.PRODUCTION_BRANCH) }}
        run: |
          echo 'Set deployment env to PRODUCTION'
          echo "DEPLOYMENT_ENV=PRODUCTION" >> $GITHUB_ENV
          echo "ECR_REPOSITORY=projectname-env-api" >> $GITHUB_ENV
          echo "ECS_TASK_DEFINITION_NAME=projectname-env-api" >> $GITHUB_ENV
          echo "ECS_CLUSTER_NAME=projectname-env-ecs-cluster" >> $GITHUB_ENV
          echo "ECS_SERVICE_NAME=projectname-env-api" >> $GITHUB_ENV
          echo "CODEDEPLOY_APPLICATION=AppECS-projectname-env-ecs-cluster-projectname-env-api" >> $GITHUB_ENV
          echo "CODEDEPLOY_DEPLOYMENT_GROUP=DgpECS-projectname-env-ecs-cluster-projectname-env-api" >> $GITHUB_ENV
      - name: Configure AWS credentials
        # This step configures env with AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID  }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY  }}
          aws-region: ${{ env.AWS_REGION }}
      - name: Login to Amazon ECR
        # This step authenticates the Docker client with AWS ECR so that we can push images to it
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v1
      - name: Build, tag, and push image to Amazon ECR
        # This step builds a Docker image with our codebase and pushes the image to ECR
        id: build-image
        env:
          ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          ECR_REPOSITORY: ${{ env.ECR_REPOSITORY }}
          IMAGE_TAG: ${{ github.sha }}
        # This step builds the image and adds the following tags to it: latest and current SHA commit
        # ::set-output name=image::XXXXXX => Sets the output of this step to be used in other steps
        # e.g. steps.build-image.outputs.image (steps.{step_name}.outputs.{key_name})
        run: |
          docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG -t $ECR_REGISTRY/$ECR_REPOSITORY:latest .
          docker push --all-tags $ECR_REGISTRY/$ECR_REPOSITORY
          echo "::set-output name=image::$ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG"
      - name: Download task definition
        # This step saves curent task definition from ECS into task-definition.json
        run: |
          aws ecs describe-task-definition --task-definition $ECS_TASK_DEFINITION_NAME --query taskDefinition > task-definition.json
      - name: Fill in the new image ID in the Amazon ECS task definition
        # This step updates the image url in task-definition.json with the url of image we just pushed
        id: task-def
        uses: aws-actions/amazon-ecs-render-task-definition@v1
        with:
          task-definition: task-definition.json
          container-name: ${{ env.ECS_TASK_CONTAINER_NAME }}
          image: ${{ steps.build-image.outputs.image }}
      - name: Deploy Amazon ECS task definition
        # This step deploys the new task definition with ECS and Code Deploy
        uses: aws-actions/amazon-ecs-deploy-task-definition@v1
        with:
          task-definition: ${{ steps.task-def.outputs.task-definition }}
          cluster: ${{ env.ECS_CLUSTER_NAME }}
          service: ${{ env.ECS_SERVICE_NAME }}
          wait-for-service-stability: true
          # The appspec.js is in root of the codebase.
          # This action will replace the TaskDefinition property with the task definition version created above.
          codedeploy-appspec: appspec.json
          codedeploy-application: ${{ env.CODEDEPLOY_APPLICATION }}
          codedeploy-deployment-group: ${{ env.CODEDEPLOY_DEPLOYMENT_GROUP }}
```
