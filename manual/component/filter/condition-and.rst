.. _component_filter_condition-and:

|svg_filt_condition_and_22| |img_filter_and| AND Condition
==========================================================

The "AND Condition" filter rule is a container that can hold multiple sub-filter rules.
All contained filter rules are combined with an AND link: an item must fulfill all
sub-conditions to appear in the result set.

Since filter rules at the same level within a filter set are automatically linked by AND
anyway, the AND condition is primarily needed for structural organization within an OR
condition. This allows complex filter combinations such as ``(A AND B) OR (C AND D)``
to be built.

This filter rule has no frontend widget output.


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
     - Selection of the filter rule type — here: "AND condition (AND)".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.


Matching Attributes
------------------

The AND condition is not an attribute-bound filter, but a structural container. The
contained sub-filter rules can use any attributes.


Special Functions
----------------

**Hierarchical filter structure**

Via the clipboard icons in the filter rule list, a filter rule can be inserted into an
AND condition (or OR condition). This creates a nested filter structure that can map
nearly arbitrarily complex logical expressions.


.. |svg_filt_condition_and_22| image:: /_img/icons_svg/filter_and.svg
   :width: 22px
.. |img_filter_and| image:: /_img/icons/filter_and.png

.. |br| raw:: html

   <br />
