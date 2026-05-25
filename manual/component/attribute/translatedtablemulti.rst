.. _component_attribute_translatedtablemulti:

Translated Multi-Table (MCW)
================================

The "Translated Multi-Table (MCW)" attribute is the multilingual variant of the
:ref:`Multi-Table <component_attribute_tablemulti>` attribute. It enables separate
table values per language with the same column structure. The data is stored in a
dedicated translation table, so **no own column** is created in the MetaModel table
for this attribute.

Typical use cases:

* Multilingual technical specifications with mixed input types
* Translated opening hours or feature tables

.. warning:: The column structure is **not** configured in the MetaModels backend,
   but in a PHP configuration file. This requires developer knowledge.

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_tablemulti`.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.

.. seealso:: This attribute is supported by the :ref:`File-Usage Integration <rst_extended_file-usage>`.
   This allows the Contao file manager to display whether and where a file is embedded.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedtablemulti


Settings when Creating the Attribute
--------------------------------------

The attribute has no own settings in the MetaModels backend. The column structure is
configured in the PHP configuration file via
``$GLOBALS['TL_CONFIG']['metamodelsattribute_multi']`` — identical to the monolingual
variant:

.. code-block:: php

   $GLOBALS['TL_CONFIG']['metamodelsattribute_multi']['mm_example']['my_field'] = [
       'minCount'     => 0,
       'maxCount'     => 0,
       'columnFields' => [
           'col_name' => [
               'label'     => 'Label',
               'inputType' => 'text',
               'eval'      => ['style' => 'width:200px'],
           ],
       ],
   ];


Settings in Render Settings
-----------------------------

The attribute has its own render setting:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Hide table header
     - Hides the column headings in the frontend output.
   * - Template
     - Selection of a custom template for the output.
   * - CSS class
     - Optional CSS class added to the output element.


Settings in the Input Form
----------------------------

When the attribute is added to an input form, the following options are available:

**Display**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Backend class
     - CSS classes for the display of the field in the backend form.
   * - Template for backend
     - Selection of a custom widget template for the backend form.
   * - Template for frontend
     - Selection of a custom widget template for frontend editing
       (only available if the "Frontend Editing" extension is installed).


Filter Rules
------------

The translated multi-table attribute does not support filter rules —
``getFilterOptions()`` returns an empty array.


Special Functions
-----------------

**Database storage**

The table values are stored per language in ``tl_metamodel_translatedtablemulti``
(fields: ``att_id``, ``item_id``, ``langcode``, ``row``, ``col``, ``value``).
No column is created in the MetaModel table.

**Fallback language**

If a value is missing for a language, MetaModels automatically falls back to the
fallback language.

**Difference from the monolingual variant**

The only structural difference from the monolingual multi-table attribute is the
additional ``langcode`` column in the value table and the use of the language-aware
``getTranslatedDataFor()`` and ``setTranslatedDataFor()`` methods.


.. |br| raw:: html

   <br />
