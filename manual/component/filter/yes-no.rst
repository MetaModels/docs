.. _component_filter_yes-no:

|img_filter_checkbox| Yes / No
================================

The "Yes / No" filter rule (package ``filter_checkbox``) outputs a frontend widget
through which visitors can choose between two states: "Yes" (value ``1``) or "No"
(value ``0`` or empty). Depending on the mode, the widget appears as a checkbox, as
two separate checkboxes (yes/no), or as radio buttons.

The filter rule filters items based on the selected value of a checkbox attribute and
is suitable for explicit yes/no selections in the frontend filter widget.

.. note:: In the backend selection menu for the filter rule type, this rule is displayed
   as "Yes / No". It is technically identical to the
   :ref:`Checkbox Status filter rule <component_filter_checkbox>`, but is configured
   with the "Radio buttons" or "Yes/No checkbox" mode.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_checkbox


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Yes / No".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The checkbox attribute by whose value items should be filtered.


Settings for the Frontend Widget
--------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - URL parameter
     - The key of the URL parameter for passing the filter value. Without input,
       the column name of the attribute is used. With ``auto_item``, only the value —
       without key — is embedded in the URL.
   * - URL type for the parameter
     - Defines whether the parameter is passed as a slug (friendly URL) or as a
       GET parameter (from MM 2.4) — :ref:`see SEO <rst_cookbook_tips_seo_filter-url>`
   * - Static parameter
     - If this option is active, the filter value is obtained from a selection list in
       the content element/module instead of from the URL.
   * - Label
     - Label of the filter widget.
   * - Hide label at filter widget
     - Suppresses the output of the label.
   * - Template
     - Template for the widget output. Default: ``mm_filteritem_default``;
       for checkbox-specific output: ``mm_filteritem_checkbox``.
   * - Mode
     - Selects the display mode of the widget:

       * **Yes checkbox** — Single checkbox; filters for ``1`` when checked
       * **No checkbox** — Single checkbox; filters for ``0``/empty when checked
       * **Radio buttons** — Two radio buttons (yes/no); additionally the option
         "Allow empty selection" for an "All" option appears
   * - "Yes/No" instead of attribute name
     - Displays "Yes" and "No" as option labels instead of the attribute name.
   * - Allow empty selection
     - (only in "Radio buttons" mode) Adds an empty option ("All") that can be used
       to deactivate the filter rule.
   * - Option label as parameter
     - The URL parameter value is the option text ("Yes"/"No") instead of a number.
   * - CSS ID/class
     - Sets a CSS ID or class on the widget element.


Matching Attributes
------------------

The "Yes / No" filter rule is exclusively suitable for the following attributes:

* :ref:`Checkbox <component_attribute_checkbox>`
* :ref:`Translated Checkbox <component_attribute_translatedcheckbox>`


.. |img_filter_checkbox| image:: /_img/icons/filter_checkbox.png

.. |br| raw:: html

   <br />
