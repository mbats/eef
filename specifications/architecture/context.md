---
layout: specification
description: Contexts
---
We need to be able to provide to the end users specific properties view depending on contexts.

##### User stories

###### US-1 As an end user, Lucas wants to see a properties view only when a viewpoint is activated

Lucas needs to see specific properties view when he activates a viewpoint in Sirius.

###### US-2 As an end user, Lucas wants to see a properties view only when a layer is activated

Lucas needs to see specific properties view when he activates a layer in Sirius diagram.

###### US-3 As a Sirius specifier, Zoe wants to define a properties view visible depending on a viewpoint activation

Zoe needs to specify that a view is visible only when the 'Review' viewpoint is activated in Sirius. She creates a new *UserDefinedVariable* named 'viewpointVariable' which is hold by the *userDefinedVariables* reference in the *EEFViewDescription*. This variable references the *Viewpoint* EObject defined in the ecore.odesign. We can create user defined variables on *EEFPageDescription*, *EEFGroupDescription*, *EEFContainerDescription* and *EEFWidgetDescription*.
Then Zoe sets the *preconditionExpression* of the *EEFViewDescription* to : "aql:viewpointVariable.isActive()". Finally she provides a java service which checks if a given viewpoint is active or not. This means that if the viewpointVariable exists and is active then the specified properties view will be visible. EEF allows to create same kind of precondition on *EEFPageDescription*, *EEFGroupDescription*, *EEFContainerDescription* and *EEFWidgetDescription*. This will have consequences on the visibility of the page, group, container and widget.

New concepts:

* EEFViewDescription#userDefinedVariables: UserDefinedVariable [0..-1] containment
* EEFViewDescription#userDefinedVariables: UserDefinedVariable [0..-1] containment
* EEFPageDescription#userDefinedVariables: UserDefinedVariable [0..-1] containment
* EEFGroupDescription#userDefinedVariables: UserDefinedVariable [0..-1] containment
* EEFContainerDescription#userDefinedVariables: UserDefinedVariable [0..-1] containment
* EEFWidgetDescription#userDefinedVariables: UserDefinedVariable [0..-1] containment
* EEFViewDescription#preconditionExpression: Expression [0..1] containment
* EEFPageDescription#preconditionExpression: Expression [0..1] containment
* EEFGroupDescription#preconditionExpression: Expression [0..1] containment
* EEFContainerDescription#preconditionExpression: Expression [0..1] containment
* EEFWidgetDescription#preconditionExpression: Expression [0..1] containment

##### Functional Specification

The EEF tooling must provide an easy integration with Sirius components as viewpoints, layers and mappings.

##### TODO

What can be defined as UserDefinedVariable ? Only EObject ? See userDefinedVariable.expression.
																