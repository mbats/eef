---
layout: meetingminutes
title: 2015-07-24
description: First technical meeting
---
##### Goal

The goal of this meeting is to evaluate the current state of EEF and define how to move forward to a 2.0 release for Eclipse Neon.

For Sirius 4.0, we need a brand new awesome feature. From this requirement, we have found that Sirius' end users need awesome Properties view easily.
People should not have to wonder how to get good Properties view in Sirius, it should just work.

##### Participants

* Mélanie Bats
* Stéphane Bégaudeau
* Cédric Brun
* Goulwen Le Fur

##### Technical Objectives

* Properties view for Sirius compatible with CDO / Obeo Designer Team
* Implementation of dialogs, EObjects selectors, files selectors that can be reused
* UI with first class asynchronous support
* Less code is better, the code needs to be very simple and easy to maintain
* Dynamic UI that can be parameterized like in Sirius
* Modular API (core, edit, ui, toolkit specific properly separated)

##### Overview

EEF 2 does not use code generation anymore. The runtime bundles handles the binding ( with EClasses and EStructuralFeatures), Editors and Views (no SWT dependency). It uses only EMF, Guava, OSGi, Core Runtime. The current Target Platform uses Eclipse Helios.

In a view we can have container which can have containers which can have editors which can be ElementEditors or EObjectEditors. An ElementEditor has a representation which is a widget and it can be readOnly. EEF can not specify currently the layout (grid layout etc) of the widget. The container is a ViewElement so it can have a representation (widget) which could let the specifier define the layout.

There are one model for the view and one model for the binding, no model for the result at runtime (POJO).

The EEFService works on a type of Element. It does something for a type of element.

* Those services are registered in a provider with a priority
* Everything meaningful in EEF is registered with this mechanism
* Someone can register another instance of a service with a higher priority than an existing instance

Example, the ViewHandler

* It takes a View model and creates a SWT view
* It creates the view and it can be replaced for another dialect for the view.
* We could create an ODesignViewHandler which could take an ODesign and create a SWT view for this dialect


A context (PropertiesEditingContext) has all the information regarding the edition (adapter factory, change recorder, editing domain, etc). It has options for the validation for example etc.It knows the EObject edited and it will be given to the EEF viewer. It is destroyed if the EObject displayed is modified. There is one context for one EObject.

The PropertiesEditingComponent is retrieved using PropertiesEditingContext#getEditingComponent(). It is a binding between the EObject and the SWT widget.

The EEF viewer will take the context and have a look at the EObject to edit.

* It will ask the context to call the EEFBindingSettings registry (one binding setting per metamodel)
* EEFBindingSettings holds the description of the view to create
* We find an EEFBindingSettings to create for the EObject edited
* If none found, a reflective one is created (default behavior)

The EEF viewer is a JFace viewer, it takes the context as input (setInput). The EEF view gives the composite and does not do much work. We retrieve the binding EClassBinding ( which map an EClass with one or more views -> one or more tabs). The EEFViewer asks the PropertiesEditingComponent for the viewDescriptors. The EEFViewer asks the context for a view handler provider which will provide the view handler for the view descriptor. The view handler is used to create the view (for example PropertiesEditingView).

PropertiesEditingView can realize the validation and more. It can create the view with PropertiesEditingViewHandler#initView(component, view);

The PropertyBindingHandlerImpl knows how to modify the model, receive EEF events (created form SWT events), and receive EMF events too.

* Handles all the component of EEF, one instance only
* It creates the component but to interact with them, he receives the component again
* Receives all UI events from all EEF components and all events from all instance of all EMF models handled by EEF

The PropertyBindingHandlerImpl is a singleton which maintain all the tuples EObject / Component SWT (PropertiesEditingComponent).


##### Events from EMF to SWT

When a view is created on an EObject, a listener is registered on the EObject, Resource or ResourceSet

* This listener will fire event, the PropertyBindingHandlerImpl will listen to all those events
* PropertyBindingHandler will ask all the components if it is interested in this event
* The component can retrieve the view description from the view
* We can retrieve the view handler from the view description
* The view handler will update the SWT view from the event
* One view handler for EEF it knows how to create SWT view and update SWT view

SWT Event are transmitted by the PropertiesEditingView to the component which will transmit them to PropertiesBindingHandlerImpl.

* PropertiesEditingView -> EEF view which encapsulate the SWT View
* PropertiesEditingComponent has references to the PropertiesEditingView and the EObject
* The ViewHandler manipulates the PropertiesEditingView

The ViewHandler creates the view by calling the toolkit. If we have a text field and a checkbox, the toolkit can create the widget. The SWT listener are created by the toolkit. The listener can retrieve the PropertiesBindingHandlerImpl by using the toolkit. One toolkit for EEF (for each kind of toolkit SWT, SWT JDT, SWT EMF), one toolkit per theme.

##### Events from SWT to EMF

* If an SWT event is fired, it calls the PropertiesBindingHandlerImpl.
* The original event is mapped to a PropertyEditingEvent (example TypedPropertyChangedEvent)
* PropertyEditingEvent -> what the view expects to occur in the model
* The PropertiesEditingView know the Component (bidirectional relationship)
* See PropertiesBindingHandlerImpl#firePropertiesChanged(propertiesEditingComponent, propertiesEditingEvent)

For example, a PropertiesEditingEvent could be "SET" the "NameEditor" to "Toto" or "SET" the "AbstractEditor" to "true".

The PropertiesBindingHandlerImpl asks the component for the context. The PropertiesEditingContext is retrieved using the context factory. EEF creates a new child context. For example, one major context (we have open a wizard) then we have one child context if we have edited a field in the wizard. Thus we can undo the edition of a field or the whole edition in the wizard.

The PropertiesBindingHandlerImpl creates the editing policy for the context (PropertiesEditingPolicy). The PropertiesEditingPolicy validates the modification to do, the modification to realize and how to realize it. If the validation fails, an event should be sent (PropertiesValidationEditingEvent). The PropertiesBindingHandlerImpl executes the editing event. See PropertiesEditingPolicy#execute(PropertiesEditingContext).

The EditingPolicyRequestFactory creates requests from an event. The PropertiesEditingPolicy contains a request (what should be done) and a processor (how to do it). For example, a request (change the name to "Toto") and processor (if we have an editing domain create a command). The PropertiesEditingContext knows if we have an editing domain, transactional support etc... In the DomainEditingPolicyProcessor we can do our job using processByFeature or processByAccessor (if we have registered a Java service).

##### Existing EEF Services

* EditingPolicyProcessor:  For an event and for the context modify the EObject
* EditingPolicyRequestFactory: Create the request from the event
* EEFBindingSettings: Give the EEF configuration model for a given EObject
* EEFEditingService: Bring utility stuff for the edition (find the feature from the editor, get the value for an editor, an EObject and a context, get choice of value)
* EEFInvoker: Invoke the Java services
* EEFLockManager: Can lock a field (if a field is read only), used by the view handler
* EEFLockPolicyFactoy: EEFLockPolicy for example if EMF Edit says it can't be edited -> read only
* EEFNotifier: Can add marker to the view (errors, warnings)
* EEFToolkit: Factory of SWT widget
* EMFService: EMF utility stuff (equality of EClasses)
* ModelRequestor: Not implemented
* PropertiesBindingHandler: God class
* PropertiesEditingContextFactory: Create context for all use cases
* PropertiesEditingPolicyProvider: Create the editing policy
* ViewerFilterBuilder: Create filters
* ViewHandler: Create and handler the view
* ViewServices: SWT utility stuff (find the editing domain from the property view)

The services can be replaced thanks to contribution using OSGi Declarative services. When we create a new EEF model, it creates the declarative service to register the binding. The priorities are defined by a graph where each node defines its priority relative to other members of the graph.

How can we contribute an editor for a custom data type ? Create an EEF model, define a toolkit with widget for the datatype and create a class to indicate how to manipulate it.

There are two system of options (runtime and model definition). At runtime we can tell EEF do not validate the model or we can change the delay of the asynchronous processing
