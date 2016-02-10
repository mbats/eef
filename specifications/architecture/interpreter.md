---
layout: specification
description: Interpreter
---

## Sirius interpreter API

## Variable manager

## EMF Edit support


The purpose of this page is to specify how Sirius can support by default some features provided by EMF Edit.
EMF Edit generates item providers. The Ecore specifier can modify the default generated behavior of the item providers. Our purpose is to ease the integration and usage of EMF Edit in the VSM.

The following user stories will help us define how we want to integrate Eclipse EEF 2.0 with EMF.

See [#482831](https://bugs.eclipse.org/bugs/show_bug.cgi?id=482831 "482831") on the Sirius Bugzilla.

##### US-1 As a Sirius specifier, Zoe wants to leverage the label provider specified in her item providers

###### Label expression on style

On a mapping, the Sirius specifier needs to set the *Label Expression*. By default in Sirius this expression is set to *feature:name*.
To support by default, EMF edit, this expression should be set by default to *aql:self.getLabel()* with :

- *self* represents the semantic element associated to the mapping.
- *getLabel()* is a new service in AQL which calls accordingly to the EObject type the associated *XXXItemProvider#getText(Object object)* method.

In the properties view description *properties.ecore*, each *Label Expression* will also be set by default to *aql:self.getLabel()*.

###### Direct edit tool

For complex edit tools, the specifier usually needs to call Java services from the *Input Label Expression* or under the *Initial Operation*. In this cases, the *getLabel* method should be also easily accessible from the Java services.

To create a direct edit tool, the specifier needs to get the structural features which must be updated. Sirius should provide another method *getLabelFeature()* to be able to get easily the feature associated to the label. Then the specifier can create an edit tool as following :
Begin
-> Set
--> Feature Name : *aql:self.getLabelFeature()*
--> Value Expression : *newValue*

*aql:self.getLabelFeature()* might be the default value set by Sirius when a new direct edit tool is created. 

Today in the VSM, the *feature name* is not an interpreted expression.

###### Style text

Sirius should support the EMF styled text by providing a *getStyledLabel()* service which calls the getStyledText method of the item providers.

###### Color provider

It should be possible to reuse the *Item Color Provider*, and Sirius should provide *getForeground()* and *getBackground()* services.
For example, by default when the specifier creates a new Text widget Style, the foreground expression will be set to *aql:self.getForeground()*.

##### US-2 As a Sirius specifier, Zoe wants to leverage the image provider specified in her item providers

The specifier can define *Workspace Image* style for a mapping, he must specify a *Workspace Path* to find the associated image.
This field in the VSM should be an interpreted expression with a default value equals to *aql:self.getImage()*.

The *getImage* service should call accordingly to the EObject type the associated *XXXItemProvider#getImage(Object object)* method.
/!\ The *XXXItemProvider#getImage* method returns any kind of Object.

It should be possible also to use the *getImage* service from the *Icon Path* field of a container or node style. In this case this field should be also an interpreted expression.

This *getImage* service might be used also from the properties view description on a style to get the icon path associated to a widget description.

##### US-3 As a Sirius specifier, Zoe wants to leverage the choice of values specified in her item providers

When the specifier defines an unsynchronized diagram, he usually needs to create an "Add existing elements" or "Add related elements" tools. To create this kind of tools, it could be interesting to use a new AQL service *getChoiceOfValues* which returns the choice of values defined in EMF Edit.

In the properties view description, when the specifier creates a *combo* or a *radio* widget, the *candidatesExpression* will be set to *aql:self.getChoiceOfValues()*  by default.

##### US-4 As a Sirius specifier, Zoe wants to leverage the validation rules already defined using EMF

EMF edit provides a diagnostician. By default Sirius use the EMF Edit validation when a validation is launched.
The diagnostician provides the error level and message associated to a model validation issue.
Our purpose is to be able to attach the validation error message to a specific widget description.

As the widgets are not directly associated to structural features but with interpreted expression, how do we attached the validation issues to specific widgets ?
By default, EEF will show the validation issue at the top of the page.
In order to attach the validation messages on a widget, the specifier will need to specify a validation expression which filters the diagnostician result.

The validation should be called by the edit tools.

Sirius should support also by default EDataType validation (see [this](http://eclipsesource.com/blogs/2014/08/26/emf-validation-for-datatype-constraints/)).

##### US-5 As a Sirius specifier, Zoe wants to leverage the element creation specified in her item providers

When the specifier defines a new *Create Instance*, by default Sirius will call the *createAddCommand* defined in the item providers.

##### US-6 As a Sirius specifier, Zoe wants to leverage the element setter specified in her item providers

When the specifier defines a new *Set*, by default Sirius will call the *createSetCommand* defined in the item providers.

##### US-7 As a Sirius specifier, Zoe wants to leverage the element deletion specified in her item providers

When the specifier defines a new *Remove*, by default Sirius will call the *createDeleteCommand* defined in the item providers.

##### US-8 As a Sirius specifier, Zoe wants to leverage the element move specified in her item providers

When the specifier defines a new *Move*, by default Sirius will call the *createMoveCommand* defined in the item providers.