# Accessing production systems

Occasionally, team members will need to access running systems such as production servers or databases.

In these situations, production access should be secure

1. Prefer IAM access over SSH keys when possible (e.g. AWS Systems Manager)
2. Remove whitelisted IPs in network firewalls (e.g. security groups) periodically

Do not keep production credentials in your local development environment longer than is required.
