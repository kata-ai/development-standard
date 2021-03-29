# Definition of Done of Development Phase

This document defines definition of what should be achieved per phase to enter the next phase.

For example, to be able to go to Ready phase, each items in Backlog should have clear business requirements.

Another example, for each items to be deployed to Production, it should have technical release notes, passed QA, have technical notes, and code documentation.

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
* Acceptance criteria is complete.
* Use case is defined.
* Estimation is given.
* Success metrics is defined.
* Analytics requirements are clear.

Actors: Tech Lead, Software Engineers, QAs, PM, Infra

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
