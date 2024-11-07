# [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

Use Conventional Commits to track what types of changes are being made.

This will ensure that all commits in this project adhere to a standardized format,
which will help streamline collaboration and automate release workflows.

With this well-formed, machine readable commit information, tools can derive useful
human-readable information for releases of your project.

This guide integrates with [commitlint](https://commitlint.js.org) to:

1. Add a git commit-msg hook to validate commit messages as they are made
2. Add a GitHub workflow to validate that all commits in a PR conform to conventional
   commits

To get the most out of conventional commits two configuration changes should be made
to your GitHub project:

1. The GitHub branching rules should be changed to require that the conventional
   commit workflow succeeds before PRs can be merged. This ensures that all developer
   commits are conventional commits.

2. The GitHub merging rules should be changed to ensure that ONLY rebase merges are
   allowed. This ensures that unvalidated commit messages from merge commits or sqash
   commits are not added to the release branch.

TODO