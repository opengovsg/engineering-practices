# Branching

Branches are used routinely, to allow engineers to undertake work
independently of each other, and to manage the releasing of code into
production. This guide describes OGP's approach to branching that we
aim to make consistent across our repositories.

## Branch naming

- Branch names should be kebab-case (lowercase only, - word separator)

- Default and release branches should have the following names:
  - `develop` - mainline branch, used to hold all developer changes
  - `release` - used for releases into production

- Branches for work-in-progress should follow an approach similar to
  [conventional commits](https://www.conventionalcommits.org/).
  - the format should be `<type>[/<scope>]/branch-name`
  - examples are `feat/more-cowbell` and `fix/auth/schema-change`

- Branches for releases should take the format `release-<version>`

- Separate branches should be created for non-production environments.
  Ad-hoc releases can then be made from any arbitrary branch or commit
  via git force push, ie `git push -f origin <source-branch>:staging`
  - `staging` should be created since most teams maintain a staging
    environment for pre-release testing

## GitHub branch protection rules

- Branch protection rules should be in place for `develop` and `release`
  - Require reviews and status checks to pass before merging
  - Pushing to branch can be restricted to certain teams, users 
    and bots, if desired
  - Prevent force pushes to `release`

## Branching strategy

- Changes in mainline branch should be released as follows:
  - create a branch from `staging` named `release-<version>`
  - run `npm version <patch|minor>` on the newly-created branch
  - run `git push -f origin release-<version>:staging`
  - raise a PR merging `release-<version>` into `release` 

- Feature branches should hang off and merge into the mainline branch
  - Favour rebasing over merging to update feature branches
  - Keep feature branches **small**. This makes the PR easier to understand,
    and also makes it easier to rebase the feature branch on mainline should
    the need arise
  - If implementing a feature will take too many commits or too much change, 
    you might be undertaking too much work in one go. If so, split the work
    into more manageable chunks

- Hotfixes and trivial changes can be squashed as a single commit onto mainline

## Further reading

See Atlassian's article on [Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), on which our strategy is loosely based on
