# Getting started with Datadog

The simplest way to start reaping the benefits of Datadog’s features is to use link your AWS account. All of the metrics in your AWS account can be found in Cloudwatch. With this integration, you have access to all those data in Datadog.

# Steps

1. Get access to OGP’s Datadog account (Ping Aditya or Pallani on Slack)
2. Go to the [Integrations page](https://app.datadoghq.com/account/settings#integrations/amazon-web-services)
![Untitled](https://user-images.githubusercontent.com/10253341/172754932-71b7f0a4-877d-4550-ad5a-b6cba8be35bf.png)
3. Scroll down and select `Add Another Account`
![Untitled 1](https://user-images.githubusercontent.com/10253341/172755061-9794db67-0d3a-4069-a2be-1ff4cf97d7aa.png)
4. Select `Automatically Using CloudFormation`
5. Select the Datadog products you want to integrate with
6. Select `ap-southeast-1` as the region
7. Create a new Datadog API Key and give it your project name
![Untitled 2](https://user-images.githubusercontent.com/10253341/172755113-593e9893-903a-4bc9-a4eb-d1bb332e6839.png)
![Untitled 3](https://user-images.githubusercontent.com/10253341/172755130-5e010d76-3ffa-4528-b5cd-db0ede63137c.png)
![Untitled 4](https://user-images.githubusercontent.com/10253341/172755142-a985aeb9-3208-4490-a26c-c475dc2008ec.png)
8. Sign in using the root account (or any account with [these permissions](https://docs.datadoghq.com/integrations/amazon_web_services/#aws-integration-iam-policy))
9. Review the CloudFormation configuration
![Untitled 5](https://user-images.githubusercontent.com/10253341/172755228-f9935e93-725c-4fe6-a868-6276225acca2.png)
10. Select `Create stack`
11. Wait for the CloudFormation stack to be created (less than 5 minutes)
![Untitled 6](https://user-images.githubusercontent.com/10253341/172755274-39d85506-0cc8-49e3-90ee-5a9de6199e5b.png)
12. You can inspect each stack in order to see what actions took place and what AWS resources were created/updated
![When the resource created is `AWS::CloudFormation::Stack`, this is a parent stack which spawned other stacks.](https://user-images.githubusercontent.com/10253341/172755719-e045cfe4-0336-4063-84bf-05a8cbefcb9a.png)
*When the resource created is `AWS::CloudFormation::Stack`, this is a parent stack which spawned other stacks.*
13. Add a service tag to the integration `service:projectname`
![Untitled 8](https://user-images.githubusercontent.com/10253341/172793401-5640d661-f591-45fc-a36e-3215a6d35f19.png)
14. To set up log forwarding, go to the lambda function created by the CloudFormation template. The lambda function’s name will contain `Forwarder`. Copy the ARN.
15. Go to `Collect Logs` tab of AWS Integration and select the account you just set up and paste the ARN of the lambda function
![Untitled 9](https://user-images.githubusercontent.com/10253341/172794614-621be561-237e-40c6-aefc-f735130782e2.png)
16. Select the logs you want to forward to Datadog
![Untitled 10](https://user-images.githubusercontent.com/10253341/172794676-aaeb0d3b-59f9-481e-a68e-c4830949dec8.png)
17. As the logs and metrics are not being streamed in real-time, it will take a while before they start showing up on Datadog. You can also set up the [streaming mechanism](https://docs.datadoghq.com/integrations/guide/aws-cloudwatch-metric-streams-with-kinesis-data-firehose/) on the Datadog-AWS config page. More
![Untitled 11](https://user-images.githubusercontent.com/10253341/172794722-dec18402-3126-47a2-b139-5674b4aca71c.png)
18. Create a dashboard and add some graphs
 - You can filter for metrics that are coming from your AWS account using the custom tag you configured before. e.g. `service:parkingsg`

# Resources
- [https://docs.datadoghq.com/integrations/amazon_web_services/](https://docs.datadoghq.com/integrations/amazon_web_services/)
- Log Forwarder
    - [https://docs.datadoghq.com/serverless/libraries_integrations/forwarder/#installation](https://docs.datadoghq.com/serverless/libraries_integrations/forwarder/#installation)
    - Log forwarder configuration
        - Determines whether the default configuration for the Datadog Lambda Log Forwarder is installed as part of this stack. This is useful for sending logs to Datadog for use in Log Management or Cloud SIEM. Customers who want to customise this setup to include specific custom tags, data scrubbing or redaction rules, or send logs using AWS PrivateLink should select ?no? and install this independently
