# Monitoring and alerting

Understanding the state of your infrastructure and systems is essential for ensuring the **reliability** and **stability** of your services. Information about the health and performance of your deployments not only helps the team react to issues, it also gives them the security to make changes with confidence. One of the best ways to gain this insight is with a robust monitoring system that gathers metrics, visualizes data, and alerts operators when things appear to be broken.

The key performance indicators for any web application are

1. Availability
2. Latency
3. Error rates

## Types of monitoring

### Availability monitoring

At the outer edge of monitoring are black box availability and user-centric uptime monitoring tools. These will test the uptime and response times for your site by pinging predefined routes periodically and reporting back on them. Using these tools, you can get a quick idea of how your application is responding, and better understand uptime and performance statistics. However, uptime monitoring should be used as a last line of defense and for understanding the end-user experience. By the time an uptime checker knows that your site is slow or down, your users do too, and problems can often be discovered and alerted on before issues ever reach your customers.

OGP uses [Pingdom](https://www.pingdom.com/) for availability monitoring.

### Application monitoring

Application monitoring is commonly used to provide deep insights into the state and performance of your applications. This type of monitoring is typically installed as a library in your server application (such as [express-winston](https://www.npmjs.com/package/express-winston) for ExpressJS), providing the opportunity for deep customisation of what is logged. Application logs can provide a wide range of information such as network request and response, database connectivity, login and security, business transactions, and other metrics related to application health. These are usually integrated with cloud services such as AWS CloudWatch to create higher-order metrics for monitoring purposes.

Systems should also log important transactional events that occur. For example, a form creation system should be logging all submissions that take place; a payments system should log the intermediate states of all payments performed. This helps to establish ground truth for use cases such as debugging, incident response and even addressing user support queries.

#### Client-side application monitoring

Client-side tracking has the advantage of taking place on the user’s device, giving direct access to contextual user-specific data and behavior when using the application. This is exceptionally useful given the wide range of potential actions that a user may take on the client-side, not all of which results in a server interaction. Client-side monitoring also has an advantage when it comes to how easy it is to install - many vendors supply a snippet of code that’s as easy as copying and pasting to implement. Client-side tracking tags have also been standard practice for many years, making it a common task in the industry.

OGP typically uses [Google Analytics](https://analytics.google.com) for client-side application monitoring, although we do not preclude other analytics trackers should the need arise.

### Error monitoring

Ideally, we would want to identify bugs in your web application before users encounter them. Even worse, users probably will not take the time to inform you that your app crashed on them. Monitoring errors are critical to delivering bug-free applications, and can be collected from both server and client.

#### Server-side monitoring

Server-side monitoring has plenty of benefits such as accuracy, control, and reliability.

1. HTTP Errors – The number of web requests that ended in an error
2. Logged Exceptions – Number of unhandled and logged errors from the application
3. Thrown Exceptions – Number of all exceptions that have been thrown

#### Client-side error monitoring

Client-side tracking has the advantage of taking place on the user’s device, giving direct access to contextual user-specific data, behavior and even call stack traces. This is exceptionally useful given the wide range of client browsers, mobile devices and various configurations that are expected to execute the application code. Client-side monitoring also has an advantage when it comes to how easy it is to install. Many vendors supply a snippet of code that’s as easy as copying and pasting to implement.

The difference between server-side and client-side monitoring is that more effort is required to separate the signal from the noise in the logs, as the client environment is not under the control of the application developer. For example, applications frequently break or perform differently due to browser compatibility issues or differences with native device configuration. However it is still better to set up client-side monitoring so that there is a feedback mechanism for developers to be aware of such issues.

OGP uses [Sentry.io](https://sentry.io) for client-side error monitoring. 

### Log monitoring

While application logs provide aggregatable health information of services over time, they are less useful in identifying why something is going wrong deeper in the stack. Infrastructure logs are generated all the time, but they provide information lower in the stack than application logs.

For example, load balancers usually contain logs of all network requests, which are helpful for post-incident impact assessment in case the application server is unresponsive. Underlying application server metrics such as CPU utilisation and network performance are frequently pre-collected by the cloud provider and exposed as convenient metrics on deployment platforms such as AWS Elastic Beanstalk. Important database metrics are also frequently provided out-of-the-box by services such as AWS RDS, allowing quick detection of database performance issues such as missing indexes, table scans and page faults.

### User feedback monitoring

Despite automated monitoring, there may still be small issues that slip through the cracks and plague users in the real-world. Having direct channels for users to surface problems  - such as emails, feedback forms etc. - can be extremely helpful to understand how the software is behaving and perceived in the wild.

Granted, not all feedback from these channels are actionable (e.g. corporate network firewalls), and some may even be obscure (e.g. a UI bug that only affects IE10 users) or even irrelevant. However, it is always better to have a direct communication channel so that users can report any issues that are occurring firsthand.

### Batch job monitoring

In the simplest terms, a batch job is a scheduled program that is run periodically without further user interaction. Batch jobs are frequently used to automate long-running tasks that need to be performed on a regular basis - typical use cases include data processing, report generation and billing.

Some applications in OGP require periodic batch jobs to support certain features. To ensure that the batch jobs are running reliably, it is useful to have each batch job report to a central monitoring service, which can alert the team if the job doesn't run as expected, or if the job runs for too long. The team can then restart the job manually if it fails.

OGP uses [cronitor.io](https://cronitor.io/) for batch job monitoring.

## Creating Metrics

Metrics represent an aggregate measurement of resource usage that can be observed and collected throughout the system. These may be low-level measurements provided by the operating system, or higher-level application metrics that are tied to specific functionality.

### Start with available metrics first

Often, the easiest metrics to begin with are those already exposed by the cloud environment. For example, key server metrics such as CPU utilisation, load balancer target response time and HTTP 5XXs server errors are readily available for applications deployed to AWS Elastic Beanstalk. Integrating such metrics early can form a baseline to make sure that the most common issues can be detected and addressed before greater effort is expended to expand instrumentation for greater coverage.

### Expanding coverage with custom metrics

Custom metrics can be created to enhance monitoring coverage once the basic metrics are available. Application logs can be automatically parsed to create custom metric filters with cloud services (such as AWS CloudWatch) and automatically monitored to alert a notification pubsub service (such as AWS Simple Notification Service).

## Alerting

### Developing the alerting process

On call’s first notification of an incident comes through an alert, typically in the form of a message in a dedicated Slack channel or an automated phone call. Depending on how these are set up, alerts can become the best or worst friend of the on-call personnel. Whether an alerting system is helpful depends greatly on the team’s investment in effort on instrumentation and alert fine-tuning. If alerts are frequent and aren’t actionable, then time is persistently wasted on response and can exact a high toll on the team. Investing in the alert system is invaluable to maintaining a positive attitude and high level of morale.

#### Determining an alerting threshold

Determining the appropriate alerting threshold is an iterative process that depends on many external factors, such as the underlying frequency of data generation, the amount of users, etc. It is important to find the right balance between excessive false positives and suppressing valid alerts.

If the underlying metric is consistently available with well-defined minimum and maximum values, consider using a static threshold with sufficient margin to buffer for response time warning. For example, on a two-core server running a single-threaded NodeJS application, it is a good idea to set a 40% threshold (or even lower!) on overall CPU utilisation to detect resource saturation as the process exceeds the capabilities of one core (50%). On a database, it is possible to work out the amount of IOPS available in advance and set a threshold when exceeding it.

If the metric is consistently available, but without well-defined thresholds, consider using statistical alerts which allow setting thresholds using standard deviations, by treating the underlying metric as a time-series.

Some metrics can be fairly rare to obtain. For example, a specific exception may never be thrown until the unexpected failure of a usually-reliable external service. In such cases, a generic alarm for HTTP 5XX errors can be set. This relies on the server application to be resilient to unexpected exceptions (usually implemented with top-level error handling), so that a generic _HTTP 500 Internal Server Error_ can be returned and detected with an appropriate alarm.

Otherwise, it is generally a good idea to first start out at a conservative lower value, and fine-tune as necessary over time to arrive at an appropriate alarm threshold.

#### Alert Prioritisation

##### Critical Alerts

Critical alerts are _reactive_ alerts that indicate a worst case scenario which requires immediate response, such as one of the following:

1. Availability downtime
2. Cybersecurity alerts
   - High-severity AWS GuardDuty
   - HTTP 401 Unauthorized
   - HTTP 403 Forbidden

The alerts must be routed to the on-call service (such as Opsgenie or PagerDuty), and the on-call personnel must respond _as soon as possible_.

##### Error Alerts

Error alerts are reactive alerts that are less severe, and usually conveys that something is wrong or isn’t behaving normally, and warrants an immediate investigation. They may indicate service degradation or partial service outage, reflected by one of the following:

1. HTTP 500 Internal Server Error
2. Generic HTTP 5xx errors
3. HTTP 400 Bad Request
4. HTTP 404 Not Found
5. HTTP 422 Unprocessable Entity
6. Generic HTTP 4xx errors

This is not an exhaustive nor prescriptive list of alerts, nor are alerts limited only to HTTP statuses. Teams are encouraged to tailor their alerts to the needs of their specific application.

##### Warning Alerts

These are _pre-emptive_ alerts that indicate something _is happening_ or may be _about to happen_. Examples include:

1. Server CPU utilisation
2. Database CPU utilisation
3. Database IOPS thresholds
4. Database disk utilisation

In such cases, pre-emptive action (such as scaling the appropriate resource) can be taken to head off an impending service outage.

#### Alert Notification

Use the following guidelines to evaluate whether an alert is configured properly:

1. Every time an alert goes off, I should be able to react with a sense of urgency. I can only react with a sense of urgency a few times a day before I become fatigued.
2. Every alert should be actionable.
3. Pages should be about a novel problem or an event that hasn’t been seen before.

It is important to escalate an alert to an appropriate channel so that an incident can be responded to in a timely manner, and on-call personnel must acknowledge the alert.

##### Alert routing

Use the following table to decide where to route the alert:

<table>
  <tr>
   <td><strong>Alert Priority</strong>
   </td>
   <td><strong>Alerting Channel</strong>
   </td>
   <td><strong>Rationale</strong>
   </td>
  </tr>
  <tr>
   <td>Critical
<p>
(Availability)
   </td>
   <td>OpsGenie / PagerDuty
<p>
+
<p>
Team Slack channel
   </td>
   <td>Availability alerts indicate users are already affected, and requires the team to undertake immediate action for rectification.
   </td>
  </tr>
  <tr>
   <td>Critical \
(Security)
   </td>
   <td>OpsGenie / PagerDuty
<p>
+
<p>
Product-specific
<p>
Slack channel
   </td>
   <td>Security alerts indicate that a cybersecurity incident may be occuring and requires the team to undertake immediate investigation.
   </td>
  </tr>
  <tr>
   <td>Error
   </td>
   <td>Product-specific
<p>
Slack channel
   </td>
   <td>Error alerts could indicate service degradation, which may necessitate action for immediate rectification by the team.
   </td>
  </tr>
  <tr>
   <td>Warning
   </td>
   <td>Product-specific
<p>
Slack channel
   </td>
   <td>Warning alerts are pre-emptive alerts that indicate something <em>may be</em> <em>about</em> to <em>happen</em>. The team may be required to under pre-emptive action to avoid a full-blown incident.
   </td>
  </tr>
</table>


##### Escalation policy

Use the following table to configure an automated escalation policy on OpsGenie / PagerDuty:

<table>
  <tr>
   <td><strong>Minutes Since Alert Unacknowledged</strong>
   </td>
   <td><strong>Escalation To</strong>
   </td>
   <td><strong>Rationale</strong>
   </td>
  </tr>
  <tr>
   <td>0
   </td>
   <td>On-call personnel
   </td>
   <td>On-call personnel is responsible for responding to the alert in a timely manner
   </td>
  </tr>
  <tr>
   <td>5
   </td>
   <td>Project team
   </td>
   <td>Other team members are needed to step in to cover for the incident.
   </td>
  </tr>
  <tr>
   <td>10
   </td>
   <td>D/OGP
   </td>
   <td>There may be an ongoing incident which is not being addressed by the entire team.
   </td>
  </tr>
</table>

### Operational Dashboards

It is convenient to be able to obtain a common view of critical resource and application measurements that can be shared by team members for faster understanding and communication during an incident. Cloud services such as AWS Cloudwatch allow the creation of dashboards and widgets that can chart and present log queries in an easily consumable manner. Such dashboards can also be integrated with an operational playbook that provides guidance for team members about how to respond to specific incidents.
