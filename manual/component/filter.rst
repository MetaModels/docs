.. _component_filter:

|img_filter_32| Filter Sets
============================

.. note:: Optionally create filter sets for backend and frontend;
  create filter set and activate in components or content elements/modules


Introduction
------------

The "Filter set" component provides a comprehensive tool for influencing the view and selection
of records (items) of a MetaModel. Filter sets reduce the total number of items, i.e. after
filtering, a subset of these is available for output. Note that each filter set always only
outputs a list of IDs (of the items), or a filter rule passes a list of IDs to the next filter
rule — changing item values via an SQL query, for example, is not possible.

A filter set is created in a two-level hierarchy, where first a named filter set "as a container"
is created, which in turn can contain one or more filter rules. If multiple filter rules exist
at this level, they are automatically linked with AND. For an OR link, an OR filter rule must
be created, which in turn can include further filter rules. With the possibilities of nesting,
almost all AND/OR combinations of a native SQL query can be replicated.

Some filter rules have the selectable option to show only assigned or only remaining filter
entries, to ensure a dynamic display of the filter set.

Filter sets can be used in both the backend and frontend.

Filter rules can be dynamically influenced, e.g. via GET/POST parameters, resulting in very
extensive filtering.


Types of Filter Rules
---------------------

* **Predefined item set** (core): |br|
  Enter a list of IDs to filter by
* **Simple lookup** (core): |br|
  Creates a filter by an attribute; a URL parameter can be specified for filtering; with the
  "Static parameter" option, a value for filtering can be activated in content elements/FE
  modules from a select list
* **Custom SQL** (core): |br|
  Custom SQL conditions for filtering; note the |img_help| help assistant (popup) |br|
  see also in the "Cookbook" :ref:`rst_cookbook_filter_custom-sql`
* **AND condition** (core): |br|
  Container for further filter rules with AND link
* **OR condition** (core): |br|
  Container for further filter rules with OR link; option to execute only the first matching rule
  (checkbox "Stop after first match")
* **Checkbox status** (filter_checkbox): |br|
  Checks an attribute value for 1; (formerly "Published status"); own template mm_filteritem_checkbox(.html5)
* **Translated checkbox status** (filter_checkbox): |br|
  Checks a translated attribute value for 1; (formerly "Translated published status"); own template
  mm_filteritem_checkbox(.html5)
* **Yes / No** (filter_checkbox): |br|
  Yes/No selection, e.g. as radio buttons
* **Value from/to for one field** (filter_fromto): |br|
  From/to selection for values of an attribute value
* **Value from/to for a date field** (filter_fromto): |br|
  From/to selection for date of an attribute value; own template mm_filteritem_datepicker(.html5) —
  `Set date to YYYY-MM-DD <https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/date>`_
* **Value from/to for two fields** (filter_range): |br|
  Two fields with values
* **Value from/to for two date fields** (filter_range): |br|
  Two fields with values for date; own template mm_filteritem_datepicker(.html5) —
  `Set date to YYYY-MM-DD <https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/date>`_
* **Single select** (filter_select): |br|
  Single selection of a value, e.g. from a select list; alternatively templates
  mm_filteritem_radiobutton(.html5) or mm_filteritem_linklist(.html5)
* **Multi-select** (filter_tags): |br|
  Multiple selection of values, e.g. from a checkbox list; alternatively template mm_filteritem_linklist(.html5)
* **Text filter** (filter_text): |br|
  Filters by text input
* **Perimeter search** (filter_perimetersearch): |br|
  Filters by an address/geo coordinates and a radius based on lat/long values in the records |br|
  see :ref:`extended_perimetersearch`
* **Register** (filter_register): |br|
  Filters by initial letters; generates a list of all or existing initial letters; own template
  mm_filteritem_register(.html5)
* **Levenshtein-based search** (attribute_levenshtein): |br|
  Creates a full-text index of selected attributes including similarity search and auto-completion;
  own template mm_filteritem_levenshtein(.html5)
* **Filter-by-related** (filter_by_related) [from MM 2.4]: |br|
  Enables filtering items with properties from a linked (relation) MetaModel; relations can be
  built via child table or single select |br|
  see :ref:`rst_extended_filter_by_related`
* **Loupe** (filter_loupe) [from MM 2.4]: |br|
  Creates a full-text index of selected attributes in a dedicated SQLite database — based on
  `Loupe <https://github.com/loupe-php/loupe>`_; more about this in the :ref:`Loupe filter rule <rst_extended_loupe>`
* **Expression rule** (filter_expression) [from MM 2.4]: |br|
  Allows the execution of further filter rules to be linked to conditions. A node is created in the
  rule list that can contain one or at most two further filter rules as child nodes; more about this
  in the :ref:`Expression filter rule <rst_cookbook_filter_expression-rule>`


Configuration Parameters
------------------------

The various filter rules can be adapted to individual requirements via specific configuration
options. For most filter rules, the following parameters can be set:

* **URL parameter:** defines the keyword (key) for the URL; without specification this is the column
  name of the attribute. With the keyword ``auto_item``, the keyword is not included in the URL
  but only the value is output — ``auto_item`` can only be used for one filter rule. The keywords
  ``language`` and ``items`` are reserved by Contao — from MM 2.3 these are automatically rewritten
  and ``__`` is appended if set as column name.
* **URL type for the parameter:** (from MM 2.4) here you can set whether the filter parameter is passed
  to the URL as a slug or GET parameter — more about this in the :ref:`SEO tips <rst_cookbook_tips_seo_filter-url>`
* **Template:** selection of the widget template for the frontend display; in addition to the template
  ``mm_filteritem_default``, various filter rules bring their own templates such as checkbox, Levenshtein,
  register, etc. The templates can be customized in the usual Contao way. The surrounding template
  (wrapper) is selected in the CE/FE module filter.
* **CSS ID/class:** sets an ID or CSS class in the widget to be output; this allows individual control
  of the view/formatting.


Procedure
---------

A new filter set is created via "|img_new| New" and a name must be assigned.

Via the icon "|img_filter_setting| Filter rules" you reach the filter rule entry list, where
a new filter rule can be set up via "|img_new| New". Via the "clipboard icons", the hierarchy
can be influenced during the creation of a filter rule, e.g. to insert the filter rule within
an OR rule.


.. seealso:: In the cookbook:

   * :ref:`rst_cookbook_checklists_filter`
   * :ref:`rst_cookbook_filter_exclusion`


.. _component_filter_list:
Details of All Filter Rules
----------------------------

.. toctree::
   :maxdepth: 1

   filter/idlist
   filter/simplelookup
   filter/customsql
   filter/condition-and
   filter/condition-or
   filter/expression-rule
   filter/checkbox
   filter/translated-checkbox
   filter/yes-no
   filter/fromto
   filter/fromto-date
   filter/range
   filter/range-date
   filter/select
   filter/tags
   filter/text
   filter/perimeter-search
   filter/register
   filter/levenshtein
   filter/by-related
   filter/loupe
   filter/parent


.. |img_filter_32| image:: /_img/icons/filter_32.png
.. |img_filter| image:: /_img/icons/filter.png
.. |img_filter_setting| image:: /_img/icons/filter_setting.png
.. |img_new| image:: /_img/icons/new.gif
.. |img_about| image:: /_img/icons/about.png
.. |img_help| image:: /_img/icons/help.svg

.. |br| raw:: html

   <br />
