---
layout: specification
description: Roadmap
---


##### Neon M4 (2015-12-11)

* _Availability_: 4 weeks or 30 days
* _Estimation_: 30 days

The goal of this milestone is to build an industrialized prototype.
At the end of this milestone, we provide an update site for EEF and an update site for a specific Sirius version. 
With this milestone a Sirius specifier is able to specify in a odesign file a new properties view description. He is able to create in the VSM properties views, tabs, sections and Text widgets. Following the Sirius philosophy, the properties view description is interpreted at runtime by Sirius and is based on binding description. Same as in classical Sirius representation, the mapping are based on semantic candidates defined with domain class and interpreted expressions.
When the end user selects a diagram element in a Sirius based modeler, the properties view shows the selected element properties. Only text widget elements are available.

###### Metamodels (DSLs)

- ![DONE](images/status-done.svg) Remove unnecessary concepts from `eef.ecore` (Est: **2 days**)
  - fresh start with a new `eef.ecore` and rename the old one (we won't have the generated code of the "old" one)
  - only keep the concepts that can be used from `properties.ecore`, and only the ones needed for the current milestone.
- ![DONE](images/status-done.svg) Sirius: add support for unknown/optional extensions in the VSM (in particular, under _Group_)
  - use EMF's [native mechanism](http://ed-merks.blogspot.fr/2008/01/creating-children-you-didnt-know.html)
  - take this opportunity to remove Sirius's legacy way of doing this and replace it with EMF's mechanism

###### Runtime

- ![DONE](images/status-done.svg) Extract a basic query API in new `org.eclipse.sirius.common.query`
  - the new API should not depend on the rest of Sirius and can be used by EEF.
- ![DONE](images/status-done.svg) Add support for different semantic candidates (pages / groups) (Est: **7d**)
  - change the "self" used for a Page, a Group, etc...
    - see `org.eclipse.eef.core.internal.EEFPageImpl.createControl()`
  - accessible variables for each expression? how to manage them?
     - `IVariableManager` in a Sirius bundle named `org.eclipse.sirius.common.query`
  - **depends on work on the Sirius side** (see below)
- ![WIP](images/status-wip.svg) [Improve the interpreter API](https://bugs.eclipse.org/bugs/show_bug.cgi?id=482993)
  - ![DONE](images/status-done.svg) improved variable scoping and lifecycle (remove setVariables / unsetVariables)
  - completion proposals (EMF 2.8's styled strings for the display and information)
  - camel case support and filters for the interpreters and proposals

###### Widgets

- ![DONE](images/status-done.svg) Fully implement a simple _Text Field_ widget
  - lifecycle (edit initial operation / valueExpression) (Est: **1d**)
    - see `org.eclipse.eef.ide.ui.internal.data.EEFSection.aboutToBeShown().new IConsumer() {...}.apply(String)`
  - refactor the controller and instantiation of the widgets (Est: **2d**)
    - `org.eclipse.eef.ide.ui.internal.data.EEFSection.createControls(Composite, TabbedPropertySheetPage)`

###### Default behavior and customization

- ![DONE](images/status-done.svg) Sepcify the default behavior of `properties.ecore`
  - [definition of the default behavior](default-behavior.html) expected (each structural should have a default widget in the order of the structural features) (Est: **1d**)
- ![DONE](images/status-done.svg) [Ensure seamless integration with tabs provided by other tools](https://git.eclipse.org/r/#/c/63098/) (GMF, Advanced, Semantic) (Est: **2d**)
  - may **depend on work on the Sirius side**
  

###### Lifecycle

We need to have a clear vision of the way the lifecycle of our application works. At the very least we should document the rules and constraints imposed by the Eclipse properties view framework. If possible we should hide some/most of the complexity under a simpler façade which isolates the rest of our work from the complexities and/or enforce the constraints.

- ![DONE](images/status-done.svg) [Fork the tabbed-property views framework](https://git.eclipse.org/c/eef/org.eclipse.eef.git/tree/plugins/org.eclipse.eef.properties.ui)
  - The existing framework has too many limitations, especially in terms of dynamic behavior.
- ![DONE](images/status-done.svg) [Understand and document the lifecycle of properties view](images/lifecycle.png) **1d**
- ![WIP](images/status-wip.svg) Lifecycle of the interaction with Eclipse Sirius **2d**
- ![TODO](images/status-todo.svg) Take into account the validation **1d**
- ![TODO](images/status-todo.svg) Take "read only/locked" status into account **1d**
- ![TODO](images/status-todo.svg) Evaluate the performances and capabilities of SWT **2d**
  - ability to create and destroy widgets (hide them?)
  - scaling (how many widgets? are pages not displayed refreshed?)
  - we need this info to have an idea of how much dynamicity in the contents of the property views we can expect to support

###### EMF Edit Support in Sirius

The goal is to make it easy/natural to benefit from manual customizations done on the EMF.Edit generated code (e.g. item providers) from Sirius-based modelers wherever it makes sense. This includes, but is not limited to, the property views.

- ![DONE](images/status-done.svg) [Specification](http://sbegaudeau.github.io/eef/specifications/emf-integration.html) with the Sirius team **2d**
  - ![DONE](images/status-done.svg) Identify the parts of EMF.Edit that make sense to expose more directly from the VSM
    - for example when configuring a _labelExpression_, it should be easy to say "use the label provider I've already customized for my metamodel"
  - ![DONE](images/status-done.svg) Prioritize which of these mechanisms will be needed earlier than others to be exploited by EEF
  - ![DONE](images/status-done.svg) Launch [the corresponding work on the Sirius team](see https://bugs.eclipse.org/bugs/show_bug.cgi?id=482831).

###### Release engineering and project management

- ![DONE](images/status-done.svg) Technical documentation **2d**
- ![DONE](images/status-done.svg) Unit tests, [continuous integration](https://hudson.eclipse.org/eef/job/eef-incubation-master/) and [integration with Gerrit](https://hudson.eclipse.org/eef/job/master-gerrit/) **2d**
- ![DONE](images/status-done.svg) Contribute to the [EEF update site](http://download.eclipse.org/modeling/emft/eef/updates/nightly/latest/neon) for Eclipse Sirius

##### Neon M5 (2016-01-29)

* _Availability_: 7 weeks or 50 days
* _Estimation_: 46 days

The goal of this milestone is to support basic widgets and dynamic mappings.
At the end of this milestone, we provide some more basic widgets : Checkbox, Combo, Label, Radio, TextArea, Button.
It is also possible for the specifier to define dynamic mappings to specify with one mapping description many widgets. For example, it will be possible to define that 'EString' attributes defined in the meta-model will be represented with a Text widget.
At the end of this milestone, when the specifier does not specify anything for properties view in the VSM, at runtime a default properties view will be provided.
The specifier can also define that a properties view is visible only when a Sirius element exists : Viewpoint, Representation, Layer, Filter, Mapping. For example, this means that when the end user selects an element in a diagram if the 'Documentation' layer is activated, he will see a new documentation tab with a text area. If this layer is unactivated, this tab will not be visible anymore.

###### Widgets

- ![DONE](images/status-done.svg) checkbox **2d**
- ![DONE](images/status-done.svg) combo **3d**
- ![DONE](images/status-done.svg) label **2d**
- ![WIP](images/status-wip.svg) radio **2d**
- textArea **1d**
- button (test with dialogs -> browse button with fileselector /!\ lifecycle) **3d**
- ![DONE](images/status-done.svg) dynamic mappings **3d**
  - for iterator in collectionExpression (domainClass.eAllStructuralFeatures) -> switch switchExpression -> case valueExpression
- ![WIP](images/status-wip.svg) implementation of the default behavior
  - definition of the default rules in a default.properties **1d**
- implementation of a prototype of the custom widget (color picker) **1d**
  - we will use this example to evaluate the API needed to support custom widgets
- implementation of the custom widget to manage mono/multi valued references **16d**

- ![WIP](images/status-wip.svg) context **10d**
  - viewpoint, representation, layer, filter, mapping
  - model explorer ~> lack of context with the Eclipse Sirius bridge

- ![WIP](images/status-wip.svg) tab order **2d**

###### EMF Edit (see https://bugs.eclipse.org/bugs/show_bug.cgi?id=482831)

Feedback and improvement with the Sirius team **2d**

###### Specification of the styling **10d**

- Font
- Colors (background, foreground)
- Label (bold, italic, underline, strikethrough etc)
- Conditional style

###### Release engineering and project management **4d**

- technical documentation
- more tests


##### M6 26/03/16 (API freeze) availability 8 weeks or 65 days (planning 57 days)

The goal of this milestone is to support EMF edit by default and get a first version of validation and styling.
By default, EEF uses EMF Edit : label provider, choice of values, diagnostician, validation.
The specifier can provide custom complex widgets to his properties view description thanks to a dedicated API.
For the end user, when a validation error is thrown, the associated element in the properties view is highlighted (marker, color change).
It is possible for the specifier to provide styles for widgets. For example for a Text widget, he will be able to change the font and the foreground/background color. As in Sirius, he can also specify conditional style on widget description based on a predicate.

###### Feedback

Fix and improvements **10d**

###### Widget

- provisional API to extend properties.ecore with custom widgets (conversion in eef.ecore)
  - how to define a custom widget in properties.ecore **2d**
  - how to make this definition evolve with the future changes in properties.ecore (regenerate the code, etc) **1d**
  - provisional java API **2d**
  - update of the color picker prototype **2d**

###### EMF Edit (see https://bugs.eclipse.org/bugs/show_bug.cgi?id=482831)

Integration of the EMF Edit support created by the Sirius team **5d**

- label provider
- choice of value
- diagnostician (message, feature, context, etc)
- EDataType validation (see [this](http://eclipsesource.com/blogs/2014/08/26/emf-validation-for-datatype-constraints/))
- eSet / eGet
- evaluation of the usage I18N form Sirius in eef (fallback to EMF.edit by default)
- update of the default behavior to use EMF Edit

###### Validation and messages

Development **10d**

- Message and kind on widgets, groups, view ([message manager](https://www.eclipse.org/eclipse/platform-ua/proposals/forms/enhancements-3.3/))
- Markers and Problems view

###### Styling

Implementation of the styling **20d**

###### Release engineering and project management **5d**

- technical documentation
- more tests

##### M7 06/05/16 availability 6 weeks or 50 days (planning 52 days)

The goal of this milestone is to fix performance/scale issue. We provide also layout capabilities and some basic tooling to help the specifier to define his properties view description.

###### Feedback

Fix and improvements **20d**

###### Tooling **15d**

- advanced features in properties.ecore
  - reuse / import
  - new tooling

###### Performances **2d**

- performance and scaling (fix everything!)

###### Layout **10d**

- grid layout with containers?

###### Release engineering and project management **5d**

- technical documentation
- user documentation
- more tests


##### RC1 20/05/16

##### Eclipse Neon 1

- quickfixes
- content assist

###### Interpreters

- usage of expression.ecore **15d**

###### Behavior
- specification of the behavior expected to override the default behavior **2d**
  - replacement
  - position
  - order
  - should we generate anything in the odesign of the specifier?
  - create some example
  - implementation of the overriding **10d**

##### Eclipse Neon 2

###### Dialogs and wizards **10d**

We need to evaluate how we can create dialogs & wizards

- undo / cancel (best practices)
- UI thread / jobs interactions


###### Dialogs and wizards **20d**

Implementation of the dialogs et wizards

##### Eclipse Omega

- Tree **15d**
- Table **20d**
  - actions: appearance and behavior (add, remove, up, down)

- tooltip
- actions on the view
  - show advanced
  - restore default value
  - see related elements

- lock
  - extract, refine and reuse the IPermissionAuthority from Eclipse Sirius

- support for multi selection **15d**
  - evaluate the need for this feature and its behavior (what should we display)
  - 1st prototype and final implementation in M6

- creation of a complex widget (link eref viewer) **10d**
  - find its new name :)
  - definition in properties.ecore
  - implementation


