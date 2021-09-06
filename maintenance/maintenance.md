# Maintenance guidelines

Out of all the phases in a software development lifecycle, the maintenance phase is the longest. As OGP matures, there will be an increasingly long tail of projects that we will need to maintain in order to ensure application functionality and uphold security for our users.

## Documentation

The following documentation should exist for each project as it transits into maintenance

- Architecture diagram
- Build and release guide
- Checklist of smoke tests
- Incident response runbook

Having these documentation helps to maintain good knowledge management, which is crucially important in light of inevitable personnel changes over time.

They are also helpful to get new engineers onboard.

## Test coverage

Ideally, maintenance projects should have ample test coverage (unit, integration etc.), especially around key functionality offered by the application. Tests not only serves as a form of documentation, but also protects future engineers who may not be familiar with the application from accidentally introducing breaking changes. Having sufficient tests can help to offer a baseline level of protection and feedback, making dependency upgrades safer, especially if a key security flaw needs to be patched urgently. Of course, having ample tests also has the additional benefit of making feature development and refactoring faster and safer.

## Security and on-call

Maintenance projects should continue to uphold standards for security vulnerabilities remediation. For more information, see the guide on [vulnerability detection and remediation](../security/vulnerabilities.md).

On-call and incident response expectations do not change for maintenance projects. For more information, see the [incident response guide](../monitoring-and-incident-response/incident-response.md).

## Key person risk

To mitigate [key person risk](https://en.wikipedia.org/wiki/Key_person_insurance), we should strive to have at least 2 maintainers per project to provide a minimum amount of [bus factor](https://en.wikipedia.org/wiki/Bus_factor).

## Periodic review

The maintenance team should convene and review the state of the current project once per quarter, and evaluate whether any actions are required along the following axes:

1. Security vulnerabilities
2. Application code & dependencies
3. Infrastructure & other external environment changes
4. Documentation

Doing this quarterly should help to strike a good balance between effort required and avoiding the deleterious effects of [software rot](https://en.wikipedia.org/wiki/Software_rot).
