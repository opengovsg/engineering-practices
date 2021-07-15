# Commits and Pull Requests

This guide details how git commits and pull requests should be written;
combined, they lead to a well-maintained revision history within the
codebase.

## Writing commits

OGP follows [conventional commits](https://www.conventionalcommits.org/)
to write consistent commit messages. This allows the commit history to
be easily understood, particularly when using `git log --oneline`:

```
$ git log --oneline -n 6
b0ef8790 (origin/develop, origin/HEAD) refactor(sgid): use httpOnly cookies, inject sgid config directly
876fe7a9 fix(sgid): rework codebase in-line with review
db1e4857 feat(sgid): guard with beta flag, reword text
cf12c150 test(sgid): provide test coverage
34a9b6fa feat(auth): enable sgID
86f709f8 feat: Set SP/CP JWT cookie to HttpOnly (#2193)
```

This also allows us to automatically generate CHANGELOGs, and consider
further automation of releases in future.

Contributors who are unfamiliar with conventional commits may invoke
[commitizen](http://commitizen.github.io/cz-cli/) via `npx cz` or
`npx git-cz`, or `npm run cz` on the OGP Application Template. This
provides a series of prompts to write the commit message

[How to Write a Git Commit](https://chris.beams.io/posts/git-commit/)
by Chris Beams is recommended reading; it describes how to write 
comprehensive git commit messages that contribute to a healthy revision
history, allowing someone to understand the motivations and nature of
changes in the codebase easily.


## Opening Pull Requests

### PR Description

Pull Requests (PR) are the mechanism through which changes to projects gets reviewed by team members for approval before being incorporated in the codebase.

Writing a good PR description is a critical aspect of software change and historical tracking.

PRs should give all the context a reviewer needs to understand:
- why the PR exists
- the approach taken to solve the issue

The PR description should be generous in the information it provides to help reviewers.
- link to an RFC where other approaches were discussed
- link to compliance policies
- link to issue in github (ideally all PRs relate, either in part or in full, to an issue)
- link to documentation of esoteric API calls being used

Even when reviewers are expected to be from the team, and therefore be familiar with context at that point in time, the PR description should stand on its own from an informational perspective. Doing so ensures:
- The PRs are understandable by other reviewers (e.g. new joiners, other team members)
- The context will be understandable by your future self (do not underestimate the mind's ability to forget details!)
- The PR description can be used in other processes (e.g. risk assessment for deployments, audits, incident response)

To faciliate writing consistent PRs descriptions, all repositories should have an up to date [PR template](https://docs.github.com/es/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository). PRs should have the following sections:
- Problem
- Solution
- Tests
- Risks

The Risk section is one of the most important part of the PR. The PR owner indicates therein he/she has done the required thinking of what could go wrong (in term of best/worst case) when the code is live, how errors might cascade through the system, and how likely it really is to happen. Risk is a subjective assessment, and obviously, no one opens PRs thinking things are likely to break. Still, all changes carry risk, and engineers are notoriously bad at evaluating risks. The risk section is a reminder for the PR owner to think about risks. It also helps reviewers validate the thinking, and comment with their own evaluation if necessary.

Risks typically is a combination of 2 factors:
- **Impact**: what is changed, how many systems or sub systems can thus potentially be affected? Are these system critical? 
  - e.g. "change to the login system": `High`; vs. "change" css font styles on the 'About the team' page: `Low`
- **Likelihood**: does the change concern area with high fluctuations of input or behaviour?

Finally, do follow this guidelines:
  - [Closing issues via Pull Requests](https://help.github.com/en/articles/closing-issues-using-keywords)
  - [Referencing commits, issues and other repos](https://help.github.com/en/articles/autolinked-references-and-urls)
  - [Referencing repository code](https://help.github.com/en/articles/creating-a-permanent-link-to-a-code-snippet)
  - [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown)
  - [Marking duplicates](https://help.github.com/en/articles/about-duplicate-issues-and-pull-requests)


### Content and Requirements

All PRs should include tests. If no tests are present on purpose, the PR description should indicate why none were needed.

Repositories should ideally have explicit code coverage targets, and a policy that no PR reduces coverage. It is recommended that the coverage level be an automatically assessed criteria for a PR to be mergeable (i.e. PR which cause coverage to drop are blocked).

### Draft PRs

Once a PR is opened, it is expected to be reviewed in a timely manner. Engineers should proactively check the repositories their teams own, and contribute PR reviews before starting entirely new work.

Because reviews can take a considerable amount of time, and because engineers are notified when PRs are opened, PR owners should be considerate when opening PRs. PRs should only be opened when the typical criteria have been met:
- PR Description is complete
- Changeset is complete
- Tests are present
- Functional tests have been performed locally

That being said, it is sometimes needed to get feedback early. For these cases, PRs may be opened in an incomplete state, provided they meet the following requirements:
1. They are opened as [Draft PRs](https://github.blog/2019-02-14-introducing-draft-pull-requests/)
2. The PR title includes a leading string `[WIP] ` in the PR title
3. [Optional] Add a label `wip`

Draft PRs are not expected to receive proactive reviews from engineers. Instead the PR owner may seek feedback from specific folks as needed.

When the PR is ready for review, the PR owner must transition it to **Ready for Review**, and remove the leading `[WIP]` string from the PR title.

### Closing PRs

All PRs at OGP require (at least) 1 approval to be merged.

Teams should strive to close PRs in a timely manner. Teams should ideally have an SLA policy for review time and merging. When a repository has many opened PRs, it causes context switching overheads to engineers.

PR owners are responsible for merging their PRs. If a PR is not getting attention from the team, it is up to the PR owner to ask for reviews and approvals. 

It is recommended that all repositories adopt a "Stale-PR" policy. PRs which see no activity and are not getting merged should be closed automatically.

If priorities shift, and a PR cannot get the reviews it requires, the PR owner is encouraged to close the PR proactively, and re-open it later, when the team has bandwidth to tend to the work.


## Reviewing Pull Requests

All engineers are required to review PRs as part of their duties. 

### Purpose

PR reviews foster discussions on implementation details, algorithms, feature sets, edge cases, etc. They are an excellent avenue for learning for everyone involved and, can help surface potential issues. Review comments typically touch on subjects such as:
- Feedback on code structure, abstractions, encapsulation, testability, etc.
- Suggest alternative APIs or libraries
- Identify edge cases
- Gain concensus on structure

Code is read much more than it is written. PR reviews ensure that changes are legible by multiple people early, before they are integrated into the code base.

### Manners

PR reviews are done over comment threads in github. As asynchronous discussions, PR comment threads can cause anxiety for both PR owners and reviewers when disagrements surface.

All participants in PR reviews should maintain a level head, and abide by some simple rules of etiquette:
- Assume good intent
- Always be respectful, polite, and pleasant
- Favour questions over statements
- Don't jump to conclusions / criticism
- Be generous in your explanations and background details when proposing alternatives

### Content

Where possible, when a reviewer has an idea for an alternative implementation, he/she should provide some sample code to explain the different approach (use github's [code suggestion](https://docs.github.com/en/github/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/commenting-on-a-pull-request#adding-line-comments-to-a-pull-request) feature for that)

Discussions on code styles preferences are typically not productive in PRs and are discouraged. To make this a non-issue, At OGP, repositories should up opinionated code formatters, which will help to:
1. Ensure consistent coding style across repos
2. Eliminate discussions on syntax and style in PR review (because that's not useful!)

### WorkFlow

PR owners are notified of comments as they happen, and this is noisy when a slow trickle of comments is submitted. To prevent that, reviewers should submit all of their comments in one go by using github's review mechanism. This helps reviewers themselves, since a comment or question they had early on may answer itself as they progress through the PR review.

When submitting a review, github classification should be use with care by reviewers, as follows:
- `Comment`: You have questions or comments and are thus so far non-committal. Should someone else were to approve the PR, you would be generally OK with the PR being merged as-is
- `Approve`: You agree with the changeset in the PR, and stand by it as if you had written the code yourself
- `Request changes`:  You have spotted serious flaws in the PR that must be addressed. PR must not be merged in its current state

The PR owner is expected to answer questions and comments in the PR. When a particular PR comment thread is concluded to the satisfaction of all involved, the thread should be marked resolved through the button `Resolve Conversation`.


## Merging Pull Requests

PRs at OGP require at least 1 approval in order to be merged.

In case of contributions from open source contributors, or contributions from egineers not members of the team owning the repo, one approval from a member of the owning team should be gathered.

When Merging PRs into the develop branch. Use github's "Squash and merge" strategy, as it makes reverts trivial.
