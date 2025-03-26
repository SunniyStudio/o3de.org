---
title: "Maintaining-O3DE-Roadmap"
description: ""
toc: false
---

This document describes the process of how each special interest group (SIG) maintains its O3DE Roadmap. 
By having an up-to-date O3DE roadmap, the marketing committee, release manager, and the community have visibility to understand what are things the team is currently working on and what will be done in the future.

This document will be recommended reading for all new and existing contributors (including the chairs) to O3DE to help people all get on the same page about the importance and responsibility of maintaining the O3DE roadmap.

When you make edits, please note your change when you save.
***

# O3DE Roadmap
Each SIG has its own roadmap project board. 

# Roles and Responsibilities
Here are the roles that involved keeping the O3DE roadmap up-to-date.

### SIG Chair/Co-chair
* Maintain the roadmap and keep the roadmap updated with the latest updates.
* SIGs are responsible for understanding all of the contribution channels to their roadmap and reviewing these sources regularly. For example, reviewing any corporate or community roadmaps that may be published, RFCs, and other issues.

### SIG-Release
* Remind SIGs to keep the roadmap updated.
* Continuously reviewing the roadmap maintenance process and improving it.

### TSC
* To give feedback or ask questions about the roadmap item updates in the TSC monthly meeting.

# Processes

## Roadmap Items
### Definition
* It's an issue that shows in the O3DE public roadmap. The issue communicates the initiative and vision of the SIG.
* The roadmap item is an issue labeled as kind/roadmap.

### Roadmap Item Description
* By default, it follows the RFC Features request format like this one (which I believe is the same across all SIGs). The format: https://raw.githubusercontent.com/o3de/sig-release/main/.github/ISSUE_TEMPLATE/roadmap-item.md
* Roadmap item example: https://github.com/o3de/o3de/issues/13206
* It contains tasks (link to GitHub issues) to complete the roadmap item.

### Responsibility
* It's each SIG's responsibility to maintain the status of the item, including closing the issue and adding labels as suggested in (2).

## Roadmap Review in TSC Monthly Meeting
### Purpose
To provide visibility for TSC and other SIG members for the key roadmap items ahead. Key roadmap items could be items that impact another area of the product or key features.
### Participants
SIG chairs (mandatory
### Agenda
Each of the SIGs informs others about the key roadmap items ahead as part of their monthly updates. The roadmap should be updated prior to the meeting.
### When
Every join TSC + SIG monthly meeting

## SIG Roadmap View Setup
### Purpose
This is to help readers read and consume the roadmap items easier by organizing the issues in a table structure
### Each SIG creates a public project board under each SIG's repositories.
Based on the discussion https://github.com/o3de/sig-release/issues/79#issuecomment-1346838887, it’s the current practice to have everything related to the SIG in each respective repository.
### The project title follows this template "O3DE - [SIG Name] Project Board". 
Example: O3DE UI/UX Project Board
### The board contains a view named "Roadmap View". The view contains all issues with a kind/roadmap label.
### To reduce redundancy, hide the "Status" column in the project so we can reuse the existing label to communicate statuses.
* needs/triage.
* triage/accepted - when the issue is accepted and planned to be done in the future.

## Add a new template "Add a roadmap Item"
### Purpose
To increase awareness of the expected information in the roadmap item.
### Content
* The template contains the information explained in the section Roadmap Item - Format.
* By default contains labels: needs-triage and needs-sig.
### Location
The new template is in the O3DE repo.

## A Roadmap Page in the Documentation portal
### Purpose
To show all roadmap boards (including SIGs, O3DE corporate roadmap, etc) on one single page. This is so we have a page that tells the community where the roadmap is located.
### Ownership
The ownership on SIG-release to make sure it's updated, but it's not necessarily the one that updates the websites.
### Content
* The page contains all roadmap links.
* The page contains a disclaimer the roadmap is subject to change as the maintainer considers feedback both from the community and others.
### Location
* The page is located at the o3de.org/docs/roadmap
* The page can be accessed from the navigation menu.

# Use Case Flows
## Roadmap item creation
1. Contributor (including chairs) creates the roadmap item in the SIG repositories.
## Keeping the O3DE Roadmap up to date
1. SIG triage each roadmap item. The triage cadence is up to each SIG.
2. Once the issues are accepted, SIG assigns the label "triage/accepted"
3. Once the issues are completed, SIG to close the issue.
4. Every month each SIG shares its key roadmap with the team in the Joint TSC-SIG meeting as part of their monthly updates. The roadmap should be updated prior to the meeting.
## The next O3DE release is about to happen
1. The SIG release queries the completed roadmap items after the last release.

