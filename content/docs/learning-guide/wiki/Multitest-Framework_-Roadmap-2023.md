---
title: "Multitest-Framework_-Roadmap-2023"
description: ""
toc: false
---

- [Roadmap Project Link](#roadmap-project-link)
- [Multitest Framework wiki](#multitest-framework-wiki)
- [Introduction](#introduction)
- [Q1 Jan-Mar: Stability](#q1-jan-mar-stability)
  * [Feature Request: Improve reporting deadlocks from EditorTest](#feature-request-improve-reporting-deadlocks-from-editortest-httpsgithubcomo3deo3deissues8364)
  * [Parallel and Shared Python tests disabled by default](#parallel-and-shared-python-tests-disabled-by-default-httpsgithubcomo3deo3deissues12465)
  * [TestResult Editor_Test.Log is Missing Data if Test Successful](#testresult-editor_testlog-is-missing-data-if-test-successful-httpsgithubcomo3deo3deissues8537)
  * [Automated Test Teardown and Wrap_Run Doesn't Run if Test Fails](#automated-test-teardown-and-wrap_run-doesnt-run-if-test-fails-httpsgithubcomo3deo3deissues8058)
  * [Test Tools: launcher_platform parameterization behaves unexpectedly](#test-tools-launcher_platform-parameterization-behaves-unexpectedly-httpsgithubcomo3deo3deissues10941)
  * [AR Bug Report: editor_test.log Wrong Time Stamp](#ar-bug-report-editor_testlog-wrong-time-stamp-httpsgithubcomo3deo3deissues13913)
- [Q2 Apr-Jun: Code cleanup and stability follow-up.](#q2-apr-jun-code-cleanup-and-stability-follow-up)
  * [EditorTest expectations library](#editortest-expectations-library-httpsgithubcomo3deo3deissues8874)
  * [Code Cleanup reminder for Multitest Framework (typos, etc. - not a full re-factor).](#code-cleanup-reminder-for-multitest-framework-typos-etc---not-a-full-re-factor-no-issue-link-currently)
  * [Gather feedback from sig-testing team for feature requests or improvements.](#gather-feedback-from-sig-testing-team-for-feature-requests-or-improvements-no-issue-link-currently)

## Roadmap Project Link
* Github project page for Multitest Framework Roadmap 2023: https://github.com/orgs/o3de/projects/30/views/1 

## Multitest Framework wiki
* Wiki page for using the Multitest Framework to write automated tests: https://github.com/o3de/o3de/wiki/Multitest-Framework:-Running-one-or-multiple-tests-in-one-or-multiple-executable(s). 

## Introduction
Welcome to the Roadmap page for the Multitest Framework tool. The primary focus of Q1 for the roadmap is going to be bug resolution and stability improvements for the existing Multitest Framework as we believe solidifying this first is more important than adding a new feature right away for Q1. We can explore changing this in Q2 or Q3 depending on how the fixes go.
* The Q1 goals created here are based off of the list of issues reported here: https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Ftesting+
* Issues selected will be based on viability to implement as well as its impact on issue resolution. GHI tickets deal with crashes, freezes, and other critical/blocker priority issues will be the focal point in order to increase stability of the Multitest Framework.
* Q3 and Q4 haven't been planned out yet. We plan to use the results of Q1 and Q2 as well as gather feedback from sig-testing during Q2 before planning out Q3 and Q4. This feedback gathering initiative is part of the Q2 roadmap so we won't forget to do it for planning Q3 and Q4.

## Q1 Jan-Mar: Stability
### **Feature Request: Improve reporting deadlocks from EditorTest** (https://github.com/o3de/o3de/issues/8364)
* **Related GHIs**:
  * https://github.com/o3de/o3de/issues/11260 
  * https://github.com/o3de/o3de/issues/10402 
* **Description**: This has been an issue for a while now, though feedback regarding it has gone down. Still, it may become a problem in the future, so the plan is to add code in the Multitest Framework that will force crash the currently frozen Editor (or other executable) and then dump the results of that into the test output logs. This will hopefully help debug freezes and hard locks in the future once added.

### **Parallel and Shared Python tests disabled by default** (https://github.com/o3de/o3de/issues/12465)
* **Related GHIs**:
  * https://github.com/o3de/o3de/issues/9295
  * https://github.com/o3de/o3de/pull/9547
  * https://github.com/o3de/o3de/issues/9484
* **Description**: Parallel tests are no longer used by any test code and only exist currently to confuse users of the Multitest Framework. Additionally, they have issues with crash logs when multiple instances of the same executable (such as Editor) are open. This creates a massive risk just to gain a bit of speed on the run time of the tests, so the trade off ends up not being worth it. Editor itself isn't coded in such a way to handle multiple instances of itself being open and some applications (such as MaterialEditor) even limit how many can be open at once. For these reasons, we'll be removing the parallel test options from the Multitest Framework code completely. All references to it will be updated on the Multitest Framework wikis.

### **TestResult Editor_Test.Log is Missing Data if Test Successful** (https://github.com/o3de/o3de/issues/8537)
* **Related GHIs**: N/A
* **Description**: This is a Linux specific issue and should be fixed (assuming there isn't an underlying issue with the Editor itself).

### **Automated Test Teardown and Wrap_Run Doesn't Run if Test Fails** (https://github.com/o3de/o3de/issues/8058)
* **Related GHIs**: N/A
* **Description**: These calls should work as expected, especially with EditorSingleTest class objects. This is most likely just a bug somewhere in the logic and hopefully will be easy to fix. The harder part will be the testing to verify it still works as expected. Regarding other test class objects such as EditorBatchedTest, it might not be possible to use wrap_run() and teardown() but we will double check to make sure this is the case.

### **Test Tools: launcher_platform parameterization behaves unexpectedly** (https://github.com/o3de/o3de/issues/10941)
* **Related GHIs**: N/A
* **Description**: Users select their platforms using this fixture and it should be supported by the Multitest Framework. This change will most likely require an update to the launchers in ly_test_tools but its also possible this information isn't being pushed to the launcher from the Multitest Framework. Regardless, it will be fixed in Q1 for this stability push on Multitest Framework.

### **AR Bug Report: editor_test.log Wrong Time Stamp** (https://github.com/o3de/o3de/issues/13913)
* **Related GHIs**: N/A
* **Description**: Simply a bug with the timestamp in the editor_test.log. We'll get this fixed for Q1.

## Q2 Apr-Jun: Code cleanup and stability follow-up.
### **EditorTest expectations library** (https://github.com/o3de/o3de/issues/8874)
* **Related GHIs**: N/A
* **Description**: This answers the problem with crashes failing all of the tests that come after the test that causes the crash. Ideally when a crash occurs we have a way to continue running the test, or somehow move the tests into the next batch of tests that are going to run. This will require a bit of exploration to see how possible it is to do, but would be a great addition to help with identifying crashes.

### **Code Cleanup reminder for Multitest Framework (typos, etc. - not a full re-factor).** (No issue link currently)
* **Related GHIs**: N/A
* **Description**: There are various typos and random errors in the various Multitest Framework files that should be cleaned up to prevent confusion. One example is a reference to `BaseTestSuite` which no longer exists, but there are other typo errors too.

### **Gather feedback from sig-testing team for feature requests or improvements.** (No issue link currently)
* **Related GHIs**: N/A
* **Description**: This will be good to get a gauge on how users feel about the Multitest Framework and find some new feature proposals to add to it or important changes to make for bug fixes.

## Q3: This will be updated sometime in Q2.
## Q4: This also will be updated sometime in Q2.
