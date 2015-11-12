---
layout: meetingminutes
title: 2015-11-12
description: Planning and status of the project
---
##### Goal

The goal of this meeting is to define the status of the project and the planning of the next milestones.

##### Participants

* Mélanie Bats
* Stéphane Bégaudeau
* Pierre-Charles David

##### What is necessary for a minimum viable product?

- widgets?
- features? (validation, quick fixes, style, etc)
- interpreter integration? (aql, var, service, feature)
- transaction? (EMF transaction & cdo friendly architecture?)
- context? (reaction to the activation of a viewpoint, selection in a specific diagram)
- links to the Sirius DSL? (context, expression)
- link with EMF edit? (directly or thanks to expressions?)
- integration with the Sirius expression language? (yes, no, how, where)
- default behavior? (and how to customize it)
- model explorer view support? (selection of any EObject)
- support for custom EDataType?

##### User stories

- Define a Properties view for the odesign file editor
- Add a text field to edit the name of my properties
- Integration with EMF Edit?

##### What should be done to obtain this minimum viable product?

- Continuous integration & Gerrit
- Sirius integration (metamodel & transaction)
- Restrained number of widgets: text, checkbox, selectbox (multiple and single)
- Context -> viewpoint, representation, layer, filter, mapping
- Interpreter (AQL)
- Lower priority for the tooling -> potential issues for the user stories related to EMF / EMF Edit (diagnostician, choice of values, etc)

##### Planning

- Eclipse Neon M4 2015-12-11
- Eclipse Neon M5 2016-01-29
- Eclipse Neon M6 2016-03-25
