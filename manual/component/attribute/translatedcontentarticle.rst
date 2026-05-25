.. _component_attribute_translatedcontentarticle:

Translated Content of an Article
==================================

The "Translated Content of an Article" attribute is the multilingual variant of the
:ref:`Content of an Article <component_attribute_contentarticle>` attribute.
It enables separate Contao content elements per language for a MetaModels record.
The content is stored in the Contao table ``tl_content`` with an additional language field.

Typical use cases:

* Multilingual product descriptions with complex content layout
* Language-dependent detail page content with different structure per language
* Translated editorial content per item

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_contentarticle`.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.

.. seealso:: This attribute is supported by the :ref:`File-Usage Integration <rst_extended_file-usage>`.
   This allows the Contao file manager to display whether and where a file is embedded.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedcontentarticle


Settings when Creating the Attribute
--------------------------------------

The attribute has no specific settings when creating it.
Only the general attribute settings are used:

* Name, column name, description
* Override variants


Settings in Render Settings
-----------------------------

The attribute has no specific render settings. In the attribute list of a render setting,
the usual options are available:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Template
     - Selection of a custom template for the output of the content elements.
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

.. note:: The attribute can only be populated with content elements after the record
   has been saved for the first time.


Filter Rules
------------

The attribute does not support its own filter rules.


Special Functions
-----------------

**Database storage**

The content elements are stored in ``tl_content`` — identical to the monolingual
variant, but with an additional field:

* ``pid`` — ID of the MetaModels record
* ``ptable`` — name of the MetaModel table
* ``mm_slot`` — column name of the attribute
* ``mm_lang`` — language code of the content element (e.g. ``de``, ``en``)

The content elements are retrieved language-specifically. If a content element is
missing for a language, the fallback language is used automatically.

**Backend widget**

The backend widget displays the content elements of the currently active backend
language. The language selection is automatically passed to the widget via the
``setWidgetLanguage`` event listener.

**Duplicating records**

When a MetaModels record is duplicated, the associated content elements of all
languages are copied along with it. Copying from one language to the same language
is not possible (error notification).

**Fallback language**

If a set of content elements is missing for a language, the content of the fallback
language is output automatically.


.. |br| raw:: html

   <br />
