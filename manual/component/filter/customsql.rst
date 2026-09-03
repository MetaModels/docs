.. _component_filter_customsql:

|svg_filt_customsql_22| |img_filter_customsql| Custom SQL
=========================================================

The "Custom SQL" filter rule enables the use of a custom SQL query to filter items.
The query must return a list of item IDs. This filter rule is aimed at advanced users
who need complex filter conditions that cannot be mapped with the available filter rule
types — e.g. comparisons across multiple columns, subqueries, or date-based calculations.

This filter rule has no frontend widget output.

Column names should always be enclosed in backticks ` (e.g. \`name\`) or prefixed with
the table name or its alias (see `MySQL Identifiers <https://dev.mysql.com/doc/refman/8.0/en/identifiers.html>`_).
This also allows the use of (My)SQL `reserved words <https://dev.mysql.com/doc/refman/8.0/en/keywords.html>`_.

For more complex queries, it is advisable to test them with SQL tools such as phpMyAdmin,
PHPStorm, etc. before integrating, or to build nested queries step by step using fixed
values first. The corresponding data should of course also be present as items in the DB.
As a final step, add any necessary dynamic parameters using the available insert tags.
The MM-SQL insert tags are only resolved within the query processing and are therefore
not generally available in the frontend.

Even with the "Custom SQL" filter rule, only IDs are passed to the next filter rule or
filter set. No "attribute values" can be added or calculated, even if that would be
possible via SQL e.g. through JOINs or mathematical operations.

.. seealso:: Practical examples and usage notes can be found in the cookbook:
   :ref:`rst_cookbook_filter_custom-sql` |br|
   General SQL tips: :ref:`rst_cookbook_sql-tips`


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
     - Selection of the filter rule type — here: "Custom SQL".
   * - Enabled
     - Enables or disables this filter rule.
   * - Comment
     - Free text field for describing the purpose of this filter rule.
   * - Custom SQL query
     - Input of the SQL query. The query must return at least one column ``id``.
       Column names should be prefixed with the table alias (e.g. ``t.id``). Insert
       tags and parameter sources are supported. |br|
       Default template: ``SELECT id FROM {{table}} WHERE 1 = 1``
   * - Use only in environment
     - Optional restriction defining in which Contao environment (e.g. backend or
       frontend) the filter rule should be executed.


Matching Attributes
------------------

The "Custom SQL" filter rule is not attribute-bound. The filter logic is fully defined
in the SQL query. The ``{{table}}`` placeholder is used to insert the MetaModel's
table name.


Special Functions
----------------

Placeholder ``{{table}}``
~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``{{table}}`` placeholder is replaced at runtime with the actual table name of
the MetaModel, e.g. ``mm_mymodel``.

.. code-block:: sql

   SELECT t.id FROM {{table}} AS t WHERE t.page_id = 1

is equivalent to:

.. code-block:: sql

   SELECT t.id FROM mm_mymetamodel AS t WHERE t.page_id = 1


Insert Tags
~~~~~~~~~~~

Contao insert tags can be used in the SQL query. Note that not all tags are available
in all contexts. A tag like ``{{page::id}}`` for example only works with frontend page
requests, not with RSS feeds.

Examples:

* ``{{user::id}}`` — ID of the logged-in frontend member
* ``{{page::id}}`` — ID of the current page
* ``{{env::request}}`` — Current request URI


Secure Insert Tags
~~~~~~~~~~~~~~~~~~~

Secure insert tags work like normal insert tags, but the values are automatically
escaped in the query. Careless use can therefore lead to unexpected results.

Notation:

.. code-block:: text

   {{secure::page::id}}


Parameter Sources ``{{param::...}}``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Parameter sources allow access to various external values directly in the SQL query.
The pattern is:

.. code-block:: text

   {{param::[source]?[querystring]}}

**Available sources:**

.. list-table::
   :header-rows: 1
   :widths: 20 80

   * - Source
     - Description
   * - ``get``
     - HTTP GET query string
   * - ``post``
     - HTTP POST fields
   * - ``session``
     - Any field from the Contao session
   * - ``filter``
     - Executed filter parameter (for sharing filter values between filter rules)

**Optional keywords in the querystring:**

.. list-table::
   :header-rows: 1
   :widths: 20 80

   * - Keyword
     - Description
   * - ``name``
     - Name of the parameter (required)
   * - ``default``
     - Default value if no other value is available
   * - ``aggregate``
     - ``list`` or ``set`` — for array values
   * - ``key``
     - Set to ``1`` to read the key of an array (requires ``aggregate``)
   * - ``recursive``
     - Set to ``1`` to read arrays recursively (requires ``aggregate``)


Examples
---------

**Example 1 — Simple query**

.. code-block:: sql

   SELECT t.id FROM mm_mymetamodel AS t WHERE t.page_id = 1

Selects all IDs from the table ``mm_mymetamodel`` where ``page_id = 1``.

**Example 2 — Insert table name**

.. code-block:: sql

   SELECT t.id FROM {{table}} AS t WHERE t.page_id = 1

Like example 1, but the table name of the current MetaModel is inserted automatically.

**Example 3 — GET parameter and default value**

.. code-block:: sql

   SELECT t.id
   FROM {{table}} AS t
   WHERE t.catname = {{param::get?name=category&default=defaultcat}}

For the URL ``https://example.org/list/category/demo.html`` this yields: |br|
``SELECT t.id FROM mm_demo AS t WHERE t.catname = 'demo'``

For the URL ``https://example.org/list.html`` (no parameter): |br|
``SELECT t.id FROM mm_demo AS t WHERE t.catname = 'defaultcat'``


.. |svg_filt_customsql_22| image:: /_img/icons_svg/filter_customsql.svg
   :width: 22px
.. |img_filter_customsql| image:: /_img/icons/filter_customsql.png

.. |br| raw:: html

   <br />
