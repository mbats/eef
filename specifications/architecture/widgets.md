---
layout: specification
description: Widgets
---

## Default widgets

### Implementation

ViewDescriptionConverter / LifecycleManager / Controller / Consumer

### Default behavior

##### Default behavior of properties.ecore

By default if no properties view description is provided, EEF creates one properties view tab named *General*. 

One section per semantic element associated to the selection will be also created.

Each structural features should have a default widget.
The structural features are represented according to the order of the structural features in the EClass.

The following table defines how each structural features will be represented by default:


| Structural feature | Widget                   |
| ------------------ | ------------------------ |
| Single EAttribute  |                          |
|  EChar             | Text                     |
|  EString           | Text                     |
|  EInt              | Text                     |
|  EDouble           | Text                     |
|  EShort            | Text                     |
|  ELong             | Text                     |
|  EFloat            | Text                     |
|  EBigInteger       | Text                     |
|  EBigDecimal       | Text                     |
|  EByte             | Text                     |
|  EByteArray        | Text                     |
|  EBoolean          | Checkbox                 |
|  EEnum             | Radio or Combo           |
|  EDate             | DatePicker               |
|  EList             | MultipleValuesEditor     |
| Many EAttributes   | MultipleValuesEditor     |
| Single Containment | SingleReferenceViewer    |
| Many Containments  | MultipleReferencesViewer |
| Single EReference  | SingleReferenceViewer    |
| Many EReferences   | MultipleReferencesViewer |
| Default            | Text                     |

Non changeable attributes and derived features are represented as read-only by default.

###### Default widgets

* Text :
![Text]({{ site.baseurl }}specifications/images/default-behavior/Text.png "Text")
* Checkbox :
![Checkbox]({{ site.baseurl }}specifications/images/default-behavior/Checkbox.png "Checkbox")
* Radio :
![Radio]({{ site.baseurl }}specifications/images/default-behavior/Radio.png "Radio")
* DatePicker :
![DatePicker]({{ site.baseurl }}specifications/images/default-behavior/DatePicker.png "DatePicker")
* MultipleValuesEditor :
![MultipleValuesEditor]({{ site.baseurl }}specifications/images/default-behavior/MultipleValuesEditor.png "MultipleValuesEditor")
* SingleReferenceViewer :
![SingleReferenceViewer]({{ site.baseurl }}specifications/images/default-behavior/SingleReferenceViewer.png "SingleReferenceViewer")
* MultipleReferencesViewer :
![MultipleReferencesViewer]({{ site.baseurl }}specifications/images/default-behavior/MultipleReferencesViewer.png "MultipleReferencesViewer")



## Custom widgets

#### Single reference

##### User Stories

###### US-1 As a Sirius specifier, Zoe wants to provide a widget to represent a single reference

Zoe needs to represent a single reference in her properties view. She uses the single reference complex widget. 

##### Proposed solutions

This widget is composed by:

- a label : a `labelExpression` much be set, provides the label value
- a text field : `valueExpression` much be set, provides the current value
- the text field contains an hyperlink : `onClickExpression` is optional, provides a way to call a specific operation (open a wizard, select the element in the explorer...). The operation must be provided by the Sirius specifier.
- a create button : `createExpression` is optional, if not set the button does not appear, provides a way to call a specific operation (creation wizard...). The operation must be provided by the Sirius specifier. In case of containment reference, the expression needs to reference the current semantic element which will be the parent of the newly created element. In case of non containment reference, the specifier should specify which element will be the parent of the newly created element.
- a search button : `semanticCandidatesExpression` much be set, provides a way to call a specific operation (selection wizard...). The operation must be provided by the Sirius specifier.
- an unset button : `unsetExpression` much be set, provides a way to call a specific operation (unset...). The operation must be provided by the Sirius specifier.

![Single reference widget]({{ site.baseurl }}specifications/images/complex-widgets/SingleReferenceViewer.png "Single reference widget")

#### Multiple references

##### User Stories

###### US-2 As a Sirius specifier, Zoe wants to provide a widget to represent multiple references

Zoe needs to represent multiple references in her properties view. She uses the multiple references complex widget. 

##### Proposed solutions

This widget is composed by:

- a label : a `labelExpression` much be set, provides the label value
- a list field : `valueExpression` much be set, provides the current selected values
- the list field contains an hyperlink : `onClickExpression` is optional, provides a way to call a specific operation (open a wizard, select the element in the explorer...). The operation must be provided by the Sirius specifier.
- a create button : `createExpression` is optional, if not set the button does not appear, provides a way to call a specific operation (creation wizard...). The operation must be provided by the Sirius specifier. In case of containment reference, the expression needs to reference the current semantic element which will be the parent of the newly created element. In case of non containment reference, the specifier should specify which element will be the parent of the newly created element.
- a search button : `semanticCandidatesExpression` much be set, provides a way to call a specific operation (selection wizard...). The operation must be provided by the Sirius specifier.
- an unset button : `unsetExpression` much be set, provides a way to call a specific operation (unset...). The operation must be provided by the Sirius specifier.
- an up button : `upExpression` much be set, provides a way to call a specific operation (up the selected element in the list...). The operation must be provided by the Sirius specifier.
- an down button : `downExpression` much be set, provides a way to call a specific operation (down the selected element in the list...). The operation must be provided by the Sirius specifier.

![Multiple references widget]({{ site.baseurl }}specifications/images/complex-widgets/MultipleReferencesViewer.png "Multiple references widget")

