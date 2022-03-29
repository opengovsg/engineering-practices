# Release practices

## Standard releases

We adhere to the following production release practices:

1. Only run production modifications during office hours (unless hotfixing or some other form of emergency)
2. Inform the project team via the appropriate Slack project channel

## High-risk releases

Higher-risk changes, and releases with planned of downtime, may be scheduled to run when the number of users interacting with the system is low (late in the night, very early in the morning, or in weekends). Such releases typically include (but is not limited to):

1. Infrastructure migrations or modifications
2. Database migrations

For such changes, teams should produce a migration plan before the change, and have at least two pairs of eyes when making any modifications to production infrastructure.
