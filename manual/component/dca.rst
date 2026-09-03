.. _component_dca:

|svg_dca_32| |img_dca_32| Input Forms
=======================================

.. note:: Create input forms for data entry;
  add, activate and configure attributes; optionally define
  display conditions for input fields; definition of
  grouping and sorting of stored items possible

Introduction
------------

Input forms are necessary to populate the database from the backend. Each input form can
include the attributes defined per MetaModel as input elements.

One or more different input forms can be created for each MetaModel, equipped with different
attribute input fields. This allows different permissions or workflows to be covered.

The creation of input forms is also divided into the basic settings of the input form, the
activation of attributes, and the selection of specific options for individual attributes
such as required field, arrangement, validation, etc. Most configuration options reflect the
possibilities of the "DCA" of the "Contao framework" (see `DCA <https://docs.contao.org/books/api/dca/index.html>`_).
More about the options under "Procedure".

One of the most important points in the basic settings is the selection of the "Integration"
option with the options "Standalone" or "Child table". With "Standalone", the input form is
integrated into one of the navigation blocks in Contao, and with "Child table" it is assigned
to an existing MetaModel or Contao table.

When selecting "Child table", note that the "Render mode" must be set to "Parent element exists"
if items are to be clearly assigned to a parent item. Otherwise, all child items are listed
for all parent items.

The display of the input field can be influenced via additional control parameters. Each render
setting has an edit icon for creating dependencies for the display and visibility ("visibility
conditions"). This allows one or more input fields in the input form to be visible only when,
for example, a specific checkbox is checked.

For each input form, one or more groupings and sortings can be defined for a clear display of
the stored items.

If you want the display of items in the list view as a tree structure or hierarchy, two basic
settings are necessary:

* in the input form properties, set the "Render mode" to "Hierarchy" (table view off)
* in the input form sorting, add a sorting as default with "Activate manual sorting"


Options of the Input Form
--------------------------
* **Name**: |br|
  Label
* **Panel layout**: |br|
  Configuration of tools in the header: search, sort, filter, limit;
  for search and filtering of attributes, the option must be set in the input widgets
* **Integration**: |br|
  "Standalone" with selection of the backend section; "As child table" with selection
  of the parent table
* **Render mode**: |br|
  Output mode of the listing as "Flat (without hierarchy)" or "Hierarchy",
  or additionally as "Parent element exists" for child tables
* **Display in table form**: |br|
  Option to display attributes as a table
* **Allow editing/creating/deleting**: |br|
  Permission to modify, create, delete entries

Options of the Input Field
---------------------------
* **Type**: |br|
  Legend: subdivision of the input panel ("green line") |br|
  Attribute: display of attribute options
* **Function-related settings**: |br|
  Activation of "read only" or "required field" |br|
  Further options depending on attribute type, e.g. input validation, TinyMCE activation, etc.
* **Display options**: |br|
  Specification of Contao CSS backend classes, e.g. "w50" for 50% width
* **Listing, filtering and sorting in backend**: |br|
  Checkboxes for filterable and/or searchable — depending on attribute type

Options of the Visibility Conditions of the Input Widget
---------------------------------------------------------
* **Type**: |br|
  Type of visibility conditions: AND/OR/NOT for linking or
  dependency via property from other attributes
* **Attribute/value** |br|
  Selection for dependency on another attribute

Options of Grouping and Sorting
---------------------------------
* **Name**: |br|
  Label
* **Activate manual sorting**: |br|
  When the value is set, items can be sorted manually; if the checkbox is not set,
  the following options can be set:
* **Grouping attribute**: |br|
  Selection of the attribute by which to group
* **Grouping length**: |br|
  The number of letters used for grouping (when grouping type is set)
* **Grouping type**: |br|
  Grouping type such as by initial letter or by time period such as week, month
* **Sorting attribute**: |br|
  Selection of the attribute by which to sort (optionally within a grouping)
* **Sorting direction**: |br|
  Sort direction: Ascending (ASC) or Descending (DESC)

A language key can be used to customize the display for grouping by week. With
``$GLOBALS['TL_LANG']['MSC']['week_format'] = 'K\W W. Y';``, for example, the output
``KW 43. 2023`` is generated (using `PHP date formatting <https://www.php.net/manual/en/datetime.format.php>`_,
the first ``W`` is escaped with ``\`` to output it instead of using it for PHP date formatting).

Procedure
---------

A new input form entry is created via "|img_new| New input form". After all necessary options
have been entered or selected, the setting is saved and appears in the list of existing input
forms for a MetaModel.

In addition to the "|img_edit| pencil icon", there is the icon for
"|img_dca_setting| Input form settings". Clicking on the icon opens a list of attributes
activated for the input form. If no attributes are present or need to be added, this can be
done via the "|img_dca_setting_add| Add all" icon — alternatively via "|img_new| New". The
"Add all" route requires confirmation twice.

The attributes of the input form are then available and may need to be activated.

For individual attributes, the template to be used can be changed and/or a special CSS class
entered ("|img_edit| Edit").

Via "|img_dca_condition| Visibility conditions" the visibility of the input widget in the
input form can be configured.

In the list view of input forms, various entries for sorting and grouping the stored items can
then be created via the "|img_dca_groupsortsettings| Sorting and grouping" icon.

.. seealso:: In the cookbook:

   * :ref:`rst_cookbook_inputmask_dca`
   * :ref:`rst_cookbook_inputmask_default-values`
   * :ref:`rst_cookbook_inputmask_regex`


.. |svg_dca_32| image:: /_img/icons_svg/dca.svg
   :width: 32px
.. |img_dca_32| image:: /_img/icons/dca_32.png
.. |img_dca| image:: /_img/icons/dca.png
.. |img_dca_setting| image:: /_img/icons/dca_setting.png
.. |img_dca_setting_add| image:: /_img/icons/dca.png
.. |img_dca_groupsortsettings| image:: /_img/icons/dca_groupsortsettings.png
.. |img_dca_condition| image:: /_img/icons/dca_condition.png
.. |img_new| image:: /_img/icons/new.gif
.. |img_edit| image:: /_img/icons/edit.gif

.. |br| raw:: html

   <br />
