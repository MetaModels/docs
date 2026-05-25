.. _component_dca_visibility-conditions:

|img_dca_condition| Visibility Conditions / Sub-Palettes
==========================================================

Visibility conditions are also referred to as "sub-palettes", as they allow an input widget of an
attribute in an input form to be specifically shown or hidden based on the values of another widget.

An example to illustrate this: In an input form there is a checkbox "Billing address" and you want
three additional input fields (street, postal code, city) to appear in the input form when it is
checked — and only when the checkbox is checked.

In this case, the three fields (street, postal code, city) react to the value of the "Billing address"
checkbox (value = 1) and are shown when the checkbox is set, and only then is the data also saved.

The idea of showing/hiding fields via a legend ("green separator line") falls short — this only sets
the current visibility in the form of an accordion and does not affect saving. Moreover, with
visibility conditions/sub-palettes it is also possible to set up complex rules about under what
condition an input widget should be visible or not.

Note that visibility conditions are not created like an accordion with two enclosing "wrapper elements"
that enclose multiple input widgets — instead, the conditions must be set separately for each widget.
Only in this way is it possible to set up very complex and mutually dependent display rules.

If you have created a complex rule for an input widget and want to easily assign it to additional
input widgets, you can use the property type "Property is visible..." (see below).

To create a visibility condition, click the icon |img_dca_condition_1| "Visibility conditions for
input field ID n" in the attribute list of an input form. In the overview of visibility conditions
that opens, a new condition is added by clicking the |img_new| "New" button and inserting it via
the clipboard.

In the input form that opens, the condition type must first be selected in the base configuration.
Two groups of condition types are available:

* Conditions referring to an attribute or an input widget
* Conditions as logical operators (AND/OR/NOT)

As a small memory aid, the types and their usage are stored in the |img_help| help assistant.

For widgets that have a visibility condition implemented, the icon is color-highlighted |img_dca_condition|.

The following types of conditions are implemented:

* **Property value is...** |br|
  The condition is met when the attribute value equals the specified value.
  Attributes with single selection, such as Select or Checkbox, can be selected. \*
* **Property value can contain...** |br|
  The condition is met when any attribute value equals any of the specified values
  (intersection/OR). Attributes with multiple selection, such as Tags, can be selected.
* **Property is visible...** |br|
  The condition is met when all conditions for a selected attribute are met. In other words,
  the attribute is visible if and only if the selected (or "referenced") attribute is also
  visible. This condition type saves you from duplicating created visibility conditions of
  an attribute.
* **OR** |br|
  Any one condition must be met.
* **AND** |br|
  All conditions must be met.
* **NOT** |br|
  Reverses the result of a given condition.

.. note:: \* From version 2.3, the empty or unfilled condition of a Select or Checkbox widget can
   also be used without further configuration — :ref:`before MM 2.2, an additional "NOT" was required
   <rst_cookbook_inputmask_checkbox-negation>`. Note that for a checkbox, the value "-" and "Inactive"
   are equivalent, so the select jumps back to the first value "-" after saving.


.. |img_dca_condition| image:: /_img/icons/dca_condition.png
.. |img_dca_condition_1| image:: /_img/icons/dca_condition_1.png
.. |img_new| image:: /_img/icons/new.gif
.. |img_about| image:: /_img/icons/about.png
.. |img_help| image:: /_img/icons/help.svg

.. |br| raw:: html

   <br />
