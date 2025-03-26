---
title: "How-to-merge-from-stabilization-branches-to-main-for-release"
description: ""
toc: false
---

This document helps the maintainer that is tasked with preparing a PR that will merge stabilization or point release into the `main` branch for a release.
The goal is to prepare a PR which, if submitted, makes the resulting `main` branch, identical to the stabilization of point release branch, on release day.

## Table of Contents
* [When to execute](https://github.com/o3de/o3de/wiki/How-to-merge-from-stabilization-branches-to-main-for-release#when-to-execute)
* [The Goal](https://github.com/o3de/o3de/wiki/How-to-merge-from-stabilization-branches-to-main-for-release#the-goal)
* [Making the PR Itself](https://github.com/o3de/o3de/wiki/How-to-merge-from-stabilization-branches-to-main-for-release#making-the-pr-itself)
* [How someone else can verify it](https://github.com/o3de/o3de/wiki/How-to-merge-from-stabilization-branches-to-main-for-release#how-someone-else-can-verify-it)
* [How to update the existing merge PR](https://github.com/o3de/o3de/wiki/How-to-merge-from-stabilization-branches-to-main-for-release#how-to-update-the-existing-merge-pr)
* [Release day](https://github.com/o3de/o3de/wiki/How-to-merge-from-stabilization-branches-to-main-for-release#release-day)
* [Merging from Point Release to main](https://github.com/o3de/o3de/wiki/How-to-merge-from-stabilization-branches-to-main-for-release#merging-from-point-release-to-main)

## When to execute

This outline steps through a process which should only be done during a release phase, and CAN only be done with someone with permissions to merge branches into main, which is going to be people primarily involved in release.  Creating the PR should happen as far ahead of time as possible, and the PR should be tested and AR'd.  You may need a DCO punch thru from someone who has permission.

To be more specific, anyone with any role can make the PR, but it takes a release maintainer to actually push the merge button and accept it.  So the task of creating the PR can be delegated, but not the task of final submission.

## The Goal
The goal would be to create a merge PR, which, if submitted, makes the `main` branch identical to the current stabilization branch.  It can be verified as such by a third party.

## Making the PR itself

Anyone can do this step, no special permissions are required.

1.  You will need a fork of each repo in your own github account and the appropriate tokens / credentials to write to your own account.  You do not need write access to O3DE.  You can do all this using the GitHub website GUIs. 
2.  Ensure your remotes are set up correctly.  I will assume in all following that `upstream` is the real, actual o3de repo, and `origin` is your own private fork.

```
git remote -v
```

Example output:
```
origin  https://github.com/nick-l-o3de/o3de (fetch)
origin  https://github.com/nick-l-o3de/o3de (push)
upstream        https://github.com/o3de/o3de.git (fetch)
upstream        DO_NOT_PUSH (push)
```

if `upstream` is not the official repo (for example, `o3de/o3de.git`) you can use `git remote rename <OLDNAME> upstream` to rename it to `upstream`.
If `upstream` or `origin` is missing, you can use `git remote add <name> <url>` for example, `git remote add origin https://github.com/my-account/my-fork-name.git`
In addition, you can ensure that you will never push directly to upstream (if you have the ability to do so) by replacing the push URL for upstream with a nonsense URL that does not exist, like `git remote set-url --push upstream DO_NOT_PUSH`

3.  Get latest

``` 
git fetch upstream
git fetch origin
```

4.  Switch to a new branch from `main`.   Name it appropriately.
```
git switch -c merging_stabilization_2409_to_main upstream/main
```

5.  Merge stabilization into main tentatively.   Change the source branch (last parameter) as appropriate
``` 
git merge --no-ff -X theirs --no-commit upstream/stabilization/2409
```

If all went well, no messages will be output and the merge is ready.  But you should still check if there are any issues, so do the following steps.

6.  Fix merge issues.
Remember, the goal here is that the final merge should make `main` look literally exactly like stabilization, in git language this would mean that 
```
git diff --staged upstream/stabilization/2409
```
should output no diffs.
There isn't a lot of decisions to make here.  We want to discard anything that doesn't match stabilization.
So for each different possible merge conflict (if any show up), solve it exactly like this.
Start by issuing a `git status` and then follow these guidelines:
 a)  When the merge says the conflict is because it was 'modified by both', accept 'theirs' (the incoming change) and discard any of 'our' changes.  You can navigate to that branch in the github web site and look at the file if you get confused.
 b)  when the merge says the conflict is because it was 'deleted by them' (ie, it was deleted in stabilization) issue a `git rm <path>` to ack the delete.
 c)  when the merge says 'added by them', issue a `git add <path>` to ack the add

Once you're done here, there is the final step, to make it exact

7.  Fix accidental merges
```
git diff --staged upstream/stabilization/2409
```
Should show no changes.  You are done with this step when it does.
If it shows changes still, it means that Git merged something it should not have, and the code probably won't compile.
Fix it.
For each file that has a diff, we want it to be exactly like stabilization, so do this for each such file:
```
git checkout upstream/stabilization/2409 -- <full path to file that showed up in the diff>
```
You can continue to issue `git diff --staged upstream/stabilization/2409` periodically until no diffs show up
Having this show no output literally means that someone grabbing this PR or merging it would have no differences between stabilization, which is the goal here.

Once ready, you can make your commit.
```
git commit --signoff
```
Use an appropriate commit message

8. Upload the commit to your fork.
```
git push origin
```
This will open a clickable URL to merge it, but you can also manually create one on github.com/o3de/o3de

9. Open github, and create a PR to merge it into `main`.    Note that the default for github is not `main` so the first thing you should change is that you are merging into main.

10.  Open the PR, it is ready

## How someone else can verify it

1.  Open the created merge PR in github and note its repository source and name near the top
2.  Go to the github compare tool ** for the repo that the PR is targetted against**, for example

https://github.com/o3de/o3de/compare/ for the o3de repo

https://github.com/o3de/o3de-extras/compare/ for the o3de-extras repo, etc.

3. click "compare across forks"

4. on the left hand side, choose the base repo 'stabilization branch' so for example o3de's stabilization/2409 branch
5. on the right side, choose the PR that will be committed that contains the changes, see below

![image](https://github.com/user-attachments/assets/26923eaa-e31d-4265-b146-d05301bd7264)

You should see that 0 files are going to be changed, meaning that if we commit the PR, its results are identical to stabilization/2409

## How to update the existing merge PR
If more commits appear after you make it, you can issue the same commands over the top of your existing merge, starting from the "get latest" step.  it will update the existing merge and roll it forward.

## Release day

On release day, someone with access to the repos, will need to accept the PR, by merging it by clicking the merge button.

It is preferred to run the PR through the AR process, so that no backdoors/break glass red buttons have to pushed to force it through.  You will probably have to get a DCO pass though.

## Merging from Point Release to main

Its the same process, except when you do the `git merge` command, instead you should try first by removing the `--no-ff` flag.  This flag prevents it from doing a fast-forward.  However, when it comes to a point release, the point release will be identical to the previous main release, except with a few commits on top of it that do not branch or anything.  So a FF is fine here.

If it doesn't allow a FF due to branch problems, then use the `--no-ff` flag exactly as the original steps above.

Updating the merge in case new PRs appear is the same process - get latest, issue another merge command (without `--no-ff`) and keep rolling.

