.. _rst_cookbook_filter_expression-rule:

Expression Filter Rule
========================

.. note:: Available from version 2.4 — currently an experimental feature!

The "Expression" filter rule allows the execution of further filter rules to be
tied to conditions. A node is created in the rule list that can hold one or at
most two further filter rules as child nodes.

The condition is defined in the filter rule as a
`Symfony Expression <https://symfony.com/doc/current/reference/formats/expression_language.html>`_
— if the condition is met, the first child node filter rule is executed; if the
condition is not met, the second child node filter rule is executed.

The second child node filter rule is optional — if it is not present, no item
IDs are passed to the list, i.e. no data is displayed in the list.

The structure could look as follows:

|img_expression_01|

If there is a collection of filter rules that should come into effect in the
first child node, they can be grouped in an AND condition.

|img_expression_02|

The following parameters are currently available in the expression syntax for
evaluation:

* ``filterUrl``: array with the URL filter parameters
* ``request``: the current request stack

.. note:: The display of filter widgets in the frontend is not affected by this
   filter rule — this feature may be added in the future.


Examples of structure:
**********************

**Task:** Show the list only when a filter value is set. |br|
**Expression:** ``filterUrl != []`` |br|
**Structure:**

* Filter rule Expression

  * Filter rule for filtering, e.g. multi-select or single select

No second filter rule needs to be created as a child node — but it is optionally possible.

**Task:** Show the filtered list only when a filter value is set — if no filter
is set, show a fixed record. |br|
**Expression:** ``filterUrl != []`` |br|
**Structure:**

* Filter rule Expression

  * Filter rule for filtering, e.g. multi-select or single select
  * Filter rule "Predefined set of items" with the desired record ID or IDs


Expression examples:
********************

* no filter parameter is set: ``filterUrl != []``
* filter parameter with URL parameter ``categories`` must contain a value: ``(filterUrl['categories'] ?? '') != ''``
* GET parameter ``foo`` must not be ``1``: ``(!request.query.has('foo') || request.query.get('foo') !== '1')``

Supported operators can be found in the
`Symfony documentation <https://symfony.com/doc/current/reference/formats/expression_language.html#supported-operators>`_.


.. |img_expression_01| image:: /_img/screenshots/cookbook/filter/expression_01.png
.. |img_expression_02| image:: /_img/screenshots/cookbook/filter/expression_02.png

.. |br| raw:: html

   <br />
