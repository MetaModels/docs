.. _component_filter_translated-checkbox:

|svg_filt_translated_checkbox_22| |img_filter_checkbox| Translated Checkbox Status
==================================================================================

The "Translated Checkbox Status" filter rule (package ``attribute_translatedcheckbox``, called "Translated published
status" in the backend) checks whether
the value of a translated checkbox attribute is equal to ``1`` (active). It is
functionally identical to the :ref:`component_filter_checkbox` filter rule, but is
designed for use with the :ref:`Translated Checkbox <component_attribute_translatedcheckbox>`
attribute type in multilingual MetaModels.

The translated checkbox status is evaluated language-specifically: the value of the
translated checkbox is checked in the active language. This allows publication states
to be controlled per language. As with the :ref:`Checkbox Status <component_filter_checkbox>`, the rule outputs no
widget in the frontend and adds no parameter to the URL.

.. warning:: The "Yes / No" rule (package ``filter_checkbox``), on the other hand, is a widget for the visitors and not
   suitable for publication control, see :ref:`component_filter_yes-no`.

.. seealso:: For monolingual MetaModels, the filter rule
   :ref:`component_filter_checkbox` is available.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedcheckbox


Settings when Creating the Filter Rule
------------------------------------------

The settings correspond to those of the :ref:`component_filter_checkbox` filter rule.
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
   * - Allow override
     - A URL parameter can override the filter rule (e.g. for preview links).
   * - Do not use filter in frontend preview
     - The filter rule is skipped when a backend user uses the Contao frontend preview.


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
