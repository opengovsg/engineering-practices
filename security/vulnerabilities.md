# Vulnerability detection and remediation

## Penetration testing

With the exception of *pilot projects*, each OGP project team is expected to procure and arrange for an annual penetration test for their production systems.

### Pentest remediation standards

OGP engineers are expected to adhere to the following standards for penetration testing findings:

- Critical/High - issues to be *remediated* **immediately**
- Medium/Low - issues to be *remediated* within 7 calendar days

where *remediate* means to have deployed the necessary fixes into production.

If the issues cannot be remediated in time, documentation should be produced to state why (e.g. mobile release cycle).

## Static Analysis Security Testing (SAST) and dependency upgrades

OGP uses [Snyk.io](https://snyk.io) for static analysis and security testing. Each product team should import their GitHub repositories into Snyk for application testing.

### SAST remediation standards

OGP engineers are expected to adhere to the following standards for static analysis and dependency upgrades:

- Critical/High - issues to be *triaged* within 7 calendar days
- Medium/Low - issues to be *triaged* within 14 calendar days

where *triage* means to make an impact assessment, file an issue on the project board, or flagged as a false positive.

## Explanation on remediation standards

Remediation standards differ between penetration tests and SAST. Notably, patches for code dependencies and OS-level upgrades depend on open source or external contributors, therefore it does not make sense to impose timelines for remediation, only for triaging. However, pentest findings are almost always severe in impact when exploited, as well as actionable by OGP engineers, therefore remediation timelines are shorter and explicit.

|                               | Penetration tests                                           | Static Analysis and Security Testing                  |
|------------------------------ |------------------------------------------------------------ |------------------------------------------------------ |
| Cost                          | Expensive                                                   | Cheap                                                 |
| Frequency                     | Annual                                                      | Daily / Every pull request                            |
| Source of vulnerability       | Our own code                                                | Open source                                           |
| Feasibility of attack vector  | Demonstrable                                                | Requires further analysis                             |
| Impact of exploit             | Demonstrable damage to public or OGP                        | Not demonstrated, frequent false positives            |
| Patch availability            | Written by OGP engineers                                    | Depends on supply chain                               |
| Severity rating               | Based on demonstrable and practical exploits by pentesters  | Theoretical CVSS rating by cybersecurity researchers  |
