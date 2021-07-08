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

## Writing and Reviewing Pull Requests

- Make use of the following features:
  - [Closing issues via Pull Requests](https://help.github.com/en/articles/closing-issues-using-keywords)
  - [Referencing commits, issues and other repos](https://help.github.com/en/articles/autolinked-references-and-urls)
  - [Referencing repository code](https://help.github.com/en/articles/creating-a-permanent-link-to-a-code-snippet)
  - [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown)
  - [Marking duplicates](https://help.github.com/en/articles/about-duplicate-issues-and-pull-requests)
