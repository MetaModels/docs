.. _component_filter_checkbox:

|svg_filt_checkbox_22| |img_filter_checkbox| Checkbox Status
============================================================

The "Checkbox Status" filter rule (package ``attribute_checkbox``) checks whether the value of a checkbox attribute
is equal to ``1`` (active). It is used for publication control: only items with an activated checkbox (e.g.
``published = 1``) are displayed in the frontend. The rule outputs **no** widget in the frontend and adds no parameter
to the URL.

.. warning:: Do not confuse it with the :ref:`"Yes / No" <component_filter_yes-no>` filter rule (package
   ``filter_checkbox``): that is a widget for the visitors. It only filters once someone makes a choice, and it puts a
   parameter like ``published/1`` into every URL the filter generates, including the links of a list to the detail
   page. For "only published items", "Checkbox Status" is always the right choice.

.. seealso:: For multilingual MetaModels, the filter rule
   :ref:`component_filter_translated-checkbox` is available.


Installation
------------

The filter rule is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_checkbox


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Checkbox Status".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Attribute
     - The checkbox attribute whose value should be checked.
   * - Allow override
     - A URL parameter can override the filter rule (e.g. for preview links).
   * - Do not use filter in frontend preview
     - The filter rule is skipped when a backend user uses the Contao frontend preview. The option "Show unpublished
       items" is then ignored.


Matching Attributes
------------------

The "Checkbox Status" filter rule is exclusively suitable for the following attribute:

* :ref:`Checkbox <component_attribute_checkbox>`


Special Functions
----------------

**Publication control**

The most common use case is the filter rule as a "publication filter": a checkbox attribute (typical column name:
``published``) with the "Toggle icon" option enabled in the backend allows quick activation/deactivation of items. In
the frontend, the "Checkbox Status" filter rule filters out the inactive items - even if a visitor has bookmarked the
link to an item and the filter belongs to the detail page.

The filter rules of a level are always linked with AND: the "Checkbox Status" simply sits next to the other rules of the
filter, no "AND" rule is needed for it.


.. |svg_filt_checkbox_22| image:: /_img/icons_svg/filter_checkbox.svg
   :width: 22px
.. |img_filter_checkbox| image:: /_img/icons/filter_checkbox.png

.. |br| raw:: html

   <br />
