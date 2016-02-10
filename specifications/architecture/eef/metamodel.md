---
layout: specification
description: EEF Core
---

## User stories

### Sirius specifier

#### US-1 As a Sirius specifier, Zoe wants to create a Properties view

Zoe needs to define a Properties view for her Sirius designer using the EEF meta-model. As the root concept of our EEF meta-model, we have the concept of *EEFViewDescription*. An *EEFViewDescription* has an *identifier* in order to easily identify a specific *EEFViewDescription* instance among all those available. The *EEFViewDescription* has the list of the NsUris of the meta-models used. Since we do not want to have a direct reference to the meta-models used in order to not force their loading, we will only use the NsUri of the meta-models. Those NsUris will be stored in a property named *ePackageNsUris* containing a list of Strings.

The label of the *EEFViewDescription* can be easily defined using the property *labelExpression*. This property will contain an *expression* which will be interpreted using the available interpreters in the EEF runtime. Some of the interpreters currently used by Sirius specifiers will be available. This label of the *EEFViewDescription* will be used as the label of the Eclipse's Workbench View when the *EEFViewDescription* is used an Eclipse View.

In order to compute the value of the expressions, some variables will be accessible. The list of the variables accessible for a given expression will be found using the expression DSL. The input of the View will be available using the property *viewSemanticCandidate* of the EEFViewDescription. For example, using an EClass as an input of the view the label of the view could be computed to display the name of the input EClass with the *labelExpression* "aql:viewSemanticCandidate.name". This expression would rely on the interpreter "aql" and it would use the variables provided by the EEF runtime for its evaluation. For the evaluation of the *labelExpression*, we have here access to the input of the *EEFViewDescription* under the name *viewSemanticCandidate*. All the variables will be referenced using an explicit name.

Concepts from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFViewDescription
* EEFViewDescription#identifier: EString [1..1]
* EEFViewDescription#ePackageNsUris: EString [0..-1]
* EEFViewDescription#labelExpression: Expression [0..1] return String [0..1]

#### US-2 As a Sirius specifier, Zoe wants to define tabs in a view to separate the different concerns

In order to show the various aspects of the objects that Zoe wants to edit, she wants to separate those various aspects using tabs. To represent those tabs, we will use the concept of *EEFPageDescription* in our meta-model. The *EEFPageDescription* will be created in the property *pages* of the *EEFViewDescription*. The *EEFPageDescription* needs to be identifiable using a property named *identifier*. The tab created by the *EEFPageDescription* needs to have a label to be identified by the end users. Zoe will be able to use the property *labelExpression*, in order to be able to compute this label easily.

In our simple use case, our first *EEFPageDescription* will only show the properties of our input EClass. As a result, the label of our *EEFPageDescription* will be computed using the *labelExpression* "General". If we do not have any interpreter register that support specifically the expression typed by the user, we will handle the text entered by the user for a label expression as a String. In our case, we will use the String "General" directly as a String.

Our *EEFPageDescription* needs to specify the kind of element it support, for this need we will work using the property *domainClass*. This property does not referenced directly an EClass of a meta-model since we do not want to load said EClass directly in a similar approach as Eclipse Sirius. In our case, we are using the same object in the *EEFPageDescription* as the input of the *EEFViewDescription* so Zoe will use the *domainClass* "ecore.EClass".

Zoe could use a different input by specifying a *semanticCandidateExpression* for the *EEFPageDescription*. By not specifying it, the EEF runtime will reuse the input of the *EEFViewDescription* for the *EEFPageDescription*. This behavior would be equivalent to using "aql:self" as the *semanticCandidateExpression* of the *EEFPageDescription*.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFViewDescription#pages: EEFPageDescription [1..-1] containment
* EEFPageDescription
* EEFPageDescription#identifier: EString [1..1]
* EEFPageDescription#labelExpression: Expression [0..1] return String [0..1]
* EEFPageDescription#domainClass: TypeName [0..1]
* EEFPageDescription#semanticCandidateExpression: Expression [0..1] return ecore.EObject [0..1]

#### US-3 As a Sirius specifier, Zoe wants to define parts of a *Page* to contain a set of widgets

In order to structure the user interface within an *EEFPageDescription*, Zoe needs to be able to create a group of widgets easily identifiable by the end users. To achieve this need, Zoe will be able to use the concept of *EEFGroupDescription*. An *EEFGroupDescription* contains an *identifier* in order to be easily identified in the *EEFViewDescription*. This parts of the *EEFPageDescription* can have a label to be identifiable by the end users using a *labelExpression* just like the *EEFPageDescription*.

If you use a *labelExpression* for your *EEFGroupDescription* it will create in the user interface a complete section with its label visible. If you do not want to see the *EEFGroupDescription* as a visible structure in the resulting user interface, do not specify any *labelExpression*. As a result, the boundaries of the *EEFGroupDescription* will not be visible in the result, it will be mainly used as a layout structure. Using a similar approach as before, we will create a label for our *EEFGroupDescription* using the String "ENamedElement" which will be used directly as is.

Just like the *EEFPageDescription*, the *EEFGroupDescription* needs to have a *domainClass* in order to specify the context in which this *EEFGroupDescription* will be used. In order to work with an ENamedElement, the *domainClass* will be "ecore.ENamedElement". The *semanticCandidateExpression* will stay empty to keep working using the context of the *EEFPageDescription* where the *EEFGroupDescription* will be used.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFGroupDescription
* EEFGroupDescription#identifier: EString [1..1]
* EEFGroupDescription#labelExpression: Expression [0..1] return String [0..1]
* EEFGroupDescription#domainClass: TypeName [0..1]
* EEFGroupDescription#semanticCandidateExpression: Expression [0..1] return ecore.EObject [0..1]

#### US-4 As a Sirius specifier, Zoe wants factorize the definition of the *Groups*

In order to support the definition of the Properties view of her meta-model, Zoe needs to be able to factorize her work as much as possible. As such, if she has specified how to display a subset of the properties of an EClass, she does not want to have to redefine everything when she will work on displaying one of its subclass.

The concept of *EEFPageDescription* will not contain directly the various *EEFGroupDescriptions* available. They will be defined under the *EEFViewDescription* directly and then the *EEFPageDescriptions* will be able to reuse them easily. As such, the *EEFPageDescription* used to display the instances of EClass should be able to reuse an *EEFGroupDescription* defined for the properties of ENamedElement. Zoe will be able to define an *EEFGroupDescription* for the properties of ENamedElement and this *EEFGroupDescription* will be reused for all the subclasses of ENamedElement. This approach creates an issue since that in order to display an EObject, the specifier would have to manually look for all the *EEFGroupDescriptions* related to its EClass and its parents under the *EEFViewDescription*.

In her case, Zoe will create a second *EEFGroupDescription* using the *labelExpression* "EClass" for the *domainClass* "ecore.EClass". Since the *EEFPageDescription* uses the *domainClass* "ecore.EClass", the *EEFPageDescription* will be able to work with both *EEFGroupDescriptions*. If the *EEFGroupDescriptions* to use for the *EEFPageDescription* are not specifically referenced by the *EEFPageDescription*, the EEF runtime will look for all the *EEFGroupDescriptions* accessible under the *EEFViewDescription* containing the *EEFPageDescription* that have a *domainClass* which is equal or a superclass of the *domainClass* of the *EEFPageDescription* and use them following the order of the inheritance tree of the EObject.

In our case, the *EEFPageDescription* has the *domainClass* "ecore.EClass" and the *EEFViewDescription* contains two *EEFGroupDescriptions*, one for "ecore.ENamedElement" and the other for "ecore.EClass" so since the *EEFPageDescription* does not explicitly reference any *EEFGroupDescription* the runtime will automatically import both *Groups* in the *EEFPageDescription* and it will order them using the inheritance order. As such, since EClass extends ENamedElement, the first *EEFGroupDescription* will be the one named "ENamedElement" and the second one will be the one named "EClass".

If Zoe wants to customize the *EEFGroupDescriptions* used for the *EEFPageDescription* of a specific EClass, she can manually reference the *EEFGroupDescriptions* used by a specific *EEFPageDescriptions*. With this she can also customize the order of appearance of the *EEFGroupDescriptions* for a specific *EEFPageDescription* since, if explicitly specified, the order to the *EEFGroupDescriptions* will follow the order defined in the property *groups* of the *EEFPageDescription*. It will also allow Zoe to prevent the use of specific *EEFGroupDescriptions* for an *EEFPageDescription* by not referencing them among the *EEFGroupDescriptions* explicitly referenced in the property *groups* of the *EEFPageDescription*.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFViewDescription#groups: EEFGroupDescription [0..-1] containment
* EEFPageDescription#groups: EEFGroupDescription [0..-1]

#### US-5 As a Sirius specifier, Zoe wants to add a text widget to display and edit the name of an ENamedElement

We have already seen how the *EEFViewDescription* is structured using *EEFPageDescriptions* and *EEFGroupDescription*, now Zoe wants to add her first widget to display the name of the EClass received as the input of the *EEFViewDescription*. This name will be displayed in the *EEFGroupDescription* used for the properties of ENamedElements. As a first step, she has to create an *EEFContainerDescription* in the *EEFGroupDescription* which will be used to hold the widgets of the ENamedElement. An *EEFContainerDescription* needs to be identified and as such it contains a property *identifier*.

In order to hold the widgets, the *EEFContainerDescription* will use a property named *widgets*. All the widgets that can be used in an *EEFContainerDescription* will extend a common superclass named *EEFWidgetDescription*. All the *EEFWidgetDescriptions* will have an *identifier* and a label computed using a *labelExpression*. In order to display the name of the EClass received as the input of the *EEFViewDescription*, Zoe can use a specific widget described by an *EEFTextDescription*.

An *EEFTextDescription* is a subclass of *EEFWidgetDescription* which will be used to create a text field in the user interface. This text field will be used to display and edit a value. In our use case, this *EEFTextDescription* will be used to edit an EAttribute of type EString but in a similar fashion as Eclipse's Sirius, we could use this field to edit another kind of value. This *EEFTextDescription* will first use the *labelExpression* "Name" in order to indicate to the end users that the field created will be used to edit the name of the EClass. Zoe needs to display the current value of the name of the EClass received as the input of the *EEFViewDescription* and to achieve this she can use the *valueExpression* of the *Text* in order to indicate how the value displayed should be computed. We know that the input of the *EEFViewDescription* is accessible using the variable *viewSemanticCandidate*, we can thus use the *valueExpression* "aql:viewSemanticCandidate.name" to display the name of the EClass in the *EEFTextDescription*.

In order to edit the value of the name of the input EClass, Zoe can use the *editExpression* of the *EEFTextDescription*. This *editExpression* will be executed when the end users will enter some text in the text field. In order to know how to access the value entered by the end users, Zoe will use the variable named "newValue". In our case, Zoe could write the following *editExpression* to edit the name of the EClass "aql:viewSemanticCandidate.eSet('name', newValue)".

Now Zoe has created a *EEFTextDescription* which can be used to display and edit the name of the EClass. This approach is directly inspired by the way a Label and a Direct Edit Tool work in Eclipse Sirius.

Model Concepts for platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFGroupDescription#container: EEFContainerDescription [1..1] containment
* EEFContainerDescription
* EEFContainerDescription#identifier: EString [1..1]
* EEFContainerDescription#widgets: EEFWidgetDescription [1..-1] containment
* EEFWidgetDescription
* EEFWidgetDescription#identifier: EString [1..1]
* EEFWidgetDescription#labelExpression: Expression [0..1] return String [0..1]
* EEFTextDescription
* EEFTextDescription#valueExpression: Expression [0..1] return String [0..1]
* EEFTextDescription#editExpression: Expression [0..1] return void [0..1]

#### US-6 As a Sirius specifier, Zoe wants to add a checkbox *Widget* to display and edit the property abstract of an EClass

Now that we have seen how to use an *EEFTextDescription*, using an *EEFCheckboxDescription* will feel easy. This *EEFCheckboxDescription* will be used to represent an attribute specific to an EClass so it will be defined in the EClass *EEFGroupDescription*. In this *EEFGroupDescription*, Zoe will first define an *EEFContainerDescription* then Zoe will create an *EEFCheckboxDescription* using "Abstract" for its *labelExpression*. In order to display the property abstract of the input EClass, Zoe can use the *valueExpression* of the *EEFCheckboxDescription* with the expression "aql:viewSemanticCandidate.abstract". In a similar fashion as the *EEFTextDescription*, to edit the value of the property abstract, Zoe can define the *editExpression* of the *EEFCheckboxDescription* using the expression "aql:viewSemanticCandidate.eSet('abstract', newValue)".

As usual, just like in Sirius it is possible using a different expression to edit something completely unrelated to a boolean using a *EEFCheckboxDescription*.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFCheckboxDescription
* EEFCheckboxDescription#valueExpression: Expression [0..1] return Boolean [0..1]
* EEFCheckboxDescription#editExpression: Expression [0..1] return void [0..1]

#### US-7 As a Sirius specifier, Zoe wants to add a label widget to display some read only information.

Zoe can use an *EEFLabelDescription* in order to add a read only piece of text in the *EEFContainerDescription*. In the *EEFContainerDescription* of the first *EEFGroupDescription*, Zoe will add an *EEFLabelDescription* with the *labelExpression* "Description". The body of the label will contain in our case a piece of static text (not a computed one) using the *valueExpression* "This is the label description".

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFLabelDescription
* EEFLabelDescription#valueExpression: Expression [0..1]

###### US-8 As a Sirius specifier, Zoe wants to add a hyperlink in order to execute an action

Zoe can create an *EEFLinkDescription* in the *EEFContainerDescription* of her first *EEFGroupDescription* in order to display a label with an action. The label of the *EEFLinkDescription* will be computed using the property *labelExpression* since *EEFLinkDescription* extends *EEFWidgetDescription*. When the user will click on the link, the behavior specified in the property *onClickExpression* will be executed. Here Zoe will use the expression "Open documentation" as the *labelExpression* of the *EEFLinkDescription* and "aql:viewSemanticCandidate.openDocumentation()" for the *onClickExpression*.

The service "openDocumentation" does not exist on an ENamedElement neither on EClass so Zoe will need to contribute it to this definition. For that, she can use the concept of *EEFJavaExtensionDescription* in a similar manner as in Eclipse Sirius. An *EEFJavaExtensionDescription* can be created in the *EEFViewDescription* using the property *javaExtensions* and it should contain a *qualifiedName*. In our case, Zoe can create an *EEFJavaExtensionDescription* with the *qualifiedName* "org.eclipse.eef.myview.Service". Zoe will have to create the Java class with the same qualified name in the project containing her EEF *EEFViewDescription* model and in this class, she will have to create a method with the following signature:

{% highlight java linenos %}
public class Service {
  public void openDocumentation(ENamedElement eNamedElement) {
    // insert here the code used to open a new editor with the documentation
  }
}
{% endhighlight %}

This service will be registered and available for the various interpreters. With the AQL interpreter, this service will be register as a regular service available on all ENamedElements. When the end user will click on the link, the method "openDocumentation" will be called.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFLinkDescription
* EEFLinkDescription#onClickExpression: Expression [0..1] return void [0..1]
* EEFJavaExtensionDescription
* EEFJavaExtensionDescription#qualifiedName: EString [0..1]
* EEFViewDescription#javaExtensions: EEFJavaExtensionDescription [0..-1] containment

#### US-9 As a Sirius specifier, Zoe wants to add an image to her *Group*.

In order to display an image, Zoe can add an *EEFImageDescription*. The *EEFImageDescription* will display an image using the URI computed from its *valueExpression*. In our example, Zoe will use "aql:'platform:/plugins/org.eclipse.emf/icons/obj16/EClass.gif'". The URI entered by the specifier will require a scheme, here "platform". By using this scheme, the EEF runtime will consider the URI as an EMF URI and it would look for the image in the plugin "org.eclipse.emf".

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFImageDescription
* EEFImageDescription#valueExpression: Expression [0..1] return String [0..1]

#### US-10 As a Sirius specifier, Zoe wants to display and edit an image

We have seen how Zoe can display an image but if she wants to let the end user edit the image in question, she can use an *EEFImagePickerDescription*. The *EEFImagePickerDescription* is a widget which can display and image and let the end user modify it. To display an image, Zoe can use the *valueExpression* of the *EEFImagePickerDescription*. This *valueExpression* will have the same behavior as the *valueExpression* of the *EEFImageDescription*.

The end user will need to select a new image among a collection of images. This collection will be computed using the URIs returned by the *candidatesExpression*. In order to display those images, Zoe will use the expression "aql:viewSemanticCandidate.getCandidateImages()". To update the value of the image, Zoe will use the variable with the name "newValue" and an *editExpression* using the expression "aql:viewSemanticCandidate.updateImage(newValue)". This expression will also use a method of our Java extension.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFImagePickerDescription
* EEFImagePickerDescription#valueExpression: Expression [0..1] return String [0..1]
* EEFImagePickerDescription#candidatesExpression: Expression [0..1] return String [0..1]

#### US-11 As a Sirius specifier, Zoe wants to display and edit a value using a radio widget

In order to use a radio widget, Zoe can use an *EEFRadioDescription*. For that, Zoe needs to start by specifying the various proposals available using the property *candidatesExpression*. In our case, Zoe will enter the expression "aql:Sequence{'public', 'protected', 'private'}". Those proposals will not be displayed as is in the user interface. To determine how the proposals will be displayed, Zoe will use the property *candidateDisplayExpression" along with the variable *candidate*. Zoe will thus use the expression "aql:candidate + 'class'" for the *candidateDisplayExpression*. As a result, the three proposals will be "public class", "protected class" and "private class".

In order to initialize the value of the *Radio*, Zoe will have access to the property *valueExpression*. This expression will be used to select on the existing candidate returned by the *candidatesExpression*. In this situation, the *valueExpression* will be "aql:viewSemanticCandidate.getVisibility()". When the end user will select a value, the expression defined in the property *editExpression* will be executed. This expression will have access to the variable *newValue*. In our use case, the *editExpression* will be "aql:viewSemanticCandidate.updateVisibility(newValue)".

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFRadioDescription
* EEFRadioDescription#candidatesExpression: Expression [0..1] return Object [0..-1]
* EEFRadioDescription#candidateDisplayExpression: Expression [0..1] return String [0..1]
* EEFRadioDescription#valueExpression: Expression [0..1] return Object [0..1]
* EEFRadioDescription#editExpression: Expression [0..1] return void [0..1]

#### US-12 As a Sirius specifier, Zoe wants to display and edit some text in a text area.

We have previously define how to edit some text in an *EEFTextDescription*. In order to display the value in a text are instead, we can change the property *lineCount* of the *EEFTextDescription*. By default, this property has the value 1. If the *lineCount* is an integer bigger than 1, a text area will be used instead of a text field in the user interface. An *EEFTextDescription* could be used to display the documentation of the input EClass using "aql:viewSemanticCandidate.getDocumentation()" as the *valueExpression*. This documentation could be updated using the variable "newValue" and the *editExpression* "aql:viewSemanticCandidate.updateDocumentation(newValue)". This *EEFTextDescription* could use a *lineCount* of 4.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFTextDescription#lineCount: EInt [0..1]

#### US-13 As a Sirius specifier, Zoe wants to edit a value with multiple choices using a select box.

The EEF meta-model comes with a concept of *EEFSelectDescription*, a *Widget* used to display a collection of candidate used to edit a single or multi-valued property. In her use case, Zoe wants to edit the superclasses of her input EClass. For that, Zoe will create an *EEFSelectDescription* with *multiple* equal to true and using "aql:viewSemanticCandidate.eSuperTypes" as the *valueExpression*. In order to find out all the possible candidates, Zoe can used an *candidatesExpression* with "aql:viewSemanticCandidate.eResource().eAllContents(ecore::EClass)". This expression will return all the EClass in the same resource as the input EClass. In order to display them, Zoe can use the variable named "candidate" and "aql:candidate.name" as a *candidateDisplayExpression*.

To edit the value, Zoe can use the variable "newValue" which will contain the values selected by the end user and the expression "aql:viewSemanticCandidate.updateSupertypes(newValue)" as an *editExpression*.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFSelectDescription
* EEFSelectDescription#multiple: EBoolean [0..1]
* EEFSelectDescription#valueExpression: Expression [0..1] return Object [0..1]
* EEFSelectDescription#candidatesExpression: Expression [0..1] return Object [0..-1]
* EEFSelectDescription#candidateDisplayExpression: Expression [0..1] return String [0..1]
* EEFSelectDescription#editExpression: Expression [0..1] return void [0..1]

#### US-14 As a Sirius specifier, Zoe wants to let the user select a value in a tree.

In order to let the end users select a value in a tree, Zoe will first define the structure of a tree and then use it in the widget. As such, the tree described can be reused in multiple widgets after that. In her use case, Zoe will create an *EEFTreeStructureDescription* in order to define how the final tree will look like. The *EEFTreeStructureDescription* will be created in the property *treeStructures* of the *View*.

The *EEFTreeStructureDescription* is an abstract concept with two different implementations. The *EEFInterpretedTreeStructureDescription* which will define the structure of the tree using various expressions and variables and the *EEFAdapterFactoryTreeStructureDescription* which will let the specifier create a tree initialized from an AdapterFactory. In this use case, Zoe will work with an *EEFInterpretedTreeStructureDescription*. She will first give it an *identifier* and set *multiple* to true.

Zoe will use the variable "treeSemanticCandidate" to retrieve the input value of the tree. Zoe will use the property *rootsExpression* to define the collection of elements that should appear as the root of the tree. In her case, she will use the expression "aql:treeSemanticCandidate" to have the current EClass as the root. To compute the child to display in the tree, Zoe can use "aql:parent.eContents()" as an *childrenExpression* along with the variable named "parent". To display those elements in the tree, Zoe can use "aql:candidate.name" as an *candidateDisplayExpression* and the variable "candidate". Finally, to determine the elements that can be selected, Zoe can provide "aql:candidate.ancestors().contains(treeSemanticCandidate)" as the *selectablePredicateExpression*.

In order to use the *EEFInterpretedTreeStructureDescription*, Zoe will have to create an *EEFTreeDescription*. This tree will first referenced the existing *EEFInterpretedTreeStructureDescription*, then she will set the *valueExpression* to the expression "aql:viewSemanticCandidate.getRelatedObjects()" in order to return the related Objects that should be selected in the user interface. In order to edit the value of the related objects when the user modify the selection, the value will be captured using the variable named "newValue" and an *editExpression* "aql:viewSemanticCandidate.updateRelatedObjects(newValue)".

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFTreeStructureDescription
* EEFTreeStructureDescription#identifier: EString [1..1]
* EEFTreeStructureDescription#multiple: EBoolean [0..1]
* EEFViewDescription#treeStructures: EEFTreeStructureDescription [0..-1] containment
* EEFInterpretedTreeStructureDescription
* EEFInterpretedTreeStructureDescription#rootsExpression: Expression [0..1] return Object [0..-1]
* EEFInterpretedTreeStructureDescription#childrenExpression: Expression [0..1] return Object [0..-1]
* EEFInterpretedTreeStructureDescription#candidateDisplayExpression: Expression [0..1] return String [0..1]
* EEFInterpretedTreeStructureDescription#selectablePredicateExpression: Expression [0..1] return Boolean [0..1]
* EEFAdapterFactoryTreeStructureDescription
* EEFTreeDescription
* EEFTreeDescription#treeStructure: EEFTreeStructureDescription [0..1]
* EEFTreeDescription#valueExpression: Expression [0..1]
* EEFTreeDescription#editExpression: Expression [0..1]

#### US-15 As a Sirius specifier, Zoe wants to reuse a tree structure to display a dialog

In order to create a dialog showing a tree, Zoe can use the concept of *EEFTreeDialogSelectDescription* which can leverage an existing *EEFTreeStructureDescription*. Contrary to the *EEFTreeDescription*, the *EEFTreeDialogSelectDescription* can change its current value using the property *domainClass* and a *semanticCandidateExpression*. In our case, we will use "ecore.EClass" as the *domainClass* and "aql:viewSemanticCandidate" as the *semanticCandidateExpression* to keep working on the same context as our view.

In order to display a *EEFTreeDialogSelectDescription* with meaningful pieces of information, Zoe can use a *titleExpression* to determine the title of the dialog and a *descriptionExpression* for the text that will be written in the dialog. The *EEFTreeDialogSelectDescription* will reuse an existing *EEFTreeStructureDescription* thanks to the property *treeStructure*. The dialog will also contain a text field used to filter the content of the tree. This filter will use the *defaultFilterExpression* to populate the default value which will be "*" by default.

In order to select the existing value, this widget will use, as usual, a property *valueExpression* and to edit it, it will rely on the variable "newValue" and the *editExpression*.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFTreeDialogSelectDescription
* EEFTreeDialogSelectDescription#titleExpression: Expression [0..1]
* EEFTreeDialogSelectDescription#descriptionExpression: Expression [0..1] return String [0..1]
* EEFTreeDialogSelectDescription#treeStructure: EEFTreeStructureDescription [0..1]
* EEFTreeDialogSelectDescription#valueExpression: Expression [0..1]
* EEFTreeDialogSelectDescription#editExpression: Expression [0..1]
* EEFTreeDialogSelectDescription#defaultFilterExpression: Expression [0..1] = * return String [0..1]
* EEFTreeDialogSelectDescription#domainClass: TypeName [0..1]
* EEFTreeDialogSelectDescription#semanticCandidateExpression: Expression [0..1] return ecore.EObject [0..1]


#### US-16 As a Sirius specifier, Zoe wants to define a table

In order to create a table, Zoe will have to start by defining its structure and then to reuse it within an *EEFTableDescription*. Zoe will thus start with a subclass of the *EEFTableStructureDescription* named *EEFInterpretedTableStructureDescription*. This description will be created directly in the field *tableStructures* of the *EEFViewDescription*. The description contains a field named *multiple*, used to indicate if selecting multiple elements is allowed and an *identifier*. Since she will use an *EEFInterpretedTableStructureDescription*, Zoe will have to specify all the behavior declaratively in the DSL.

There are two parts to define in the *EEFInterpretedTableStructureDescription*, first the behavior of the *columns* used and then the behavior of the *line*. An *EEFColumnDescription* can have a *width* and a *headerExpression* used to compute the label of the column. The column also contain a widget using the field *cellWidget* in order to define how the content of the cell should be displayed.

In the *EEFLineDescription*, Zoe will have to specify the input of the table structure using the *semanticCandidatesExpression* and the *domainClass*. The header of the line can be computed using the *headerExpression*. Finally, a line can also have *subLines*.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFViewDescription#tableStructures: EEFTableStructureDescription [0..-1] containment
* EEFTableStructureDescription
* EEFTableStructureDescription#multiple: EBoolean [0..1]
* EEFTableStructureDescription#identifier: EString [1..1]
* EEFInterpretedTableStructureDescription
* EEFInterpretedTableStructureDescription#columns: EEFColumnDescription [0..-1] containment
* EEFInterpretedTableStructureDescription#line: EEFLineDescription [0..1] containment
* EEFColumnDescription
* EEFColumnDescription#width: EInt [0..1]
* EEFColumnDescription#headerExpression: Expression [0..1] return String [0..1]
* EEFColumnDescription#semanticCandidatesExpression: Expression [0..1] return Object [0..1]
* EEFColumnDescription#semanticCandidateVariable: Variable [0..1] containment
* EEFLineDescription
* EEFLineDescription#headerExpression: Expression [0..1] return String [0..1]
* EEFLineDescription#semanticCandidatesExpression: Expression [0..1] return Object [0..-1]
* EEFLineDescription#domainClass: EString [0..1]
* EEFLineDescription#subLines: EEFLineDescription [0..-1] containment

#### US-17 As a Sirius specifier, Zoe wants to reuse the groups used for a specific page

In the previous user story, we have defined how the page will reuse the groups as specified by the Sirius specifier. Now let's consider a scenario where Zoe has already customized the groups used by some pages for Ecore and now she wants to define the pages for Genmodel but she wants to reuse the custom set of groups defined in some existing pages. In order to achieve this, Zoe can import other views in the definition of her *EEFViewDescription* using the property *importedViews* and she can then extend an existing page to retrieve the specific configuration of the groups of the page. She can extends the set of groups used in the extended page by specifying new groups to use additionally in her page.

As such, she can define a page for the concept GenClass which can contain a group for the properties of GenClass and which extends an existing page defined in another view to display EClass. This page used to display an EClass had already defined a set of groups that should be displayed in a specific order using for example a group for EClass followed by a group for EClassifier. As a result, the final page would have firstly the group for EClass, secondly the group for EClassifier and thirdly the group for GenClass.

Concept from platform:/resources/org.eclipse.eef/model/eef.ecore

* EEFViewDescription#importedViews: View [0..-1]
* EEFPageDescription#extendedPage: Page [0..1]


#### US-18 As a Sirius specifier, Zoe wants to define a custom widget to edit her model


#### US-19 As a Sirius specifier, Zoe wants to define her layout using multiple columns

#### US-20 As a Sirius specifier, Zoe wants to define how some widgets can be read only

#### US-21 As a Sirius specifier, Zoe wants to override the creation of a specific element

#### US-22 As a Sirius specifier, Zoe wants to create a wizard

#### US-23 As a Sirius specifier, Zoe wants to provide content assist for her text fields

#### US-24 As a Sirius specifier, Zoe wants to provide some tooltips for her widgets

#### US-25 As a Sirius specifier, Zoe wants to edit some custom EDataTypes with validation

See http://eclipsesource.com/blogs/2014/08/26/emf-validation-for-datatype-constraints/

#### US-26 As a Sirius specifier, Zoe wants to edit the style of her text fields

#### US-27 As a Sirius specifier, Zoe wants to add actions in the view

show advanced, restore default value, see related elements, etc

#### US-28 As a Sirius specifier, Zoe wants to customize the behavior of the tools add, remove, up, down of a table