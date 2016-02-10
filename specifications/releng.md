---
layout: specification
description: Continuous Integration
---

In order to maintain the project, we need continuous integration.

##### User Stories

###### Claire - EEF Developer

As Claire, I need to be able to contribute to the project easily by having a continuous integration server. I need to be able to manage code reviews and I need a reproducible build.

###### David - Release Train Coordinator

As a release train coordinator, David needs:

* bundle with all the required files
* bundle properly signed
* features with required files
* correct bundle versions and feature versions
* proper description in the update site

###### Sharon - IP Manager

As an intellectual property manager, Sharon needs:

* up-to-date copyright on all source code files
* license in the bundles
* IP log contributed

###### Francis - Sirius Developer

As a Sirius developer, Francis needs to be able to contribute to the project easily to submit fixes. He also needs to retrieve nightly builds to integrate them in his project.

##### Functional Specification

In order to have continuous integration of my project we need:

* Maven and Tycho-based build
* Junit-based unit tests running in the build
* Checkstyle-based static analysis
* Hudson-based build for the branch
* Hudson-based build for Gerrit
* Promotion on a nightly update site
* Aggregation in the Simultaneous Release

The EEF project could also be moved to /modeling/eef in order to have the liberty to choose what we can add in our Bugzilla. Gerrit would also be a must have on the project.

* [Move EEF to modeling/eef](https://bugs.eclipse.org/bugs/show_bug.cgi?id=473512)
* [Gerrit for EEF](https://bugs.eclipse.org/bugs/show_bug.cgi?id=473516)
