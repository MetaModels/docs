.. _component_filter_checkbox:

|svg_filt_checkbox_22| |img_filter_checkbox| Checkbox Status
============================================================

The "Checkbox Status" filter rule (package ``filter_checkbox``) checks whether the value
of a checkbox attribute is equal to ``1`` (active). It is typically used for publication
control: only items with an activated checkbox (e.g. ``published = 1``) are displayed
in the frontend.

This filter rule usually operates without a frontend widget output, but can also be
configured so that visitors can select the checkbox status themselves via a frontend
widget.

.. seealso:: For multilingual MetaModels, the filter rule
   :ref:`component_filter_translated-checkbox` is available.


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
     - Selection of the filter rule type — here: "Checkbox Status" (or "Yes / No"
       in the backend selection menu).
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The checkbox attribute whose value should be checked.


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
   * - Label
     - Label of the filter widget in the frontend.
   * - Hide label at filter widget
     - Suppresses the output of the label in the frontend.
   * - Template
     - Template for the output of the filter widget. Default: ``mm_filteritem_default``;
       for checkbox-specific output: ``mm_filteritem_checkbox``.
   * - Mode
     - Defines whether the filter rule is configured as a yes checkbox, no checkbox,
       or as radio buttons (yes/no selection):

       * **Yes checkbox** — Filters for ``1`` (active)
       * **No checkbox** — Filters for ``0`` or empty (inactive)
       * **Radio buttons** — Outputs yes/no radio buttons; an additional option
         "Allow empty selection" (``blankoption``) appears
   * - "Yes/No" instead of attribute name
     - Displays "Yes/No" instead of the attribute name in the widget.
   * - Option label as parameter
     - The URL parameter value is the label of the option (text) instead of a number.
   * - CSS ID/class
     - Sets a CSS ID or class on the filter widget element.


Matching Attributes
------------------

The "Checkbox Status" filter rule is exclusively suitable for the following attribute:

* :ref:`Checkbox <component_attribute_checkbox>`


Special Functions
----------------

**Publication control**

The most common use case is the filter rule as a "publication filter": a checkbox
attribute (typical column name: ``published``) with the "Toggle icon" option enabled
in the backend allows quick activation/deactivation of items. In the frontend, the
"Checkbox Status" filter rule filters out the inactive items.

**Template mm_filteritem_checkbox**

The included template ``mm_filteritem_checkbox.html5`` provides a checkbox-specific
output for the filter widget.


.. |svg_filt_checkbox_22| image:: /_img/icons_svg/filter_checkbox.svg
   :width: 22px
.. |img_filter_checkbox| image:: /_img/icons/filter_checkbox.png

.. |br| raw:: html

   <br />
