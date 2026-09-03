.. _component_attribute_file:

|svg_attr_file_22| |img_file| File
==================================

The "File" attribute provides a file picker for selecting one or more files from the Contao
file directory. Typical use cases:

* Images for products, persons, or articles (single image or gallery)
* Download files such as PDFs, documents, or archives
* Videos, audio files, or other media files

The attribute supports various widget modes for different use cases in the backend and frontend.
For displaying images, the option "Use as image field with preview image" must be enabled in
the render settings.

.. seealso:: For multilingual MetaModels, the attribute
   :ref:`component_attribute_translatedfile` is available.

.. seealso:: This attribute is supported by the :ref:`File-Usage integration <rst_extended_file-usage>`.
   This allows the Contao file manager to display whether and where a file is embedded.

.. seealso:: For file upload in the frontend, the extension
   :ref:`rst_extended_frontend_editing` is available.


Installation
------------

The attribute is installed via the **Contao Manager** or **Composer**:

.. code-block:: bash

   composer require metamodels/attribute_file


Settings when Creating the Attribute
--------------------------------------

In addition to the general attribute settings (name, column name, description, override variants),
the file attribute offers the following specific options:

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
       If no folder is selected, the picker starts at the root directory.
   * - Valid file types
     - Comma-separated list of allowed file extensions (e.g. ``jpg,jpeg,png,gif``) to
       override the Contao default setting.
   * - Allowed file types
     - Restricts the selection to files, folders, or both (default: no restriction).


Settings in Render Settings
-----------------------------

The file attribute has its own render settings for output:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Option
     - Description
   * - Use as image field with preview image
     - Enables image output with preview image generation. Without this option, only the
       file path is output. For any direct image display in the backend or frontend,
       this option **must** be set.
   * - Image width and height
     - Size specification for the generated preview image (width × height). If only one
       of the two fields is filled in, it is scaled proportionally. Leaving both fields
       empty outputs the image in its original size.
   * - Create link as download or lightbox
     - Embeds the file in a link that serves either as a download or for a large view
       in a lightbox.
   * - Protected download
     - The download URL is only temporarily valid (time-limited signed URL).
   * - Sort by
     - Defines the sort order for multiple files: name ascending/descending,
       date ascending/descending, or random.
   * - Image as placeholder
     - Selects a placeholder image that is displayed when no file is selected.


Settings in the Input Form
----------------------------

When the file attribute is added to an input form, the following options are available:

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
     - Determines the display mode of the file widget. Available modes:

       * **Normal** — Standard file picker
       * **Downloads** — Display as download list
       * **Gallery** — Display as image gallery
       * **FE single upload** — Frontend upload for a single file
       * **FE single upload with preview** — Frontend upload with preview image
       * **FE multi upload** — Frontend upload for multiple files
       * **FE multi upload with preview** — Frontend upload with preview images

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
   * - Extend folder
     - Extends the target folder path dynamically via insert tags or ``##post_*##`` tokens.
   * - Normalize extended folder
     - Normalizes the extended folder path using the alias generator.
   * - Normalize file name
     - Normalizes the file name on upload using the alias generator.
   * - File name prefix / postfix
     - Prepends or appends a fixed or dynamic text to the file name.
   * - Keep existing files
     - Adds a numeric suffix for duplicate file names instead of overwriting the file.
   * - Deselect file
     - Allows the user to remove a file from the record (without deleting it).
   * - Delete file
     - Allows the user to remove a file and delete it from the file directory.
   * - Sort by
     - Defines the sort order of uploaded multiple files.
   * - Width and height of preview images
     - Size of the preview images displayed in the upload widget.

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
     - The attribute is available in the backend as a filter criterion
       (search by file name or UUID).
   * - Searchable
     - The attribute is available in the backend as a search field.


Filter Rules
------------

There is currently no dedicated frontend filter rule for the file attribute. In the backend,
a search by file name or UUID is possible.


Special Functions
-----------------

**Database storage**

Single files are stored as binary UUIDs. Multiple files are stored as a serialized array
of UUIDs in a ``blob NULL`` field. The manually defined order is embedded in the value
itself.

.. note:: Up to MetaModels 2.4, an additional column ``<column_name>__sort`` was created
   for the sort order when the *Multiple selection* option was set. Contao removed the
   corresponding widget option ``orderField`` in version 5.0, so this column is dropped
   with MetaModels 2.5 — a migration transfers the existing order into the value and then
   deletes the column. See :ref:`new_in_mm250`.

**Sorting for multiple files**

The order of multiple files can be configured independently in both the render settings
(for output) and the input form settings (for frontend upload).

Independently of this, the order in the input form can also be set **manually via drag
and drop**: in the *Gallery* and *Downloads* widget modes, the selected files are
sortable when *Multiple selection* is active. The preview images also carry a red button
that lets you remove a single file from the selection without opening the file picker.


.. |svg_attr_file_22| image:: /_img/icons_svg/file.svg
   :width: 22px
.. |img_file| image:: /_img/icons/file.png

.. |br| raw:: html

   <br />
