# Branching

Branches are used routinely, to allow engineers to undertake work
independently of each other, and to manage the releasing of code into
production. This guide describes OGP's approach to branching that we
aim to make consistent across our repositories.

## Branch naming

The default branches should have the following names:

- `develop` - mainline branch, used to hold all developer changes
- `staging` - used for deploying into staging for pre-release testing
- `uat` (optional) - used for deploying into UAT environments for user testing
- `release` - used for releases into production
- `master` - branch for stable releases

Branches names should be kebab-case and follow an approach similar to
  [conventional commits](https://www.conventionalcommits.org/):

- the format should be `<type>[/<scope>]/branch-name`
- examples are `feat/more-cowbell`, `fix/auth/schema-change`

Branches for releases should take the format `release-<version>`

- Separate branches should be created for non-production environments.
  Ad-hoc releases can then be made from any arbitrary branch or commit
  via git force push, ie `git push -f origin <source-branch>:staging`

## GitHub branch protection rules

Branch protection rules should be in place.

For `develop`:

- Require reviews and status checks to pass before merging
- Pushing to branch should be restricted to certain teams/users, and bots
- Prevent force pushes

For `release`:

- Require reviews and status checks to pass before merging
- Pushing to branch should be restricted to certain teams/users only
- Prevent force pushes
- Dismiss stale pull request approvals when new commits are pushed

## Branching strategy

### Releases

Changes in mainline branch should be released as follows:

- check out a new release branch from `develop` named `release-<version>`
- bump the ([semantic](https://semver.org/)) version by running `npm version <major|minor|patch>`
  on the newly-created branch
- push out the change to staging for [smoke testing](https://en.wikipedia.org/wiki/Smoke_testing_(software))
  with `git push -f origin release-<version>:staging`
- raise a PR _merging_ `release-<version>` into `release`
- raise a PR _merging_ `release-<version>` into `develop`
- if a bug is discovered during smoke testing, a fix can be raised against this release branch
  and picked up subsequently in both `develop` and `release`

### Hotfixes

Hotfixes should be merged as follows:

- check out a new patch branch from the previous release branch, also named `release-<version>`
- write and verify a hotfix in development
- bump the patch version by running `npm version patch` on the newly-created branch
- push out the change to staging for smoke testing with `git push -f origin release-<version>:staging`
- raise a PR _merging_ `release-<version>` into `release`
- raise a PR _merging_ `release-<version>` into `develop`

## Tips

Feature branches should hang off and merge into the mainline branch.

- Favour rebasing over merging to update feature branches
- Keep feature branches **small**. This makes the PR easier to understand,
  and also makes it easier to rebase the feature branch on mainline should
  the need arise.
- If implementing a feature will take too many commits or too much change,
  you might be undertaking too much work in one go. If so, split the work
  into more manageable chunks.

## Further reading

See Atlassian's article on [Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), on which our strategy is loosely based on.
