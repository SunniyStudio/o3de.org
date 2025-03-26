---
title: "Git-LFS-Troubleshooting"
description: ""
toc: false
---

These are a few common issues that come up when cloning or pushing LFS files fails.

# Cloning the repository or pulling LFS files fails with a "Forbidden" error or other authentication error
1. Verify you are using your GitHub username and personal token, not your GitHub password.
1. Verify that if you are copying your personal token from a text file that you are not including an extra space or tab.  You can paste your token in a text editor to verify it doesn't have any extra spaces at the front or end.
1. Verify the personal token in your password manager is correct.  If using Windows, these credentials can generally be found in your `Windows Credential Manager` with a name that includes the URL found in the `.lfsconfig` file at the root of the repository. At the time of writing, the o3de repo entry has a name similar to `git:https://d3df09qsjufr6g.cloudfront.net/api/v1`.  Keep in mind, we use different LFS URLs for each repository.
1. Verify you can authenticate with GitHub's API by running the following command in a command window.  Substitute your username and token in the command along with the URL of the repository you are attempting to access.
    ```
    curl -H <GitHub username>:<GitHub token> https://api.github.com/repos/o3de/o3de
    ```
    If you receive a 404 response, your credentials are invalid or you do not have permissions.  
    If you receive a 200 response, your permissions can be found in the returned JSON.
1. Verify you haven't exceeded your GitHub API rate limits by running the following command in a command window.  Substitute your username and token in the command.
    ```
    curl -H <GitHub username>:<GitHub token> https://api.github.com/rate_limit
    ```
    If you have no remaining resources, you will need to wait until the time set in the `reset` timestamp field.
1. Examine the contents of the HTTP headers to troubleshoot the credentials and responses you are sending to the service.  In a command window enable additional logging
    ```
    set GIT_TRACE=1
    set GIT_CURL_VERBOSE=1
    ```
    After enabling this additional logging, try your git operation again and you will be able to see the exact authentication headers you are sending and the responses given.  Keep in mind, credentials are usually base64 encoded, so you likely need to decode them.  You may also want to pipe the output to a log file for easier searching.

# Pushing LFS files fails with a "Forbidden" error or other authentication error
1. Verify you are not trying to push directly to the o3de parent repository, this is not allowed.  You should only be pushing changes to a fork  of the main repository.
1. Verify, if you are trying to push to your fork that you have followed the instructions in the `.lfsconfig` file at the root of the repository, specifically, that you are including the name of your fork.
1. Refer to the troubleshooting steps above for cloning or pushing which cover more authentication troubleshooting steps.

# Some files fail to download with a "Smudge error" message
1. When a file fails to download with a smudge error, the likely cause is it was never uploaded.  First request the original user re-upload all LFS files using the command
    ```
    git lfs push origin --all
    ```
    If you know the specific Object ID of the file (it's in the error message) you can just upload the single missing file using this command
    ```
    git lfs push origin --object-id <object-id>
    ```
2. After the file(s) have been uploaded, run the following commands to get them from the `origin` remote.
    ```
    git lfs pull origin
    ```
    If the files still do not appear in the workspace, this can be because the files were only downloaded.  Use the following command to refresh the specific files, replace `<branch>` with the name of the branch you are on and replace `path/to/files` with the path to the files you are missing whether that is a single file or multiple files
    ```
    git checkout origin/<branch> path/to/files
    ```
