---
layout: meetingminutes
title: 2015-11-13
description: User Stories of the MVP
---
##### Goal

The goal of this meeting is to define the user stories of the minimum viable product.

##### Participants

* Mélanie Bats
* Stéphane Bégaudeau
* Pierre-Charles David

##### User Stories

###### MVP-1 As an end user, Lucas wants to see a Properties view when he clicks on a DNode

###### MVP-2 As an end user, Lucas wants to see a Properties view when he clicks on a EObject in the Model Explorer view

###### MVP-3 As an end user, Lucas wants to see changes in the Properties view when he modifies an DNode in a diagram

###### MVP-4 As an end user, Lucas wants to see changes in the diagram when he modifies an DNode in the Properties view

###### MVP-5 As an end user, Lucas wants to edit the common properties of multiple EObjects at once

###### MVP-6 As a Sirius specifier, Zoe wants to edit the definition of her Properties view in the odesign

###### MVP-7 As a Sirius specifier, Zoe wants to have a Properties view computed by default for her metamodel

→ Reuse EMF Edit by default (validation, choice of value, label provider, etc)

###### MVP-8 As a Sirius specifier, Zoe wants to override parts of the Properties view computed by default

If Zoe has not defined anything → default behavior
If Zoe has defined a specific Page → We display the Page
If Zoe has defined some dynamic mappings → We display the dynamic mappings
If Zoe has defined some specific Page and some dynamic mappings → We display the dynamic mappings first, then the specific Page

Example: Zoe wants to edit Ecore models

Scenario 1
Zoe does not define how her Properties view should appear and she wants to edit an EAttribute. The user interface should display what’s necessary to edit the various properties of EAttribute following the EStructuralFeatures available in the inheritance tree of EAttribute. For example, a widget for eAnnotation, then a widget for name, then a widget for the lower bound and upper bound, then a widget for the type.

Scenario 2
Zoe does not like this default behavior, she wants to customize everything manually. She can define a page for the domain class EAttribute. She will create three groups under the view and she will import them in her page. The first group will be used to edit the name of the EAttribute, the second one will be used to edit the lower bound and upper bound and the third one will be used to edit the type of the EAttribute. She can control everything and if the has to create another page for the EReference, her custom-made groups for the name and for the bounds can be reused automatically.

Zoe should be able to specify a page and group to contain said dynamic mappings

Scenario 3
Zoe has a shitty metamodel and as a result, some structural features share the same name and type (and meaning) but they exist in various EClasses without any common superclass. Let us imagine for a minute if the structural feature name was not in ENamedElement in Ecore but on each EClass of Ecore (EClass, EAttribute, EReference, etc). As a result, Zoe wants to specify that we should create a text field to edit the name if an EClass contains a EAttribute named name. This dynamic mapping would appear instead of the default behavior.

If Zoe has specified both a static mapping with groups for the bounds and type of the EAttribute and a dynamic mapping for all the EObject with a structural feature named name and with the type EString, both should appear and work together.

###### MVP-8.1 As a Sirius specifier, Zoe wants to specify manually the structure of her Properties view

###### MVP-8.2 As a Sirius specifier, Zoe wants to specify (polymorphic) a text field for all the EAttribute with the type EString

###### MVP-9 As a Sirius specifier, Zoe wants to specify a group for a superclass and see it reused automatically for all the subclass of said superclass

###### MVP-10 As an end user, Lucas want to edit some properties using a text field

###### MVP-11 As an end user, Lucas want to edit some properties using a checkbox

###### MVP-12 As an end user, Lucas want to edit some properties using a combo

###### MVP-13 As a Sirius specifier, Zoe wants to specify some pages, groups and widgets specifically for a viewpoint, representation, layer, filter or mapping

###### MVP-14 As an EEF contributor, Claire wants to have continuous integration connected to gerrit to monitor the project
