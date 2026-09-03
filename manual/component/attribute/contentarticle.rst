.. _component_attribute_contentarticle:

|svg_attr_contentarticle_22| |img_article| Content of an Article
================================================================

The "Content of an article" attribute allows Contao content elements to be assigned to a
MetaModels record — analogous to the content elements of a Contao article. The content
is stored in the Contao table ``tl_content`` and managed via a dedicated backend widget.

Typical use cases:

* Product descriptions with complex content layouts (text, images, tables)
* Detail pages with editorially prepared content per item
* Multiple content areas per record (e.g. main content + sidebar)

In the backend, a widget appears that displays a list of the assigned content elements
and provides a direct link to the content management interface.

.. seealso:: For multilingual MetaModels, the attribute
   :ref:`component_attribute_translatedcontentarticle` is available.

.. seealso:: This attribute is supported by the :ref:`File-Usage integration <rst_extended_file-usage>`.
   This allows the Contao file manager to display whether and where a file is embedded.

.. seealso:: The output of MetaModels lists and filters in the frontend is described on the page
   :ref:`component_contentelements`.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_contentarticle


Settings when Creating the Attribute
--------------------------------------

The attribute has no specific settings when creating. Only the general attribute settings are used:

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

.. note:: The attribute can only be filled with content elements after the record has been
   saved for the first time. As long as the record has not yet been saved, a corresponding
   notice message appears.


Filter Rules
------------

The "Content of an article" attribute does not support its own filter rules —
content elements cannot be used as filter criteria in the frontend.


Special Functions
-----------------

**Storage**

The content elements are **not** stored in the MetaModel table, but as regular Contao
content elements in ``tl_content`` with the following linking fields:

* ``pid`` — ID of the MetaModels record
* ``ptable`` — name of the MetaModel table (e.g. ``mm_products``)
* ``mm_slot`` — column name of the attribute (enables multiple article attributes per MM)

The content elements are retrieved via ``ContentModel::findPublishedByPidAndTable()``
and rendered with ``Controller::getContentElement()``.

**Backend widget**

In the backend form, the widget displays a list of existing content elements with type and
visibility status. A link leads directly to the content element management interface.
AJAX requests are updated directly in the widget container.

**Duplicating records**

When a MetaModels record is duplicated or pasted, the assigned content elements are
automatically copied as well. MetaModels detects all ``contentarticle`` attributes and
creates copies of the ``tl_content`` entries with the new ``pid`` value.

**Recursion protection**

The extension contains recursion protection to prevent infinite loops if content elements
themselves reference MetaModels content.


.. |svg_attr_contentarticle_22| image:: /_img/icons_svg/article.svg
   :width: 22px
.. |img_article| image:: /_img/icons/article.png
.. |br| raw:: html

   <br />
