---
title: "Commit-Sign-Offs"
description: ""
toc: false
---

For all O3DE and related code, commits must be signed off before they are mergeable as a codebase requirement from the Linux foundation (see [this SO post for more details](https://stackoverflow.com/questions/1962094/what-is-the-sign-off-feature-in-git-for)). To sign off a commit, simply add the `-s` command-line option to the `git commit` command. This will add a line like:

```
Signed-off-by: Humpty Dumpty <humpty.dumpty@example.com>
```

in the commit body. If you forget to do this, you can remedy things by amending the commit message of each commit missing the sign off. There are many mechanisms to do this, but the simplest way is to rebase the range of commits affected and force push your branch to your fork. The failed DCO instructions explain how to do this, and under what conditions the batch sign-off correction using rebase is possible. These instructions are reproduced below:

```
Rebase the branch
If you have a local git environment and meet the criteria below, one option is to rebase the branch and add your Signed-off-by lines in the new commits.
Please note that if others have already begun work based upon the commits in this branch, this solution will rewrite history and may cause serious issues
for collaborators ([described in the git documentation](https://git-scm.com/book/en/v2/Git-Branching-Rebasing) under "The Perils of Rebasing").

You should only do this if:

You are the only author of the commits in this branch
You are absolutely certain nobody else is doing any work based upon this branch
There are no empty commits in the branch (for example, a DCO Remediation Commit which was added using --allow-empty)
To add your Signed-off-by line to every commit in this branch:

Ensure you have a local copy of your branch by [checking out the pull request locally via command line](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/checking-out-pull-requests-locally).
In your local branch, run: git rebase HEAD~10 --signoff
Force push your changes to overwrite the branch: git push --force-with-lease origin class_inheritance
```
