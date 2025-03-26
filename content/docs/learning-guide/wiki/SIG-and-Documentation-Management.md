---
title: "SIG-and-Documentation-Management"
description: ""
toc: false
---

The purpose of this doc is to provide guidance on how to manage and run a special interest group (SIG) and manage documentation.

When making edits, please be sure to note your change when you save.
***


## How to Run a Special Interest Group (SIG)

A SIG is made up of a SIG chair (or co-chairs) and SIG members. 


### High-level Flow

* Attend SIG meetings and joint TSC/SIG meetings
* Triage incoming issues submitted to o3de.org repo
* Maintain the O3DE roadmap 
* Maintain SIG projects


### SIG Responsibilities

The scope of the SIG and their responsibilities can be found in the charter located at `sig-*/governance/charter.md`. For example, see [Documentation and Community SIG Charter](https://github.com/o3de/sig-docs-community/blob/main/governance/charter.md).

**Note:** SIG chair responsibilities are only partially documented in the charter.


#### SIG Chair Responsibilities

* **Meetings.** Lead SIG meetings, lead issue triage meetings, and represent their SIG at TSC/SIG meetings. 
* **Roadmap.** SIG chair act as program managers for the SIG’s roadmap. During SIG meetings, they check in with the SIG members’ assigned tasks and update the task’s status. The roadmap should be maintained as the source of truth, which anyone can view to see what’s upcoming for O3DE. 


#### SIG Member Responsibilities

SIG members take on a role as _reviewer_ or _maintainer_. A _maintainer_ has additional responsibilities and help guide the SIG. 

For more information, see [Community Membership](https://github.com/o3de/community/blob/main/community-membership.md).


### SIG Processes


#### Monthly Cycle

Reference the [community calendar](https://lists.o3de.org/g/o3de-calendar/calendar)

#### SIG Meetings

SIG meetings typically occur biweekly or monthly. A SIG meeting provides a space for all SIG members to discuss what’s going on with the SIG and O3DE community as a whole. Some topics include: 

* **What happened since the last meeting** – changes to the team, recent events, O3DE community-wide updates. 
* **Tasks** – open action items and newly resolved items. 
* Additional agenda items that members can add. 


#### SIG Issue Triage Meetings

All SIGs are responsible for triaging issues in:
1. their SIG repo
2. code in the o3de repo, or docs in the o3de.org repo. 

If there’s a more appropriate team who can handle the issue, you can [transfer the issue to another repo](https://docs.github.com/en/issues/tracking-your-work-with-issues/transferring-an-issue-to-another-repository).

**Suggestion:** Add to “TRIAGE_GUIDE.md” “Proposed SIG-Network meeting agenda”
Example: https://github.com/o3de/sig-network/blob/main/TRIAGE_GUIDE.md 


#### TSC/SIG Meetings

The TSC creates a meeting agenda, where the SIG chair from each SIG can provide an update about the SIG. The update can include:
1. what the the SIG has worked on since the last meeting 
2. what the SIG plans to work on next. 

These updates are generally high-level, pertaining to roadmap items. For example, it’s informative to know what features a SIG is working on, but it’s not necessary to list all the minor issues they resolved. 


#### Maintaining SIG Roadmap

All SIGs follow the same instructions. See [Maintaining O3DE Roadmap](https://github.com/o3de/sig-release/issues/79).


#### SIG Projects / Request For Comments (RFCs)

SIG projects are initiatives that fall into the SIG’s scope as detailed by the charter. They are started by the SIG members or other community members. Typically SIG projects start out as issues and become RFCs (request for comment). SIG RFCs generally follow this [format](https://github.com/o3de/community/blob/main/.github/ISSUE_TEMPLATE/rfc-suggestion.md).


### Electing a SIG chair

All SIGs follow the same instructions. See [O3DE Election Guides and Resources](https://github.com/o3de/community/tree/main/O3DE%20Elections). 


### Nomating SIG Membership

Instructions on how to become a SIG member can be found in the SIG charter. For example, see **To request membership to or update the team** in the [Maintainers](https://github.com/o3de/sig-docs-community/blob/main/governance/charter.md#maintainers) section of the charter.


## How to Maintain O3DE Docs

Maintaining O3DE Docs involves SIG maintainers and reviewers to review and merge pull requests in the [o3de.org repository](https://github.com/o3de/o3de.org). 

**Note:** In the past, maintainers of the SIG Docs and Community were responsible for reviewing and merging pull requests. Now, maintainers from any SIG have permission to do so.


### Step 1: Review incoming docs changes

1. Open the [o3de.org repository](https://github.com/o3de/o3de.org) in the GitHub web interface. 
2. In the navigation, open **Pull requests** (PRs). “Open” PRs contain changes that a contributor would like to make to the documentation. 
3. Choose a PR to view its details. 
4. In the navigation of the PR, choose **Files changed**. This lists all proposed changes.
5. Ensure that the docs changes are technically accurate and use a consistent style. 
   * For style changes, refer to the [O3DE Style Guide](https://docs.o3de.org/docs/contributing/to-docs/style-guide/). Make suggestions and comments requesting the contributor to make those changes. 
   * For technical accuracy, refer to a subject matter expert (SME). While the contributor may be the expert, it’s best to request a review from another expert to double-check their work. Typically, this can be a member from the SIG that this PR pertains to. 
6. When you’ve deemed the PR is ready, choose **Review changes**, select **Approve**, and choose **Submit review**. 


### Step 2: Publish docs changes

There are a few key factors to note when merging docs changes: 

* A PR can be reviewed/approved by many reviewers, but it must be approved by a _code owner_ of the file. 
* A PR is typically merged to either the main or development branch. Only PRs merged to the main branch will appear in the live website. 


#### Requirements to Merge a PR

* One (1) approval from a valid reviewer
* A valid reviewer is someone who’s marked as a _code owner_ for the files updated in this PR. Code owner rules are specified in the [CODEOWNERS](https://github.com/o3de/o3de.org/blob/development/.github/CODEOWNERS) file of the repo. 

**Note:** Requirements are defined for each branch, via the GitHub [branch protection rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule) and the [General settings](https://github.com/o3de/o3de.org/settings).


#### To Merge a PR

In the PR webpage, at the bottom of the **Conversations** tab, there’s an action to merge the PR. 

**Note:** A PR is typically merged to either the main or development branch. Only PRs merged to the main branch will appear in the live website. The development branch must be merged to main via the [docs release process](https://github.com/o3de/sig-release/blob/main/releases/Process/Docs%20Release%20Process.md).


### Contributing to O3DE Docs

Contributors to O3DE Docs are writers, engineers, and other community members. There is complete documentation about how to contribute in the [Contributing Guide](https://docs.o3de.org/docs/contributing/). 

To continue to support O3DE engineering contributors to also contribute documentation updates for their feature work, we have implemented the following:

* Comprehensive contributor guidelines for O3DE documentation[ https://www.o3de.org/docs/contributing/](https://www.o3de.org/docs/contributing/)
* Simplified PR review requirements. Previously, PRs required 2 approvals from corresponding code owners and had to be merged by a member of sig-docs-community. Now a PR only requires 1 approval, and can be merged by a member of any SIG maintainers team
