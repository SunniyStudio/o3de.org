---
title: "Jenkins-execution-details"
description: ""
toc: false
---

This wiki gives some details on how Jenkins execution happens and what it executes under the hood. Jenkins ends up executing scripts that were created in a way that are possible to run locally (mostly, will clarify some caveats).

O3DE has a Jenkins instance. AWS has another instance setup in their fork. Any contributor can setup their own instance to do validations within their fork. The AWS instance runs more exhaustive validations.

Within each Jenkins instance, things are divided into "pipelines". 
> A Pipelines is a suite of plugins which supports implementing and integrating continuous delivery pipelines into Jenkins. (https://www.jenkins.io/doc/pipeline/tour/hello-world/)

I apologize for that awful definition. Just wanted to quote it because the concept of a "pipeline" is really ambiguous under Jenkins. My definition of a pipeline is "a suite or suites of jobs". I am diverging a bit from what Jenkins provides and what doesnt. Jenkins allows to create pipelines that may not run on any branch, or run in multiple repositories on multiple branches. However, at the end of the day, you will see a list of suites and within each of those suites you will see a set of jobs with a certain "structure". 

The O3DE instance is located in https://jenkins.build.o3de.org/job/O3DE/
The landing page has two tabs: "Branches" and "Pull Requests". Both tabs are using the same "pipeline". These tabs are just filters on branches. The "Branches" tab holds all the branches that are not PRs in the O3DE repo. Here you will find the main, development and stabilization branches. The "Pull Requests" tab contains all the PR branches. The PR branches are the ones that are validated on the PR review process. 
Another difference between the two tabs is that the "Branches" tab is configured to build on changes, whereas the "Pull Requests" tab is configured to build on-demand. The "on-demand" configuration was purposely done since developers tend to push their changes frequently, however, they may not be ready for their change to be built. The "Branches" tab on the other hand, is only going to receive changes caused by PR merges, so that makes more sense to build on changes.

Both tabs use the "default" pipeline. The difference between them ends up being the branch that it runs on and what triggers the suite to run.
Diving into one of these entries, takes you to a Jenkins page that has more information, history of runs and allows to run each suite (if you have permission to do so). 

Most of our Jenkins setup has one entry point: a [Jenkinsfile](https://github.com/o3de/o3de/blob/development/scripts/build/Jenkins/Jenkinsfile)
The Jenkinsfile is in charge of setting up the environment for the run and running the jobs. Our Jenkinsfile is somehow complex because we have done a lot of optimizations to reduce build time and have reporting/tracking features to send alerts. I will go over some of the things we are doing in the Jenkinsfile:
1. We dynamically construct the parameters that are later exposed to execute runs. This makes it possible to tweak how the run is made. There are restrictions on what can be tweaked in different runs.
2. In order to reduce build time, we utilize EBS mounts per branch and pipeline and other factors (here pipeline is referring to the suite). An EBS mount is basically a networked filesystem within AWS. We maintain the build artifacts and state across job instance runs, even if those runs happen in different machines (a.k.a. nodes). This means that if we performed a build, then make no changes in a PR and perform another build, we should have a "zero build" case. 
3. When we create those EBS volumes, instead of creating empty ones, we take snapshots from relevant branches (e.g. development, main and stabilization branches) and create the EBS volumes out of those snapshots. This means that a new build in a branch will not build from scratch, but build from the state of e.g. `development` at the point the snapshot was taken. We take snapshots nightly and keep just two snapshots per job around. Since we take snapshots from the `development` branch, only jobs in that pipeline are snapshoted. 
4. 
