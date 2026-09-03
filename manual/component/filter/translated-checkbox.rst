.. _component_filter_translated-checkbox:

|svg_filt_translated_checkbox_22| |img_filter_checkbox| Translated Checkbox Status
==================================================================================

The "Translated Checkbox Status" filter rule (package ``filter_checkbox``) checks whether
the value of a translated checkbox attribute is equal to ``1`` (active). It is
functionally identical to the :ref:`component_filter_checkbox` filter rule, but is
designed for use with the :ref:`Translated Checkbox <component_attribute_translatedcheckbox>`
attribute type in multilingual MetaModels.

The translated checkbox status is evaluated language-specifically: the value of the
translated checkbox is checked in the active language. This allows publication states
to be controlled per language.

.. seealso:: For monolingual MetaModels, the filter rule
   :ref:`component_filter_checkbox` is available.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/filter_checkbox


Settings when Creating the Filter Rule
------------------------------------------

The settings are identical to those of the :ref:`component_filter_checkbox` filter rule.
The only difference is that the attribute type must be a
:ref:`component_attribute_translatedcheckbox`.

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Translated Checkbox Status".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The translated checkbox attribute whose value should be checked.
   * - URL parameter
     - The key of the URL parameter for passing the filter value. Without input,
       the column name of the attribute is used. With ``auto_item``, only the value —
       without key — is embedded in the URL.
   * - URL type for the parameter
     - Defines whether the parameter is passed as a slug (friendly URL) or as a
       GET parameter (from MM 2.4) — :ref:`see SEO <rst_cookbook_tips_seo_filter-url>`

Settings for the Frontend Widget
--------------------------------------

Identical to the settings of the :ref:`component_filter_checkbox` filter rule — the same
options (URL parameter, mode, template, etc.) are available.


Matching Attributes
------------------

The "Translated Checkbox Status" filter rule is exclusively suitable for the following
attribute:

* :ref:`Translated Checkbox <component_attribute_translatedcheckbox>`


.. |svg_filt_translated_checkbox_22| image:: /_img/icons_svg/filter_checkbox.svg
   :width: 22px
.. |img_filter_checkbox| image:: /_img/icons/filter_checkbox.png

.. |br| raw:: html

   <br />
