---
layout: specification
description: Validation
---
We need to be able to validate the data entered by the end users.

##### User stories

###### US-1 As an end user, Lucas wants to have validation for every kind of editable fields

Lucas should have validation information for every kind of fields that he can edit. This validation should be available with at least a message using a color specific to the kind of message received. It should be possible to receive multiple kinds of validation notifications.

###### US-2 As an EEF Developer, Claire wants to provide validation information based on EMF edit

Claire should be able to provide validation information by reusing EMF Edit.

###### US-3 As a Sirius Specifier, Zoe wants to provide additional validation notifications on a widget

Zoe should be able to provide additional validation notifications. She needs to specify a new *EEFValidation* on the *EEFWidgetDescription* hold by the *validation* reference. An *EEFValidation* provides a *validationExpression* which returns false when a widget is invalid. The EEF runtime highlights the invalid widget by changing the background color and adding an error level icon before the invalid field. The validation notifications must be contributed also to the Problems view.

TODO  Notifications must be visible on the group, I don't remember also what we say about this.

New concepts:

* EEFValidation
* EEFWidgetDescription#validation: EEFValidation [0..1] containment
* EEFValidation#validationExpression: Expression [0..1] containment

###### US-4 As a Sirius Specifier, Zoe wants to provide additional validation notifications on a group

TODO I don't remember why we put validation also on an EEFGroup

New concepts:

* EEFGroupDescription#validation: EEFValidation [0..1] containment

###### US-5 As a Sirius Specifier, Zoe wants to provide quickfixes

Zoe should be able to provide quickfixes. She needs to specify a new *EEFQuickfix* on the *EEFValidation* hold by the *validation* reference. Zoe can provide a *titleExpression* to specify a title for the quick fix. She can set also the description of the quickfix by setting the *messageExpression*. She can specify if a quick fix can be applicable thanks to the *canHandleExpression*. Finally, a *fixExpression* allows to specify the behavior that must be called when the quickfix is executed.

New concepts:

* EEFQuickFix
* EEFQuickFix#titleExpression: Expression [0..1] containment
* EEFQuickFix#messageExpression: Expression [0..1] containment
* EEFQuickFix#canHandleExpression: Expression [0..1] containment
* EEFQuickFix#fixExpression: Expression [0..1] containment

##### Functional Specification

##### TODO

How to get the validation result in order to use it in other expressions as in conditional style expressions ? For example, I want to create a conditional style to set the widget background to red when the widget is invalid, I would need access to an isValid variable.  
