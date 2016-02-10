---
layout: meetingminutes
title: 2015-07-21
description: Preparation of the first technical meeting
---
##### Goal

The goal of this meeting is to realize an analysis of EEF in order to come up with some questions for the first technical meeting. This document has been updated with the answers from Goulwen.

##### Participants

* Mélanie Bats
* Stéphane Bégaudeau

##### General

Questions

1. An instance of Gerrit is necessary
2. The election process for Mélanie and Stéphane should be launched in order to have access to some tools on the project.
3. A valid target platform is needed. There are two target platforms in EEF and both of them are not complete enough to make the whole code compile.
4. Is EEF only for Properties view?
5. Does EEF work with only and EObject and a SWT Composite?
6. Does it require EObject to be in a Resource? In a ResourceSet?

Answers

1. Yes
2. Yes
3.
4.
5.
6.

##### Dependencies

Questions

1. Does EEF uses Guava?
2. Which version?
3. Is Guava visible in the API?
4. The runtime bundle has a dependency to org.eclipse.core.runtime for the log
5. Dependency to the ResourceSet in EditingModelEnvironment.getResourceSet()
6. Dependency to the EditingDomain in DomainAwarePropertiesEditingContext.getEditingDomain()
7. Why does ViewChangeNotifier extends java.beans.PropertyChangeListener? Java Beans?
8. Dependency to OSGi in the API in EEFToolkitImpl.activate(ComponentContext)

Answers

1. Yes
2. 10.1
3. No
4. The runtime bundle has a dependency to org.eclipse.core.runtime for the log
5. Dependency to resource set -> bad pattern
6. Dependency to the Editing Domain -> to create commands
7. Unit tests
8. To read the properties of the OSGi Services

##### OSGi & Declarative Services

Questions

1. OSGi Declarative services, why?
2. Why not use a couple of services and not use the XML-based dependency injection of OSGi? Our experience on some other project has highlighted various major maintainability issues.
3. Why not use a modern dependency injection framework based on annotations instead of XML like Google Guice, HK2, etc?

Answers

1. Easier to use. Can be done manually
2. To redefine the behavior. Architecture similar to GMF more flexible with GMF with OSGi.
3. Does not know the technologies named.

##### Meta-model

Questions

1. What is the role of the query meta-model?Why don't you use AQL and the Sirius interpretors? Why another mechanism?
2. Why does the name of Java appear in the meta-model? (JavaEditor, JavaView, JavaBody)
3. Why does ViewRepository have a RepositoryKind attribute? Why does this attribute start with an upper-case?
4. Why can we add views in a ViewsRepository and in a Category? It is confusing for new users.
5. What is the meaning of the field "explicit" in a View? Parental advisory explicit content?
6. Why does a view can contain another view?
7. Why is there at least four levels of recursions? Category, View, Container and ElementEditor?
8. Why not use a structure similar to Sirius? Viewpoint, Representation, Layer, Mapping, Style? With almost no recursions (Container mapping)
9. The field "representation" should not have this name. We should not use a name used in Sirius for a different concept. We should use the same naming conventions
10. What is a ViewElement compared to Sirius? A Binding, mapping, style?
11. The field nameAsLabel should not exist. It should use a string using a semantic expression (a regular String, service: or aql:).
12. We should reuse existing interpretors including those from Sirius.
13. Why does the concept ViewReference is in the meta-model? Isn't this a simple widget, like a hyperlink, which will change the view once used?
14. Why does the concept of CustomView exist?

Answers

1. Used to define queries to navigate in the properties view. When EEF2 was created, AQL did not exist.
2. For the Java POJO using Java beans
3. Legacy
4. Legacy
5. Legacy
6. Legacy
7. Category should be removed, View is the entry point. Container -> layout, ElementEditor can have children for columns in table. Metamodel not reified (no table editor with column editors)
8. Not a question
9. Yes
10. Something that can be displayed
11. Should be done
12. Yes
13. Legacy
14. Legacy used for the code generation previously

##### Technical

Questions

1. Why overriding java.lang.Object#clone()? See EEFServiceProviderImpl.clone()
2. Can EEF work with Java services in the workspace?
3. Why does it need to load classes? org.eclipse.emf.eef.runtime.binding.settings.EEFBindingSettings.loadClass(String)
4. Multiple ways of doing the same thing should be avoided. See PropertiesEditingContextFactory.createPropertiesEditingContext(). Seven methods to do the same thing is too much. If this factory can be configured, we should use the builder pattern.
5. What does the concept of autowire mean? See ContextOptions.AUTOWIRING_OPTION. Is it similar to autowire in other dependency injection framework?
6. Why does PropertiesEditingComponent have a method getViewDescriptors() which returns a List&lt;View&gt; and a method getViews() which returns a List&lt;Object&gt;?
7. bindingHandlerProvider.getBindingHandler(component.getEObject()).firePropertiesChanged(component, new PropertiesEditingEventImpl(evt.getSource(), evt.getPropertyName(), ((evt instanceof TypedPropertyChangedEvent)?((TypedPropertyChangedEvent)evt).getEventType():PropertiesEditingEvent.SET), evt.getOldValue(), evt.getNewValue()));
8. Why are there multiple notification mechanisms? PropertiesEditingListener vs PropertyChangeListener vs EEFNotification?
9. Why not use EMF notifications everywhere? Or one framework?
10. Why is there a separation for synchronous and asynchronous use cases? See PropertiesEditingEvent.delayedChanges()? Why not consider a synchronous use case as a simple asynchronous use case?
11. Why use a whole ResourceSet and not just the toolkit model in EEFToolkitImpl.getAllWidgetsFor(ResourceSet, EStructuralFeature)?

Answers

1. For the graph of services
2. No
3. For Java services (accessors)
4. Yes
5. If the name of the fields is the name of the fields of the EClass
6. getViewDescriptors() -> EEF view & getViews() -> SWT view
7. :)
8. PropertiesEditingListener (UI events) PropertyChangeListener (Java bean) EEFNotification (markers)
9. Maybe (maybe sometimes we don't need a notification Object, we could use methods directly sometimes)
10. Problems with asynchronous and transactions. EMF Transactions and thread
11. Because the toolkit needs to find itself in the ResourceSet.
