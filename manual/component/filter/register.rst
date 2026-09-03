.. _component_filter_register:

|svg_filt_register_22| |img_filter_default| Register
====================================================

The "Register" filter rule (package ``filter_register``) filters items by the first
letter of an attribute value. It generates a list of all existing or possible initial
letters as a clickable navigation (A–Z) through which visitors can restrict the output
to items starting with a specific letter.

Typical use cases: alphabetical navigation in business directories, person lists,
glossaries, or other alphabetically sorted outputs.

The included template ``mm_filteritem_register.html5`` is intended for the
register-specific output.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_register


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Register".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The text attribute by whose first letter items should be filtered
       (e.g. last name, company name).

Settings for the Frontend Widget
--------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - URL parameter
     - The key of the URL parameter for the selected letter.
   * - URL type for the parameter
     - Defines whether the parameter is passed as a slug (friendly URL) or as a
       GET parameter (from MM 2.4) — :ref:`see SEO <rst_cookbook_tips_seo_filter-url>`
   * - Label
     - Label of the filter widget.
   * - Hide label at filter widget
     - Suppresses the output of the label.
   * - Template
     - Template for the widget output. Default: ``mm_filteritem_register``.
   * - Show count
     - Shows the number of items for each letter.
   * - Hide empty letters
     - Hides letters for which no items exist.
   * - Allow multiple filtering
     - Allows simultaneous filtering by multiple initial letters.
   * - Only remaining values
     - Shows only letters for which results still exist after applying the other
       active filters.
   * - Ignore this filter for remaining values
     - This filter does not return its own options as a restriction when calculating
       remaining values.
   * - CSS ID/class
     - Sets a CSS ID or class on the widget element.


Matching Attributes
------------------

The "Register" filter rule is suitable for attributes with text values:

* :ref:`Text <component_attribute_text>`
* :ref:`Alias <component_attribute_alias>`
* :ref:`Combined values <component_attribute_combinedvalues>`
* :ref:`Token <component_attribute_token>`
* :ref:`Translated text <component_attribute_translatedtext>`


Special Functions
----------------

**Template mm_filteritem_register**

The included template renders the letter list with links. The template can be
customized using the standard Contao method (template inheritance) to integrate a
different layout or special characters.


.. |svg_filt_register_22| image:: /_img/icons_svg/filter_register.svg
   :width: 22px
.. |img_filter_default| image:: /_img/icons/filter_default.png

.. |br| raw:: html

   <br />
