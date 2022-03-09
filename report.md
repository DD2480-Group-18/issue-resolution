# Report for assignment 4

## Project

Name: RepoBee

URL: https://github.com/repobee/repobee

RepoBee is a command line tool that allows teachers and teaching assistants to work with large amounts of student Git repositories on the GitHub, GitLab and Gitea platforms (cloud and self-hosted). The archetypical use case is to automate creation of student repositories based on template repositories, that can contain for example instructions and skeleton code. Given any number of template repositories, creating a copy for each student or group is just one command away. RepoBee also has functionality for updating student repos, batch cloning of student repos, opening, closing and listing issues, no-blind and double-blind peer review, and much more!

### Set up the project on your system(s). How good is the "onboarding" documentation?

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

Patch changes:

- [Main task, changes](https://github.com/repobee/repobee/pull/1016/files)
- [Task-hindering/related bug, changes](https://github.com/repobee/repobee/pull/1017/files)
- [Non-task-hindering bug 1, changes](https://github.com/repobee/repobee/pull/1018/files)
- (Non-task-hindering bug 2) was [fixed by the maintainer](https://github.com/repobee/repobee/pull/1011)

#### Requirements

|  ID   |                 Title                 |                                                Description                                                 |
| :---: | :-----------------------------------: | :--------------------------------------------------------------------------------------------------------: |
|   1   |     Multiple issues per reviewer      |                           Create multiple issues for each assigned peer-reviewer                           |
|   2   | Remove old behavior for issue reviews |              Remove behavior that create a single peer-reviewing-issue for all peer-reviewers              |
|   3   |    Assign peer-reviewers to issues    |                 Map and assign only one peer-reviewer to each created peer-reviewing issue                 |
|   4   |      Change issue naming scheme       | Change the issue review assignment naming scheme to include the name of the assigned reviewer in the title |

Added requirements during the project work phase:

|  ID   |                  Title                   |                                                                              Description                                                                               |
| :---: | :--------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|   5   |    Make issue author names lowercase     | For consistency throughout the project (and correct comparison when issues have authors with names like ZinoKader), make author names in the Issue dataclass lowercase |
|   6   | Handle empty issue body without crashing |                                   When analyzing issues with the `--show-body` flag, do not crash if the body of the issue is empty                                    |

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

#### UML Diagrams

These issues are hard to include the typical class UML diagrams for; firstly because they do not make use of classes, and second because we mostly dabble in already existing code and extend it. The one thing that we found useful to create diagrams for was control flow, as that is relevant to the bugs we found (and created/solved issues) for.

The main issue revealed a bug (see Bugfix 1), which was one layer further down in the control flow of the main issue.
These issues have therefore been included in the same UML diagram.

##### Control flow of the _main issue_ **and** the lowercase author issue

![main issue and bugfix 1 control flow](/diagram/author-lowercase-diagram.png)

### Effort spent

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

- Setting up the project with a virtual environment (pipenv) (~1 hour)
- Setting up commit hooks (~10 minutes)
- Running all tests (~1 hour)
- Project configuation (~10 minutes)
- Setting up Github organization (~20 minutes)
- Setting up template repositories within organization (~20 minutes)

### Essence Standard

We are firmly in mostly performing. The team is mostly working effectively and efficiently.

This is our motivation:

* Consistently meeting commitments: The team consistently meets its commitments. Yes, we did meet our commitments, although we did not work every day we should have, which yet again left a lot of work for the last day of the deadline (mostly writing the report).
* Continuously adapting to change: The team continuously adapts to the changing context. When we picked the issue to work on and notified the maintainer about this, he told us that he was refactoring that exact file and function which we were going to dabble in. We started writing code anyway, and merged our changes and fixed the conflicts, showing that we are adaptable to unforeseen circumstances.
* Addresses problems: The team identifies and addresses problems without outside help. We addresses **3** bugs within the project and notified the maintainer, and fixed **2** of these, which is why we are confident in saying that we are able to address problems. That is in the actual technical work, however. We did not discuss any team-related problems for this assignment, because there were none.
* Rework and backtracking minimized: Effective progress is being achieved with minimal avoidable backtracking and reworking. In assignment 2/3, there was some reworking, but in this assignment we managed to cut up the work in chunks that did not affect the other team member, which minimized reworking.
* Waste continuously eliminated: Wasted work, and the potential for wasted work are continuously eliminated. We have not done any double-work since the first assignment as we make sure it is clear who does what.

Even with our small team, we are very happy with the way we are working. We still need to spread out work more over the entire period, that is not as easily achived because of factors ourside our group and this course.
We are both in a place where we are fine with this way of working, though, and can count on each other to do the work that is expected. We have definitely improved during the course, especially when it comes to communication and reminding each other of
when it is time to put down some hours. The next state is "adjourned", which is conveniently the state the team will be in after this assignment. 

### P+ tasks fulfilled

- Point 2: Updates are put into context with the overall software architecture
- Point 3: Relevant test cases (existing tests and updated/new tests related to the refactored code) are traced to requirements
- Point 4: Patch is clean (see [Clean patch](#clean-patch))
- Point 5: Patches/PRs are considered for acceptance
- Point 7: We have found **three** new moderately critical issues, of which we fixed **two**, and the maintainer fixed the last one.
  
#### Updates in source put into context with architecture / design patterns

The classes and modules of the project are well designed and seems to follow the principles of *closed for modification open for extension* well, especially for the tests. This means that we were able to extend the classes and functions used for testing without adding unnessessary complexity.

Furthermore, the maintainer was in the middle of [refactoring](https://github.com/repobee/repobee/pull/1015) the file that we were mainly working in.
We had some discussions in the [issue](https://github.com/repobee/repobee/issues/789) about length of functions. For instance, refactoring a long function by splitting it into smaller functions does not only come with positives.
By splitting a large function into smaller ones, new complexities can be introduced as the results of functions need to be glued together. The complexity introduced by the aforementioned refactoring is that asynchronous functions had to be handled with callbacks, which was not required before breaking the function out into smaller ones.

#### Relevant test cases and tracing to requirements

Both the main issue and bug fixes have their own respective test case that tests the main criterias.

The test logs before and after can be found in the `test-logs` folder in this repository.
The repository has [92% code coverage](https://app.codecov.io/gh/repobee/repobee), although the new tests have not been accounted for yet as they have not been merged into master yet.

By interpolating from what we have done, we can only theorize but are pretty sure that the code coverage will either increase or stay the same with our changes. We changed one behavior without adding much new code, and added another test for that (which increases coverage). We then resolved an unrelated issue, also by adding a smaller amount of code and adding one test for it.

##### Requirement 1, 3 and 4

- A [test-helper](https://github.com/repobee/repobee/pull/1016/files#diff-f565f0db0f92356917bc284edbd6234b5cef50b6023b0a4d86b80a0c7f7d6c3bR203) was written, which is used in the test written for this requirement.
- The [actual test](https://github.com/repobee/repobee/pull/1016/files#diff-8d6380215700c8bc08b443674244fd7718fc0400e8665de4338c99434288f363R44).

This test makes sure that our new [naming scheme](https://github.com/repobee/repobee/pull/1016/files#diff-8d6380215700c8bc08b443674244fd7718fc0400e8665de4338c99434288f363R48) (req. 4) for the created issue is [tested](https://github.com/repobee/repobee/pull/1016/files#diff-8d6380215700c8bc08b443674244fd7718fc0400e8665de4338c99434288f363R64).
Furthermore, it [tests](https://github.com/repobee/repobee/pull/1016/files#diff-8d6380215700c8bc08b443674244fd7718fc0400e8665de4338c99434288f363R65) that the main functionality (one issue per assignee) (req. 1, 3) is upheld.

##### Requirement 5

The [test](https://github.com/repobee/repobee/pull/1017/files#diff-3d51d4b43bd3760aff17ccea064e235d96ae06803c3e737adf3c4798ca4750a0R161).

This test makes sure that creating a new instance of a `Issue` dataclass with an author name containg a mix of uppercase and lowercase letters will convert the author's name to [all lowercase letters](https://github.com/repobee/repobee/pull/1017/files#diff-3d51d4b43bd3760aff17ccea064e235d96ae06803c3e737adf3c4798ca4750a0R171).

#### Clean patch

Both patches are clean. They do not comment out code or add comments unnessessairly, they follow the style guide and pass all style tests created by the repository, they do not add changes like line break changes, and does not produce any extraneous output. 

#### Acceptance

Based on these factors, our changes are highly likely to be accepted by a maintainer:

* We are in close contact with maintainers. Potential solutions to the problem [have been discussed and approved before submitting our code for review](https://github.com/repobee/repobee/issues/789#issuecomment-1060094424).

* All changes in our PRs has passed the required style checks (git-hooks).

```
black....................................................................Passed
Trim Trailing Whitespace.................................................Passed
Flake8...................................................................Passed
Check Yaml...........................................(no files to check)Skipped
```

* All our changes are finalized and published in their own PRs. The issues and PRs follow the unofficial format/style set by the repository. 

#### Software practices

##### Pros

Since we were in contact with developers who had more knowledge of the software system, we were able to more efficiently meet the requirements of any issue. Their testing suite were well-made and easily extendable, which helped us write tests to validate that the changes were following the requrements set in the issue.

Also, since the abstraction layer between any platform's API and the logic was well-made and generalized, we could deliver our solution much faster to all stakeholders instead of only deliverign to a subsection of them. Ease of change is another way that well-crafted software systems create opportunity.

##### Limitations

Since this is mainly a KTH project, we have little access to testers (stakeholders) that assures the quality of our software on platforms not normally used by KTH (like Github Free / CE, more. ). If there are more people from other schools usign this tech, there will be more people to validate that our fix not only solves the problem, but also still fits the requirements of the stakeholders. 

### Overall experience

Our experience working with this repository has only been positive. The code already present is very well-written and easy to understand. Even though the project is written in Python, it has type hints and does not fall into a typical pattern with Python where code is "smarter" and shorter than it needs to be just for the sake of being short.

The maintainer has been very active in assigning us to issues when we request it, and has been providing us with very useful information to quickly get started with working on the issues.

The documentation in the README and on the hosted docs are also great on their own, and would have been sufficient onboarding without the extra luxuries mentioned above.

The main takeaway from this project is undoubtedly how the "onboarding experience" or how "welcoming" the repository/project is to work on makes a big impact on how likely it is for outsider to work on it. We evaluated some other projects before landing on this one because the maintainer seemed like a helpful person. Another takeaway which we noticed during the writing of this report is how little time actually goes into writing code. Most time appears to go to evaulating strategies, planning and reading documentation.