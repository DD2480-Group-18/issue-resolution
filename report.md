# Report for assignment 4

## Project

Name: RepoBee

URL: https://github.com/repobee/repobee

RepoBee is a command line tool that allows teachers and teaching assistants to work with large amounts of student Git repositories on the GitHub, GitLab and Gitea platforms (cloud and self-hosted). The archetypical use case is to automate creation of student repositories based on template repositories, that can contain for example instructions and skeleton code. Given any number of template repositories, creating a copy for each student or group is just one command away. RepoBee also has functionality for updating student repos, batch cloning of student repos, opening, closing and listing issues, no-blind and double-blind peer review, and much more!

## 2.2 Part 1: Setup

### Set up the project on your system(s). How good is the “onboarding” documentation?

The onboard experience is pretty good. RepoBee has a dedicated [documentation website](https://docs.repobee.org/en/stable/), as well as an extensive [user guide](https://docs.repobee.org/en/stable/userguide.html). Everything one would normally need to do to set up the project either as a user or a developer, is thoroughly documented.

### Did you continue with the previous project or choose a new one?

We chose a new one as the previous project required a lot of specific domain knowledge that we did not possess. Furthermore, the previous project was mostly written in JavaScript, which meant that understanding interfaces properly sometimes requires one to follow the entire call stack of that interface. The project we chose to work on is using Python with type hints, which is a welcome addition to quicker understand the interfaces.

### If you chose a new project: How does the experience compare to the previous one?

RepoBee, while being a much smaller project than three.js, has a quite good testing suite, even emulating a GitHub-like platform for testing purposes.
The repository sets up self-hosted instances of the hosted Git-platforms `Gitea` and `GitLab` in docker containers for even more rigorous testing, but unfortunately cannot provide the same service for GitHub as there is not self-hosting option.

## 2.3 Part 2: Issue resolution

All unit tests pass, but the system tests (testing the Git-platform integrations) have 1/25 tests failing.

#### The failing test

```
...
================================================
FAILED test_gitlab_system.py::TestEndReviews::test_end_all_reviews - assert not [<Group id:110>, <Group id:111>, <Gro...
====================================== 1 failed, 24 passed in 1850.24s (0:30:50) =======================================
...
```

This test does not seem to interfere with the resolution of our chosen issue, which is to create one issue per reviewer instead of assigning multiple reviewers to a single issue. This failing test is about ending/closing reviews for the GitLab platform specifically. We are unsure why it is happening, and have opened an [issue](https://github.com/repobee/repobee/issues/1010) about it even though it does not affect our work.

#### The issue to resolve

The [issue](https://github.com/repobee/repobee/issues/789) that we have picked is about changing the way peer-reviewers are assigned to issues. Currently, multiple peer-reviewers are assigned to a single issue which works well if you are using the paid versions of GitLab and GitHub, but is not a feature that comes with neither GitHub's nor GitLab's community/free editions. The proposed solution in the issue is to simply create one issue per reviewer as the standard behavior, so that multiple reviewers work for both the paid and free plans of all the Git-platforms.

##### Requirements

| ID  |              Title              |                                   Description                                    |
| :-: | :-----------------------------: | :------------------------------------------------------------------------------: |
|  1  |  Multiple issues per reviewer   |              Create multiple issues for each assigned peer-reviewer              |
|  2  |       Remove old behavior       | Remove behavior that create a single peer-reviewing-issue for all peer-reviewers |
|  3  | Assign peer-reviewers to issues |    Map and assign only one peer-reviewer to each created peer-reviewing issue    |

## Effort spent

For each team member, how much time was spent in

1. plenary discussions/meetings;
2. discussions within parts of the group;
3. reading documentation;
4. configuration and setup;
5. analyzing code/output;
6. writing documentation;
7. writing code;
8. running code?

For setting up tools and libraries (step 4), enumerate all dependencies
you took care of and where you spent your time, if that time exceeds
30 minutes.

## Code changes

### Patch

```
From 780a060ada203ba349e78b6fbcb09679e2766889 Mon Sep 17 00:00:00 2001
From: Zino Kader <zinokad@gmail.com>
Date: Sun, 6 Mar 2022 19:18:35 -0600
Subject: [PATCH] [feat] Assign each student a separate review issue

Fixes #789

- Creates a new review issue for each student
- Assigns each user to their own review issue
---
 src/_repobee/command/peer.py | 24 ++++++++++++++----------
 1 file changed, 14 insertions(+), 10 deletions(-)

diff --git a/src/_repobee/command/peer.py b/src/_repobee/command/peer.py
index 326a097..a4d45b9 100644
--- a/src/_repobee/command/peer.py
+++ b/src/_repobee/command/peer.py
@@ -149,16 +149,20 @@ def assign_peer_reviews(
             api.assign_repo(
                 review_team, reviewed_repo, plug.TeamPermission.PULL
             )
-            api.create_issue(
-                issue.title,
-                issue.body,
-                reviewed_repo,
-                # It's not possible to assign users with read-access in Gitea
-                # FIXME redesign so Gitea does not require special handling
-                assignees=review_team.members
-                if not isinstance(api, _repobee.ext.gitea.GiteaAPI)
-                else None,
-            )
+
+            for member in review_team.members:
+                issue_title = f"{issue.title} ({member})"
+                api.create_issue(
+                    # issue.title = "Peer review" --> "Peer review (member)"
+                    issue_title,
+                    issue.body,
+                    reviewed_repo,
+                    # It's not possible to assign users with read-access in Gitea
+                    # FIXME redesign so Gitea does not require special handling
+                    assignees=[member]
+                    if not isinstance(api, _repobee.ext.gitea.GiteaAPI)
+                    else None,
+                )

             allocations_for_output.append(
                 {
--
2.32.0 (Apple Git-132)
```

Optional (point 4): the patch is clean.
Optional (point 5): considered for acceptance (passes all automated checks).

## Test results

Overall results with link to a copy or excerpt of the logs (before/after
refactoring).

## Overall experience

What are your main take-aways from this project? What did you learn?
How did you grow as a team, using the Essence standard to evaluate yourself?

Optional (point 6): How would you put your work in context with best software
engineering practice?
Optional (point 7): Is there something special you want to mention here?
