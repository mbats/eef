---
layout: specification
description: Styles
---
We need to be able to provide style customization to the end users.

##### User stories

###### US-1 As a Sirius specifier, Zoe wants to define the font used by a text widget

Zoe needs to specify the font of a text widget. She specifies a new *EEFTextStyle* contained by the *EEFText* reference *style*. In the EEF meta-model, all the widgets have there own corresponding styles defined. These styles define customizable attributes depending on what the widgets allow. For all the widgets, we can customize the font thanks to the *fontExpression* available on *EEFStyle*. This expression returns an URI of a font.
For a text widget we are also able to customize the font color with the *foregroundColorExpression* attribute and the background color of the text field with the *backgroundColorExpression*. The colors are defined as rgb (rgb(0,0,0)) or hexa (#000000).

New concepts:

* EEFStyle
* EEFStyle#fontExpression: Expression [0..1]
* EEFTextStyle
* EEFText#style: EEFTextStyle [0..1] containment

###### US-2 As a Sirius specifier, Zoe wants to define the style of a text widget

Zoe needs to customize the font color and background color of a text widget. She customizes the font color with the *foregroundColorExpression* attribute and the background color of the text field with the *backgroundColorExpression* of the *EEFTextStyle*. If no specific style is defined explicitly for a widget, then the EEF runtime provides a default style.

New concepts:

* EEFTextStyle#foregroundColorExpression: Expression [0..1]
* EEFTextStyle#backgroundColorExpression: Expression [0..1]

###### US-3 As a Sirius specifier, Zoe wants to define a conditional style of a text widget

Zoe needs to set the font color and background color of a text widget to red when the validation fail. She defines in the reference *conditionalStyles* a new *EEFTextConditionalStyle*. The conditional style references a new *EEFTextStyle* with the *style* reference. Then she customizes the font and the background colors. Finally, she provides on the conditional style a *conditionalExpression* which returns true when the validation fails for the text widget. When the conditional style returns false, the default style defined by the *style* reference is used. If multiple conditional styles are defined the first one in the order of definition which returns true will be applied.

New concepts:

* EEFText#conditionalStyles: EEFTextConditionalStyle [0..-1] containment
* EEFTextConditionalStyle
* EEFConditionalStyle#conditionalExpression: Expression [0..1]
* EEFTextConditionalStyle#style: EEFTextStyle [0..1] containment

###### US-4 As a Sirius specifier, Zoe wants to define the style of a checkbox widget

Zoe needs to customize the font color and background color of a checkbox widget. She specifies on an *EEFCheckbox* widget a new *EEFCheckboxStyle* contained by the *style* reference. She customizes the font color of the widget with the *foregroundColorExpression* attribute and the background color with the *backgroundColorExpression* of the *EEFCheckboxStyle*. Zoe can define also *EEFCheckboxConditionalStyle* with the *conditionalStyles* reference. These conditional styles can reference *EEFCheckboxStyle* with the *style* reference.

New concepts:

* EEFCheckboxStyle
* EEFCheckbox#style: EEFCheckboxStyle [0..1] containment
* EEFCheckboxStyle#foregroundColorExpression: Expression [0..1]
* EEFCheckboxStyle#backgroundColorExpression: Expression [0..1]
* EEFCheckbox#conditionalStyles: EEFCheckboxConditionalStyle [0..-1] containment
* EEFCheckboxConditionalStyle
* EEFCheckboxConditionalStyle#style: EEFCheckboxStyle [0..1] containment


###### US-5 As a Sirius specifier, Zoe wants to define the style of a label widget

Zoe needs to customize the font color and background color of a label widget. She specifies on an *EEFLabel* widget a new *EEFLabelStyle* contained by the *style* reference. She customizes the font color of the widget with the *foregroundColorExpression* attribute and the background color with the *backgroundColorExpression* of the *EEFLabelStyle*. Zoe can define also *EEFLabelConditionalStyle* with the *conditionalStyles* reference. These conditional styles can reference *EEFLabelStyle* with the *style* reference.

New concepts:

* EEFLabelStyle
* EEFLabel#style: EEFLabelStyle [0..1] containment
* EEFLabelStyle#foregroundColorExpression: Expression [0..1]
* EEFLabelStyle#backgroundColorExpression: Expression [0..1]
* EEFLabel#conditionalStyles: EEFLabelConditionalStyle [0..-1] containment
* EEFLabelConditionalStyle
* EEFLabelConditionalStyle#style: EEFLabelStyle [0..1] containment

###### US-6 As a Sirius specifier, Zoe wants to define the style of a radio widget

Zoe needs to customize the font color and background color of a radio widget. She specifies on an *EEFRadio* widget a new *EEFRadioStyle* contained by the *style* reference. She customizes the font color of the widget with the *foregroundColorExpression* attribute and the background color with the *backgroundColorExpression* of the *EEFRadioStyle*. Zoe can define also *EEFRadioConditionalStyle* with the *conditionalStyles* reference. These conditional styles can reference *EEFRadioStyle* with the *style* reference.

New concepts:

* EEFRadioStyle
* EEFRadio#style: EEFRadioStyle [0..1] containment
* EEFRadioStyle#foregroundColorExpression: Expression [0..1]
* EEFRadioStyle#backgroundColorExpression: Expression [0..1]
* EEFRadio#conditionalStyles: EEFRadioConditionalStyle [0..-1] containment
* EEFRadioConditionalStyle
* EEFRadioConditionalStyle#style: EEFRadioStyle [0..1] containment

###### US-7 As a Sirius specifier, Zoe wants to define the style of a link widget

Zoe needs to customize the image, the font color and background color of a link widget. She specifies on an *EEFLink* widget a new *EEFLinkStyle* contained by the *style* reference. She customizes the font color of the widget with the *foregroundColorExpression* attribute and the background color with the *backgroundColorExpression* of the *EEFLinkStyle*. She can specify also an image thanks to the *backgroundImageExpression*. The image is defined as an URI. If the background image is not define then only text is represented else if the value expression of a link is not set and there is a background image defined then the EEF runtime shows only an image, else it shows the text and the image. Zoe can define also *EEFLinkConditionalStyle* with the *conditionalStyles* reference. These conditional styles can reference *EEFLinkStyle* with the *style* reference.

New concepts:

* EEFLinkStyle
* EEFLink#style: EEFLinkStyle [0..1] containment
* EEFLinkStyle#foregroundColorExpression: Expression [0..1]
* EEFLinkStyle#backgroundColorExpression: Expression [0..1]
* EEFLinkStyle#backgroundImageExpression: Expression [0..1]
* EEFLink#conditionalStyles: EEFLinkConditionalStyle [0..-1] containment
* EEFLinkConditionalStyle
* EEFLinkConditionalStyle#style: EEFLinkStyle [0..1] containment

###### US-8 As a Sirius specifier, Zoe wants to define the style of a combo widget

Zoe needs to customize the font color and background color of a combo widget. She specifies on an *EEFSelect* widget a new *EEFSelectStyle* contained by the *style* reference. She customizes the font color of the widget with the *foregroundColorExpression* attribute and the background color with the *backgroundColorExpression* of the *EEFSelectStyle*.  Zoe can define also *EEFSelectConditionalStyle* with the *conditionalStyles* reference. These conditional styles can reference *EEFSelectStyle* with the *style* reference.

New concepts:

* EEFSelectStyle
* EEFSelect#style: EEFSelectStyle [0..1] containment
* EEFSelectStyle#foregroundColorExpression: Expression [0..1]
* EEFSelectStyle#backgroundColorExpression: Expression [0..1]
* EEFSelect#conditionalStyles: EEFSelectConditionalStyle [0..-1] containment
* EEFSelectConditionalStyle
* EEFSelectConditionalStyle#style: EEFSelectStyle [0..1] containment

###### US-9 As a Sirius specifier, Zoe wants to define the style of a tree widget

Zoe needs to customize the font of a tree widget. She specifies on an *EEFTree* widget a new *EEFTreeStyle* contained by the *style* reference. She customizes the font of the widget with the *fontExpression* attribute. Zoe can define also *EEFTreeConditionalStyle* with the *conditionalStyles* reference. These conditional styles can reference *EEFTreeStyle* with the *style* reference.

New concepts:

* EEFTreeStyle
* EEFTree#style: EEFTreeStyle [0..1] containment
* EEFTree#conditionalStyles: EEFTreeConditionalStyle [0..-1] containment
* EEFTreeConditionalStyle
* EEFTreeConditionalStyle#style: EEFTreeStyle [0..1] containment

###### US-10 As a Sirius specifier, Zoe wants to define the style of a table widget

Zoe needs to customize the font of a table widget. She specifies on an *EEFTable* widget a new *EEFTableStyle* contained by the *style* reference. She customizes the font of the widget with the *fontExpression* attribute. Zoe can define also *EEFTableConditionalStyle* with the *conditionalStyles* reference. These conditional styles can reference *EEFTableStyle* with the *style* reference.

New concepts:

* EEFTreeStyle
* EEFTree#style: EEFTreeStyle [0..1] containment
* EEFTree#conditionalStyles: EEFTreeConditionalStyle [0..-1] containment
* EEFTreeConditionalStyle
* EEFTreeConditionalStyle#style: EEFTreeStyle [0..1] containment

###### US-11 As a Sirius specifier, Zoe wants to define the font color of a group label

Zoe needs to customize the font color of a group widget. She specifies on an *EEFGroup* a new *EEFGroupStyle* contained by the *style* reference. She customizes the font color of the group label with the *foregroundColorExpression* attribute. This feature customizes only the group label and not all the widgets contained in the group. Zoe can define also *EEFGroupConditionalStyle* with the *conditionalStyles* reference. These conditional styles can reference *EEFGroupStyle* with the *style* reference.

New concepts:

* EEFGroupStyle
* EEFGroup#style: EEFGroupStyle [0..1] containment
* EEFGroupStyle#foregroundColorExpression: Expression [0..1]
* EEFGroup#conditionalStyles: EEFGroupConditionalStyle [0..-1] containment
* EEFGroupConditionalStyle
* EEFGroupConditionalStyle#style: EEFGroupStyle [0..1] containment

###### US-12 As a Sirius specifier, Zoe wants to customize the font for all the text widgets of her properties view

Zoe needs to use the same specific font for all the text widgets defined in her properties view. She defines a new *EEFTextStyleCustomization* under the *View* *styleCustomizations* reference. Then she provides a new *EEFTextStyle* thanks to the *style* reference of an *EEFStyleCustomization* and she customizes the font style. The EEF runtime checks first if there is style customizations for a widget. If a conditional style or a default style is defined it overrides the style customization.

New concepts:

* EEFView#styleCustomizations: EEFStyleCustomization [0..-1] containment
* EEFStyleCustomization
* EEFTextStyleCustomization
* EEFTextStyleCustomization#style: EEFTextStyle [0..1] containment

###### US-13 As a Sirius specifier, Zoe wants to define the style of the label of a widget

Zoe needs to customize the label of a widget. She set the *labelStyle* widget attribute with a new *LabelStyle*. If the label style is not set, the EEF runtime uses the default style.

New concepts:

* EEFWidget
* EEFWidget#labelStyle: EEFStyle [0..1] containment

##### Functional Specification

The properties view is rendered in the Eclipse properties view, consequently we are restricted on styles by what is possible to do with SWT.

##### TODO
On label/text widget support underline/strikedout?
