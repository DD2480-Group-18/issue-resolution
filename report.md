# Report for assignment 4

## Project

Name: RepoBee

URL: https://github.com/repobee/repobee

RepoBee is a command line tool that allows teachers and teaching assistants to work with large amounts of student Git repositories on the GitHub, GitLab and Gitea platforms (cloud and self-hosted). The archetypical use case is to automate creation of student repositories based on template repositories, that can contain for example instructions and skeleton code. Given any number of template repositories, creating a copy for each student or group is just one command away. RepoBee also has functionality for updating student repos, batch cloning of student repos, opening, closing and listing issues, no-blind and double-blind peer review, and much more!

### Set up the project on your system(s). How good is the “onboarding” documentation?

The onboard experience is pretty good. RepoBee has a dedicated [documentation website](https://docs.repobee.org/en/stable/), as well as an extensive [user guide](https://docs.repobee.org/en/stable/userguide.html). Everything one would normally need to do to set up the project either as a user or a developer, is thoroughly documented.
### Did you continue with the previous project or choose a new one?

We chose a new one as the previous project required a lot of specific domain knowledge that we did not possess. Furthermore, the previous project was mostly written in JavaScript, which meant that understanding interfaces properly sometimes requires one to follow the entire call stack of that interface. The project we chose to work on is using Python with type hints, which is a welcome addition to quicker understand the interfaces.

### If you chose a new project: How does the experience compare to the previous one?

RepoBee, while being a much smaller project than three.js, has a quite good testing suite, even emulating a GitHub-like platform for testing purposes.
The repository sets up self-hosted instances of the hosted Git-platforms `Gitea` and `GitLab` in docker containers for even more rigorous testing, but unfortunately cannot provide the same service for GitHub as there is not self-hosting option.

All unit tests pass, but the system tests (testing the Git-platform integrations) have 1/25 tests failing.

#### The failing test

```
...
================================================
FAILED test_gitlab_system.py::TestEndReviews::test_end_all_reviews - assert not [<Group id:110>, <Group id:111>, <Gro...
====================================== 1 failed, 24 passed in 1850.24s (0:30:50) =======================================
...
```

This test does not seem to interfere with the resolution of our chosen issues. The one issue that could have problems (but is confirmed to not be affected) is the issue of creating one issue per reviewer instead of assigning multiple reviewers to a single issue. This failing test is about ending/closing reviews for the GitLab platform specifically. We are unsure why it is happening, and have opened an [issue](https://github.com/repobee/repobee/issues/1010) about it even though it does not affect our work.

##### Update

This issue was confirmed and reproduced by the author, who promptly [released a fix for it](https://github.com/repobee/repobee/issues/1010).

#### The issues to resolve

The [issue](https://github.com/repobee/repobee/issues/789) that we picked is about changing the way peer-reviewers are assigned to issues. Currently, multiple peer-reviewers are assigned to a single issue which works well if you are using the paid versions of GitLab and GitHub, but is not a feature that comes with neither GitHub's nor GitLab's community/free editions. The proposed solution in the issue is to simply create one issue per reviewer as the standard behavior, so that multiple reviewers work for both the paid and free plans of all the Git-platforms.

We ended up creating three new issues in total of our own, as we worked with the project and found bugs both related and unrelated to our main issue.

- [Task-hindering/related bug](https://github.com/repobee/repobee/issues/1014)
- [Non-task-hindering bug 1](https://github.com/repobee/repobee/issues/1013)
- [Non-task-hindering bug 2](https://github.com/repobee/repobee/issues/1010)

##### Requirements

|  ID   |              Title              |                                   Description                                    |
| :---: | :-----------------------------: | :------------------------------------------------------------------------------: |
|   1   |  Multiple issues per reviewer   |              Create multiple issues for each assigned peer-reviewer              |
|   2   |       Remove old behavior       | Remove behavior that create a single peer-reviewing-issue for all peer-reviewers |
|   3   | Assign peer-reviewers to issues |    Map and assign only one peer-reviewer to each created peer-reviewing issue    |

### The changes to the code

We ended up finding two bugs while fixing our main issue, therefore the final contribution amounted to 3 pull requests.

- [Main issue](https://github.com/repobee/repobee/pull/1016)
  - Discussed pros and cons of solutions with maintainer
  - Merged our branch with a simultaneous refactor made by the maintainer (resolved conflicts)
  - Rewrote the functionality to create separate issues per reviewer (the main task)
  - Modified the project documentation to match the new functionality
  - Added a new test helper and a new integration test to cover the new functionality
- [Bugfix 1](https://github.com/repobee/repobee/pull/1017)
  - Discussed the bug with the author
  - Rewrote a part of the code where issue author names were not lowercase (the actual task)
  - Added a test to test the case-lowering of author names in created issues
- [Bugfix 2](https://github.com/repobee/repobee/pull/1018)
  - Reported the issue in the repository
  - Received confirmation from the maintainer that it is indeed an issue
  - Rewrote a part of the code with guidance from the maintainer. 

The documentation of the changes made are in the issues linked in the PRs. 

## Effort spent

Documentation of time spent (not counting time spent working on different project).

We almost entirely worked together, pair programming or speaking to each other and exchanging ideas over Discord. Therefore, most of the time spent can be documented as time spent _by the entire group_.

Total time spent: ~21 hours

1. plenary discussions/meetings: (~2 hours)
2. discussions within parts of the group: (~2 hours)
3. reading documentation: (~4 hours)
4. configuration and setup: (~3 hours)
5. analyzing code/output: (~4 hours)
6. writing documentation: (~30 minutes)
7. writing code: (~4 hours)
8. running code: (~1 hour)

#### Enumeration of configuration and setup dependencies

- Setting up the project with a virtual environment (pipenv)
- Setting up commit hooks
- Running all tests
- Project configuation
- Setting up Github organization
- Setting up template repositories within organization

## Essence Standard

