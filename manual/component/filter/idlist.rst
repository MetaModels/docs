.. _component_filter_idlist:

|svg_filt_idlist_22| |img_filter_default| Predefined Item Set
=============================================================

The "Predefined Item Set" filter rule allows specifying a fixed list of item IDs as the
filter basis. The filter set only outputs those items whose ID is contained in the
specified list. This filter rule is suitable for static selections where the records to
be displayed are known in advance, e.g. for "Featured entries" or "Highlights".

This filter rule has no frontend widget output and is configured exclusively in the
backend.


Installation
------------

This filter rule is part of ``metamodels/core`` and is available without additional
packages after the basic MetaModels installation.


Settings when Creating the Filter Rule
------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Setting
     - Description
   * - Type
     - Selection of the filter rule type — here: "Predefined set of items".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Items
     - Comma-separated list of item IDs to filter by. Only items with these IDs are
       considered in the output.


Matching Attributes
------------------

The "Predefined Item Set" filter rule does not work attribute-based, but directly with
the item IDs of the MetaModels table. Therefore no attribute selection is required.


Special Functions
----------------

The filter rule can be combined with other filter rules. Since it always returns a fixed
set of IDs, it is particularly suitable as an AND condition in combination with dynamic
filter rules to restrict the total set of searchable items in advance.


.. |svg_filt_idlist_22| image:: /_img/icons_svg/filter_idlist.svg
   :width: 22px
.. |img_filter_default| image:: /_img/icons/filter_default.png

.. |br| raw:: html

   <br />
