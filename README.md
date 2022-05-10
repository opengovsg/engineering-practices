# Engineering Practices at Open Government Products

This directory contains the engineering practices that are observed at
the Open Government Products division of the Singapore Government.

This repository is intended to be a living, breathing document that
evolves as the organisation grows. All OGP engineers are welcome to make
contributions or propose changes, by filing a pull request using the
Request For Comment (RFC) template available in this repository.

Practices are currently in place for the following areas:

## Developer Environment

- [Laptop policy](./developer-environment/laptops.md)
- Google Groups
- [Passwords and credentials](developer-environment/passwords.md)
- IDE
  - [Setting up linters](https://github.com/opengovsg/ts-template)

## Source Control

- [Branching](./source-control/branching.md)
- Pre-Commit Checks
  - Linting
  - Secrets
- Checks during Continuous Integration
  - Secrets
  - Security
- [Code Review Processes](./source-control/commits-and-prs.md)

## Coding

- Backend
  - RESTful API routing organisation
  - Using Object Relational Mappers (ORMs)

- Frontend
  - Template Screens
  - State Management

- Testing
  - Unit
  - Integration
  - End-to-End

- Libraries and tools
  - [@opengovsg/mockpass](https://www.npmjs.com/package/@opengovsg/mockpass) - mock SingPass/CorpPass/MyInfo server for dev purposes
  - [@opengovsg/sgid-client](https://www.npmjs.com/package/@opengovsg/sgid-client) - official TypeScript/JavaScript SDK for sgID integration
  - [@opengovsg/myinfo-gov-client](https://www.npmjs.com/package/@opengovsg/myinfo-gov-client) - type-safe TypeScript/JavaScript SDK for MyInfo integration
  - [@opengovsg/formsg-sdk](https://www.npmjs.com/package/@opengovsg/formsg-sdk) - official FormSG webhook SDK

## [Deployments](./deploying)

- Using CI/CD
- [Release practices](deploying/release-practices.md)
- [Deploying to AWS, Step-by-Step](./deploying/AWS.md)
- Organising and Naming Environments
- Hosting
- Scaling
- [Infrastructure guide](https://docs.google.com/document/d/1vQLuUeSOU9VEffTSF7wFRLqw7LNkrEA4rcd-dYvdOlU/edit?usp=sharing)
  - Domain Name Service (DNS)
  - Content Delivery Service (CDN)
  - Databases
  - Load Balancing
- [Backup guide](https://docs.google.com/document/d/1E7wk6hmbVkyX5rRHVDVhpy3vpQW0wqChcprlBoojc9k/edit?usp=sharing)
- [Production guide](https://docs.google.com/document/d/1Uxui35zRHYJ4CZxYhNXON-1e2QjLQQLn9Q55dgzI_k0/edit?usp=sharing)

## Monitoring and Incident Response

- [Monitoring and alerting](./monitoring-and-incident-response/monitoring.md)
- [Incident response and on-calls](./monitoring-and-incident-response/incident-response.md)
- Lighthouse for QC'ing Web Pages

## Security

- [Vulnerability detection and remediation](./security/vulnerabilities.md)
- [Accessing production systems](./security/accessing-production-systems.md)

## Project maintenance

- [Maintenance guidelines](./maintenance/maintenance.md)

## Useful starter code templates

- [ts-template](https://github.com/opengovsg/ts-template) - Web application template
- [ts-job-template](https://github.com/opengovsg/ts-job-template) - Scheduled batch jobs that execute on AWS Elastic Container Service

## Marketing and Engagement

- Facebook
- Twitter
- Zendesk

## Policy exemptions

See [here](https://docs.google.com/document/d/1-NwsqQNDb9VydMQsWg3g_cTEuCE96N8qZ0y2asHv0gQ/edit?usp=sharing).
