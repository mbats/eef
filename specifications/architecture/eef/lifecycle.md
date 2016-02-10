---
layout: specification
description: EEF Core
---

## Lifecycle

The lifecycle of the Properties view can be separated in deferent parts

### Computation of the ITabDescriptors

The first step of the lifecycle of the TabbedProperties view is the retrieval of the ITabDescriptors. Those descriptors will be computed by EEF from a description of the view (EEFViewDescription) given to the EEF runtime. The tab descriptors will contain various section descripitors which will be used to create the various sections of a tab. Those descriptors can be computed using the current selection along with the current workbench part.

The computation of the ITabDescriptiors and the ISectionDescriptors will be realized thanks to the EEFViewFactory and the EEFTabDescriptor. For that, the EEFViewFactory will create a new EEFView and initialize it. This initialization will use the semanticCandidateExpression, the interpreters and the various variables available in order to compute the various pages and groups used. 

The business layer managing the widgets that are contained in the groups will not be instantiated during this step (see creation of the controls).

### Instantiation of the ISection

The TabbedProperties view framework will then use the ISectionDescriptors to instantiate the various ISections to use. Those sections will "directly" manipulate user interface concepts. Once instantiated, those ISections do not have access to the input yet but they are parameterized by the ISectionDescriptor of their section (which can have access to the EEF description).

### Creation of the controls

When the method createControls of the ISection is called, the various widgets of the user interface should be created. The listener used to bind those widgets to the controller should only be setup in the method aboutToBeShown. As a result, in a regular view, those widgets would be stored in the fields of the ISection while waiting for a call to the method aboutToBeShown. In our case, the widgets will be created from a description of widgets from EEF. For example, an EEFTextDescription will be used to create both an SWT Text and an SWT CLabel. Later both of those widgets will have to be linked to a controller. While we would use directly the ISection as the holder of the widgets (and the holder of this relationship between the widgets and the description responsible of their creation), it would be best to separate this responsibility in another class since it will involve a lot of code (all the code of the initialization of all the widgets).

The classes XXXBinding (i.e. EEFTextBinding) will be responsible of the synchronization of the widgets and the controller with the lifecycle of the Properties view. As a result, those classes will create the controls for a specific kind of description.

### Retrieval of the input

Once the widgets have been created, the input is provided to the ISection using the method setInput. The input should be updated once for the whole view, not for each individual widgets.

### About to be shown

This operation will be called before the appearance of the user interface in order to let us connect our listeners to our widgets.

### Refresh

### About to be hidden

This operation will be called before the disappearance of the user interface in order to let us disconnect our listeners from our widgets.

### Dispose

This operation is called when the whole Properties view is disposed because another description of the Properties view is supposed to be shown.

