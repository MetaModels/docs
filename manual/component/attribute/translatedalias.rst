.. _component_attribute_translatedalias:

|svg_attr_translatedalias_22| |img_alias| Translated Alias
==========================================================

The "Translated Alias" attribute is the multilingual variant of the
:ref:`Alias <component_attribute_alias>` attribute. It generates a separate
URL-compatible identifier (slug) per language. The values are not stored in the
MetaModel table, but in the translation table ``tl_metamodel_translatedtext``.

Typical use cases:

* Multilingual URL parameters for filtering (e.g. ``/products/my-product``
  in English, ``/produkte/mein-produkt`` in German)
* Readable, stable identifiers for deep links or :ref:`SEO URLs <rst_cookbook_tips_seo_url>`
* Unique short identifiers automatically generated from names or titles

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_alias`.

.. note:: An alias is not automatically unique. To ensure uniqueness, the "Unique values"
   option in the general attribute settings must be enabled.

.. warning:: If the "Force alias regeneration" option is enabled, existing alias values
   will be regenerated on every change to the source attributes. This may invalidate
   already published URLs.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedalias


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the translated alias attribute offers the following specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Alias fields
     - Selection of one or more attributes from which the alias is generated.
       In addition to MetaModels attributes, system meta-fields (ID, PID, sorting,
       etc.) are also available.
   * - Force alias regeneration
     - If this checkbox is active, the alias is automatically regenerated whenever
       one of the source attributes changes. The field is displayed as read-only in
       the backend.
   * - Valid alias characters
     - Character set for automatic generation: *Unicode digits and lowercase letters*
       (default), *Unicode digits and letters*, *ASCII digits and lowercase letters*,
       *ASCII digits and letters*.
   * - Do not add integer prefix
     - If this checkbox is active (default), no ``id-`` prefix is prepended when
       the resulting alias is purely numeric.
   * - Alias prefix and postfix
     - Optional prefix and postfix prepended or appended to the alias. Unlike the
       monolingual alias, a separate prefix/postfix can be set per language here
       (multi-column wizard with language selection, prefix field, and postfix field).

.. note:: The "Conversion language" option from the monolingual alias attribute is not
   available here — the active MetaModels language is used automatically.


Settings in Render Settings
-----------------------------

The attribute has no specific render settings. In the attribute list of a render setting,
the usual options (Template, CSS class) are available.


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
     - CSS classes for the display (e.g. ``w50``, ``long``).
   * - Template for backend
     - Selection of a custom widget template for the backend form.
   * - Template for frontend
     - Selection of a custom widget template for frontend editing
       (only available if the "Frontend Editing" extension is installed).

**Functions**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Required field
     - Makes the field a required field.
   * - Always save
     - Set automatically when "Force alias regeneration" is active.

**Overview (backend filter and search)**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Filterable
     - The attribute is available in the backend as a filter criterion.
   * - Searchable
     - The attribute is available in the backend as a search field.


Filter Rules
------------

Same filter rules as the monolingual :ref:`Alias <component_attribute_alias>` attribute:
Simple lookup, Text search, Levenshtein, Loupe.


Special Functions
-----------------

**Database storage**

The alias values are stored per language in ``tl_metamodel_translatedtext``
(fields: ``att_id``, ``item_id``, ``langcode``, ``value`` as ``varchar(255)``).
No column is created in the MetaModel table.

**Language-dependent prefix/postfix**

Unlike the monolingual alias, each language version can be assigned its own prefix or
postfix (e.g. ``de-`` for the German, ``en-`` for the English URL variant).

**Uniqueness with suffix**

When uniqueness is enabled, MetaModels checks separately per language and automatically
appends a counter for duplicates (``my-product-2``, etc.).


.. |svg_attr_translatedalias_22| image:: /_img/icons_svg/alias.svg
   :width: 22px
.. |img_alias| image:: /_img/icons/alias.png
.. |br| raw:: html

   <br />
