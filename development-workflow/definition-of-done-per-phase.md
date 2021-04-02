# Definition of Done of Development Phase

This document defines definition of what should be achieved per phase to enter the next phase.

For example, to be able to go to Ready phase, each items in Backlog should have clear business requirements.

Another example, for each items to be deployed to Production, it should have technical release notes, passed QA, have technical notes, and code documentation.

The goal of these guidelines is to make sure we **identify things early so that we don't develop premature product which causes hidden cost or unused products**.

## Backlog

In this phase, stories is gathered as business requirements. Every stories should hava a clear description and standard scrum stories definition.

```
As a ..., I should be able to ...
So that, ...
```

Prerequisite to go to Ready phase:

* Business requirements is clear according to System Design guidelines
* Stories should also comes from GIST process

Actors: Tech Lead, Principals, PM

## Ready

In this phase, there'll be technical discussions, and it is expected to be long to produced Ready to pick stories.

Prerequisite to go to In Progress phase:

* Architecture is defined and written in Architectures Decision Record (ADR).
* Has been through UI or UX process.
* Translated into specific technical requiements, from business requirements.
* Requirement is clear, comes from System Design guidelines.
* Acceptance criteria is complete, small, and testable.
* Use case is defined.
* Estimation is given.
* Success metrics is defined.
* Analytics requirements are clear.

Actors: Principals, Tech Lead, Software Engineers, QAs, PM, Infra

Details on how story is better written, can be seen [here](./details/story.md).

## In Progress

In this phase, engineers will pick the stories and start developing features.

Prerequisite to go to QA phase:

* Contract is defined in the form of Swagger.
* Development time should be tracked in Gitlab. Development time is not including QA time.
* The feature is complete according to specification defined in Ready phase.

Actors: Tech Lead, Software Engineers, QAs

## QA

QA can test features when an Epic is done. For example say there're 3 stories per epic those 3 stories should be feature complete first before QA can test.

Prerequisite to go to Done phase:

* Tagged with Version and commit hash. Example v1.0.0-wquyrei.
* The features are feature-complete and runnable.
* Pass code review.
* Pass linter.
* Unit test is 100 percent passed.
* Code documentation is written. Eg. Class has description, public method has description.

Actors: QAs, PM at some case to consult about bug / improvement.

## Done

Prerequisite to go to Deployments:

* Stories passed the QA process.
* Codes finished within the sprint, should be tagged with version according to semantic versioning.
* Should have Technical Release notes.
* [Javascript | Go] doc generated from code documentation.

Actors: PM, Infra, Software Engineers, and SRE

## Notes

- These phases are carried out using Scrum or Kanban.

- Before make it to In-Progress phase, each stories should be thought carefully to identify dependencies with existing systems, pitfalls, etc, so that development phase is smooth. If after it goes to In-Progress, development team find an incompleteness, the story should be brought back to Ready phase for discussions.

- Each items in could be returned to specific phase, for example in QA phase, items could be brought back to In-Progress, if during QA process a bug is found. In progress could be brought back to Ready, if it's later it has some difficulties that needs to be discussed by principals and tech leads.

- If stories has been given estimation time, if it's stuck for more than one day, the team should regroup to decide whether the story is postponed or adjusting the scope of the story.

- Testing can be done right away if several stories or one story has done, and its independent from other stories in another epics.

- If a story in the end doesn't meet success metrics or the story's goal is not achieved as seen in analytics, product team with the help of principals and tech leads should discuss further to evaluate the results.
