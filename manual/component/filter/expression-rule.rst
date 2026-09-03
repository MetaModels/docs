.. _component_filter_expression-rule:

|svg_filt_expression_rule_22| |img_filter_expression| Expression Rule
=====================================================================

The "Expression Rule" filter rule (from MM 2.4) allows the execution of further filter
rules to be tied to a condition. A node is created in the rule list that can hold one or
at most two further filter rules as child nodes. If the condition is met, the first
sub-rule is executed; if it is not met, the second sub-rule is executed — if present
(if/else principle).

This filter rule has no independent frontend widget output. The "Only remaining values"
option affects which options are displayed in other filter widgets.

.. seealso:: Practical examples for the Expression Rule can be found in the cookbook:
   :ref:`rst_cookbook_filter_expression-rule`


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
     - Selection of the filter rule type — here: "Expression Rule".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Expression rule
     - The condition to be evaluated as an expression. Evaluated at runtime; if the
       expression returns ``true``, the first sub-rule is executed, otherwise the
       second (if present).
   * - Only remaining values
     - Shows only values in other filter widgets for which results still exist after
       applying this expression rule.


Matching Attributes
------------------

The expression rule is not directly attribute-bound. The filter rules used in the
sub-rules can reference any attributes.


.. |svg_filt_expression_rule_22| image:: /_img/icons_svg/filter_expression.svg
   :width: 22px
.. |img_filter_expression| image:: /_img/icons/filter_expression.png

.. |br| raw:: html

   <br />
