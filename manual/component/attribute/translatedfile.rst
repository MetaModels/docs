.. _component_attribute_translatedfile:

|svg_attr_translatedfile_22| |img_file| Translated File
=======================================================

The "Translated File" attribute is the multilingual variant of the
:ref:`File <component_attribute_file>` attribute. It provides a separate file picker
per language for selecting files from the Contao file directory. The values are not
stored in the MetaModel table, but in the translation table
``tl_metamodel_translatedlongblob``.

Typical use cases:

* Language-specific product images (e.g. a DE product photo with German text,
  an EN product photo with English text)
* Different PDFs per language (e.g. German and English data sheets)
* Language-dependent media content such as videos or audio files

.. seealso:: The monolingual variant of this attribute is described under
   :ref:`component_attribute_file`.

.. seealso:: This attribute is supported by the :ref:`File-Usage Integration <rst_extended_file-usage>`.
   This allows the Contao file manager to display whether and where a file is embedded.

.. seealso:: For file uploads in the frontend, the
   :ref:`rst_extended_frontend_editing` extension is available.

.. seealso:: Information on multilingual support in MetaModels can be found on the
   :ref:`component_multi-language` page.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_translatedfile


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings, the file attribute offers the following
specific options:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Multiple selection
     - Allows the selection of multiple files. If this option is not set, only a single
       file can be selected.
   * - Specify root folder
     - Restricts the file picker to a specific starting folder in the file directory.
   * - Valid file types
     - Comma-separated list of allowed file extensions (e.g. ``jpg,jpeg,png,gif``).
   * - Allowed file types
     - Restriction of the selection to files, folders, or both.


Settings in Render Settings
-----------------------------

The attribute has its own render settings for output:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Use as image field with thumbnail
     - Enables image output with thumbnail generation. This option **must** be set
       for any direct image display in the backend or frontend.
   * - Image width and height
     - Size specification for the generated thumbnail (width × height).
   * - Create link as download or lightbox
     - Embeds the file in a link that serves either as a download or for a large view
       in a lightbox.
   * - Protected download
     - The download URL is only temporarily valid (time-limited signed URL).
   * - Sort by
     - Defines the sort order for multiple files: name ascending/descending, date
       ascending/descending, or random.
   * - Image as placeholder
     - Selects a placeholder image to be displayed when no file is selected.


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
   * - Widget mode
     - Determines the display type of the file widget. Available modes:

       * **Normal** — Standard file picker
       * **Downloads** — Display as download list
       * **Gallery** — Display as image gallery
       * **FE single upload** — Frontend upload for a single file
       * **FE single upload with preview** — Frontend upload with thumbnail
       * **FE multi upload** — Frontend upload for multiple files
       * **FE multi upload with preview** — Frontend upload with thumbnails

**Upload settings** (only for frontend editing modes)

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Target folder
     - Folder in the file directory where uploaded files are stored.
   * - Use home directory
     - Stores the file in the home directory of the logged-in member.
   * - Expand folder
     - Expands the target folder path dynamically via insert tags or
       ``##post_*##`` tokens.
   * - Normalize expanded folder
     - Normalizes the expanded folder path with the alias generator.
   * - Normalize filename
     - Normalizes the filename on upload with the alias generator.
   * - Filename prefix / postfix
     - Prepends or appends a fixed or dynamic text to the filename.
   * - Preserve existing files
     - Adds a numeric suffix for duplicate filenames instead of overwriting the file.
   * - Deselect file
     - Allows the user to remove a file from the record.
   * - Delete file
     - Allows the user to remove a file and delete it from the file directory.
   * - Sort by
     - Defines the sort order of uploaded multiple files.
   * - Width and height of thumbnails
     - Size of the thumbnails displayed in the upload widget.

**Functions**

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Required field
     - Makes the field a required field.

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

No dedicated frontend filter rule is currently available for the translated file attribute.
A search by filename or UUID is possible in the backend.


Special Functions
-----------------

**Database storage**

The file references are stored per language in ``tl_metamodel_translatedlongblob``
(fields: ``att_id``, ``item_id``, ``langcode``, ``value`` as ``blob``).
No column is created in the MetaModel table. The manually defined order of multiple
files is embedded in the value itself.

.. note:: Up to MetaModels 2.4, the ``value_sorting`` field of the same table held the
   sort order. Contao removed the corresponding widget option ``orderField`` in version
   5.0, so this field is dropped with MetaModels 2.5 — a migration transfers the existing
   order into the value and then deletes the field. See :ref:`new_in_mm250`.

**Language-dependent files**

Each language version can reference a completely different file. In the backend, the
file widget appears per language with the language-specific value.

**Fallback language**

If a file is missing for a language, MetaModels falls back to the fallback language.

**Sorting for multiple files**

The order of multiple files can be configured independently in the render settings
(for output) and in the input form settings (for frontend upload).

Independently of this, the order in the input form can also be set **manually per
language via drag and drop**: in the *Gallery* and *Downloads* widget modes, the
selected files are sortable when *Multiple selection* is active. The preview images
also carry a red button that lets you remove a single file from the selection without
opening the file picker.


.. |svg_attr_translatedfile_22| image:: /_img/icons_svg/file.svg
   :width: 22px
.. |img_file| image:: /_img/icons/file.png
.. |br| raw:: html

   <br />
