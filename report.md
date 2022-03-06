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

|  ID   |              Title              |                                   Description                                    |
| :---: | :-----------------------------: | :------------------------------------------------------------------------------: |
|   1   |  Multiple issues per reviewer   |              Create multiple issues for each assigned peer-reviewer              |
|   2   |       Remove old behavior       | Remove behavior that create a single peer-reviewing-issue for all peer-reviewers |
|   3   | Assign peer-reviewers to issues |    Map and assign only one peer-reviewer to each created peer-reviewing issue    |