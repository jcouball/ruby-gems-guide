# [Create GitHub Release](https://github.com/main-branch/create_github_release)

When run in your gem's git worktree, the create-github-release script does the following:

* bumps the gem's version following SemVer,
* updates the gems's changelog,
* creates a new release branch and release tag,
* commits the version and changelog changes to the release branch,
* pushes these changes to GitHub and creates a PR to merge the release branch to the default branch

TODO
