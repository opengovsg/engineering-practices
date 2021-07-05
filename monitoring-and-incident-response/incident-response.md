# Incident response and on-calls

Rapid increases in the adoption of our software products and services is a key goal. As usage increases and software evolves, disruptions inevitably become more frequent. Effectively responding to these incidents is critical in ensuring ongoing acceptance of our products and upholding the team reputation and credibility.

## Definition of an incident

An incident is an event that requires an immediate emergency response, such as

1. Disruption or a reduction in the quality of a service
2. Data loss of any kind
3. Cybersecurity incidents

A good rule of thumb is to consider any incident that is security-related, or requires a _hotfix_ or _rollback_.

Examples of security incidents include

1. triggering of security-related alerts (HTTP 401 Unauthorised / 403 Forbidden)
2. suspected cyberattacks such as denial-of-service, and
3. security bugs reported by cybersecurity professionals.

For the avoidance of doubt, scanning or fuzzing activity that merely triggers HTTP 404 Not Found alerts that do not impact either system uptime nor result in successful malicious traffic, should not count as cybersecurity incidents.

## Scheduling on-calls

The number of available team members dictates the approach to on-call scheduling. The number of available individuals impacts how you address issues of daytime and nighttime coverage and ways to strike a balance between on-call obligations and the individual team members’ personal time.

In smaller teams, product managers and core engineers often take all of the on-call shifts themselves. They know the entire system well so they can triage and fix problems on their own.

When there are two people available you can schedule weekly rotations with each alternating full weeks. Being on call and dealing with alerts for an entire week, however, can be exhausting.

Scheduling becomes easier with three or more team members participating in rotations, allowing for periods of rest and recovery. More team members also allows for other shifting alternatives and even more flexibility.

It is important to enable teams to have feedback into the scheduling model so you choose the one that works best for the individual members of the team. Technical leads should have enough freedom to set up schedules that allow team members to attend to family needs, take care of their health, or pursue training in their off-hours and to make needed adjustments to maintain a positive attitude and high level of morale.

Do also note that the on-call engineer and incident manager are roles - it can be fulfilled by the same person, especially for smaller teams. You may wish to discuss with your team to set up an on-call schedule that works well for everyone, as this guide is not prescriptive about that. For example, the team may decide that it is not necessary to have a dedicated incident manager on all days as release days are typically riskier.

## Communication during an incident

1. Have we described the actual impact on customers?
2. Did we say how many internal and external customers are affected?
3. If the root cause is known, what is it?
4. If there is an ETA for restoration, what is it?
5. When & where will the next update be?

## Roles and responsibilities

### On-call Engineer

#### Pre-incident

1. Ensure preferred notification channels are open and operative
2. Ensure access to devices needed to respond to alerts such as laptop and mobile phone with Slack (carry these with you outside if you have to)
3. Ensure reliable internet connectivity
4. No alcohol

#### During incident

1. Acknowledge the alert
2. Determine the urgency of the alert and verify that there is an incident
3. Immediately escalate the incident to team members, D/OGP, GITSIR ([gitsir@tech.gov.sg](mailto:gitsir@tech.gov.sg)) and GIROC ([GIROC_Reporting@tech.gov.sg](mailto:GIROC_Reporting@tech.gov.sg)) once an incident is confirmed
4. Determine and execute a recovery plan
    1. Rollback (preferred) or hotfix
    2. Scale resource bottleneck
5. Log important actions undertaken

#### Post incident

1. Verify issue is resolved in production through end-to-end checks
2. Conduct and document postmortems
3. Create & assign tickets where necessary
4. Update documents and runbooks to keep them up-to-date

### Incident Manager

#### Pre-incident

1. Ensure preferred notification channels are open and operative
    1. Internal Slack channels for team members
    2. Workplace, email and in-app banners for users
2. Ensure access to devices needed to respond to alerts such as laptop and mobile phone with Slack (carry these with you outside if you have to)
3. Ensure reliable internet connectivity
4. No alcohol

#### During incident

1. Acknowledge the alert
2. Collaborate with team to determine the urgency of the alert
3. Immediately escalate the incident to team members, D/OGP, GITSIR ([gitsir@tech.gov.sg](mailto:gitsir@tech.gov.sg)) and GIROC ([GIROC_Reporting@tech.gov.sg](mailto:GIROC_Reporting@tech.gov.sg)) once an incident is confirmed
4. Inform affected users and stakeholders of the issue that the team is investigating and will update them soon via appropriate communication channels
5. Log important actions undertaken
6. Communicate periodically (every 15-30mins) to keep internal and external stakeholders informed until the incident has been resolved

#### Post incident

1. Conduct and document postmortems
2. Share post mortems with GITSIR, GIROC and affected users

### RACI Matrix for Incident Response

**R** - Responsible	**A** - Accountable	**C** - Consulted	**I** - Informed

<table>
  <tr>
   <td><strong>Incident Stage</strong>
   </td>
   <td><strong>Incident Manager</strong>
   </td>
   <td><strong>On-call Engineer</strong>
   </td>
   <td><strong>D/OGP</strong>
   </td>
   <td><strong>GITSIR/GIROC</strong>
   </td>
  </tr>
  <tr>
   <td>Acknowledge alert & confirm incident
   </td>
   <td>R
   </td>
   <td>R
   </td>
   <td>A
   </td>
   <td>-
   </td>
  </tr>
  <tr>
   <td>Escalate incident
   </td>
   <td>R
   </td>
   <td>R
   </td>
   <td>A
   </td>
   <td>I
   </td>
  </tr>
  <tr>
   <td>Assess impact
   </td>
   <td>A
   </td>
   <td>R
   </td>
   <td>I
   </td>
   <td>I
   </td>
  </tr>
  <tr>
   <td>Inform users
   </td>
   <td>R
   </td>
   <td>C
   </td>
   <td>A
   </td>
   <td>I
   </td>
  </tr>
  <tr>
   <td>Execute recovery
   </td>
   <td>A
   </td>
   <td>R
   </td>
   <td>I
   </td>
   <td>I
   </td>
  </tr>
  <tr>
   <td>Communicate periodically
   </td>
   <td>R
   </td>
   <td>C
   </td>
   <td>A
   </td>
   <td>I
   </td>
  </tr>
  <tr>
   <td>Verify recovery
   </td>
   <td>A
   </td>
   <td>R
   </td>
   <td>I
   </td>
   <td>I
   </td>
  </tr>
  <tr>
   <td>Conduct & share postmortem
   </td>
   <td>A
   </td>
   <td>R
   </td>
   <td>I
   </td>
   <td>I
   </td>
  </tr>
</table>

## Learning through experience

### Incident response runbook

A runbook is the documented form of a team’s procedures for conducting a task or series of tasks. When an incident is detected, containing the event and returning to a known good state are important elements of a response plan. The remediation might be as simple as removing the variance through a redeployment of the resources with the proper configuration. To do this, each team should plan ahead and define their own response procedures, which are often called runbooks. These runbooks should be maintained and improved upon by the project team over time as the application functionality grows and new incidents uncovered.

### Blameless postmortems

A postmortem is a written record of an incident, its impact, the actions taken to mitigate or resolve it, the root cause(s), and planned follow-up actions to prevent the incident from recurring.

Writing a postmortem is not punishment—it is a learning opportunity for the entire team. 

For a postmortem to be truly blameless, it must focus on identifying the contributing causes of the incident without indicting any individual or team for bad or inappropriate behavior. A blamelessly written postmortem assumes that everyone involved in an incident had good intentions and did the right thing with the information they had. If a culture of finger pointing and shaming individuals or teams for doing the "wrong" thing prevails, people will not bring issues to light for fear of punishment.

### Incident Tracking

Incident tracking aims to improve stability and reduce the number of incidents in the long-term. Key Performance Indicators to be measured month-on-month are \

1. Number of incidents with root cause attributable to the project team
2. Average time to incident detection
3. Average time to mitigation from detection
4. Average time to recovery from detection

## Appendix - Application Rollbacks vs Hotfix

The highest priority in an incident is to restore service. Maintaining availability and quality of service is more important than understanding the root cause of the issue.

### Rolling back the application

When possible, this should always be the preferred option as it offers the fastest route to service recovery.

A rollback can be performed when these conditions have been met:

* The last state of the system is verified and known to be in working order. This is not always true.
* The failure is caused by a change that can be rolled back with no problems of backwards compatibility

#### Types of rollbacks

##### Full rollback

This is the fastest and safest since no new code needs to be written and tested. This can be performed by deploying the last known build artifact (e.g. Docker image) to the deployment infrastructure. However, this requires backwards compatibility between the current and previous release versions.

##### Partial rollback

This is similar but is still safer compared to a hotfix. A partial rollback is performed by reverting the specific Git commit that is problematic, and then re-deploying the application through the usual CI/CD pipeline.

#### Examples of when rolling back is not possible

* There is a security bug with login, and the code was written over a year ago
* A change to the API broke backwards compatibility, and there are both old and new clients released in the wild
* A new feature which users are reliant on required the addition of a new field to the database schema, and is not backwards compatible

### Hotfix

In case the application cannot be rolled back, a hotfix will have to be written.

Fixing forward has significant downsides:

* Hotfixes are usually rushed out during high stress situations, and code written in haste usually escapes the scrutiny and testing expected of regular changes
* The point fix usually incurs significant system entropy and technical debt
* There is a possibility of breaking something else