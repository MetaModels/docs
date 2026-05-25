.. _component_filter_condition-or:

|img_filter_or| OR Condition
=====================================

The "OR Condition" filter rule is a container that can hold multiple sub-filter rules.
The contained filter rules are combined with an OR link: an item must fulfill at least
one of the sub-conditions to appear in the result set.

This filter rule allows filter alternatives to be mapped, e.g. "Show all items of
category A or category B". By nesting with AND conditions, complex combinations such as
``(A AND B) OR (C AND D)`` can be realized.

The "Stop after first match" option allows a performance optimization: as soon as a
sub-rule finds items, the subsequent sub-rules are no longer executed.

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
     - Selection of the filter rule type — here: "OR condition (OR)".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Stop after first match
     - If this option is active, subsequent sub-rules are no longer executed as soon as
       a sub-rule has found at least one item. This can reduce database queries and
       improve performance.


Matching Attributes
------------------

The OR condition is not an attribute-bound filter, but a structural container. The
contained sub-filter rules can use any attributes.


Special Functions
----------------

**Nesting with AND conditions**

By combining OR and AND conditions, logical expressions can be built that replicate
native SQL WHERE clauses with AND/OR.

Example of a three-way OR link with two AND conditions each:

.. code-block:: text

   OR condition
   ├── AND condition
   │   ├── Filter rule A (e.g. category = "Sports")
   │   └── Filter rule B (e.g. status = published)
   └── AND condition
       ├── Filter rule C (e.g. category = "Culture")
       └── Filter rule D (e.g. status = published)


.. |img_filter_or| image:: /_img/icons/filter_or.png

.. |br| raw:: html

   <br />
