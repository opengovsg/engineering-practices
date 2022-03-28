# Accessing production systems

Occasionally, team members will require access to production systems such as servers or databases.

In these situations, the technical leads should periodically review (at least once every six months) production access practices to ensure that their team's access practices are secure:

1. Remove unnecessary whitelisted IP addresses in network firewalls (e.g. security groups)
2. Avoid storing production credentials in a local development environment.
3. Prefer IAM access over SSH keys when possible (e.g. AWS Systems Manager).
4. Where connecting to private subnets, consider establishing AWS VPN as the preferred method of access.
