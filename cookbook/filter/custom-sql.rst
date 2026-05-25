.. _rst_cookbook_filter_custom-sql:

Custom SQL
==========

A detailed description of the "Custom SQL" filter rule can be found on the :ref:`filter rule detail page
<component_filter_customsql>` — quick information is also available via the |img_help| help in the settings.

A reminder: even with the "Custom SQL" filter rule, only IDs are passed to the next filter rule or filter
set. No "attribute values" can be added or computed, even though SQL would allow that via JOINs or
mathematical expressions.

The following SQL queries serve as "ingredients" for your own "SQL menu":


"LIKE" query with default value
********************************

"Search items where attribute 'name' matches GET parameter 'name',
or return all items (no filtering) if not set."

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM {{table}}
   WHERE `name` LIKE (CONCAT('%',{{param::get?name=name&default=%%}},'%'))


Filter by date
**************

"Search items where attribute 'date_start' is greater than or equal to
today's date — i.e. in the future."

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM {{table}}
   WHERE FROM_UNIXTIME(`date_start`) >= CURDATE()

or

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM {{table}}
   WHERE DATE(FROM_UNIXTIME(`date_start`)) >= DATE(now())


Filter by date (start or "ongoing")
*************************************

"Search items where attribute 'date_start' is greater than or equal to today's date
— i.e. in the future — or items where the current date is between 'date_start' and
'date_end' (ongoing)."

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM {{table}}
   WHERE
   ( DATE(FROM_UNIXTIME(`date_start`)) >= DATE(NOW()) )
   OR
   ( DATE(FROM_UNIXTIME(`date_start`)) <= DATE(NOW())
     AND
     DATE(FROM_UNIXTIME(`date_end`)) >= DATE(NOW())
   )


Filter by date (start/stop)
****************************

"Search items where attribute 'start' is greater than the current Unix timestamp
and attribute 'stop' has not been reached yet. Empty attribute values are treated
as irrelevant (then only 'start' or 'stop' applies)." [by "Cyberlussi"]

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM {{table}}
   WHERE (`date_start` IS NULL OR `date_start` = '' OR `date_start` < UNIX_TIMESTAMP())
   AND (`date_stop` IS NULL OR `date_stop` = '' OR `date_stop` > UNIX_TIMESTAMP())

Alternative

.. code-block:: sql
   :linenos:

   SELECT `id` FROM {{table}}
   WHERE (`date_start` IS NULL OR DATE(FROM_UNIXTIME(`date_start`)) <= DATE(now()))
   AND (`date_stop` IS NULL OR DATE(FROM_UNIXTIME(`date_stop`)) >= DATE(now()))


Filter by date (start) and publish date with GET check
*******************************************************

For example for events that should be hidden after their start date is reached,
but should only be shown from a certain date onwards — if set.

For testing, a GET parameter can be appended to the URL in the frontend — date format
is "YYYY-MM-DD", e.g. "domain.tld/my-list.html?now=2023-07-10".

.. code-block:: sql
   :linenos:

   SELECT id FROM {{table}}
   WHERE DATE(FROM_UNIXTIME(`date_start`)) >= DATE(now())
   AND (`date_published` IS NULL
   	OR DATE(FROM_UNIXTIME(`date_published`)) <= DATE(now())
   	OR DATE(FROM_UNIXTIME(`date_published`)) <= {{param::get?name=now}}
   )

Filter by date (start) last 12 months
***************************************
Archive filter for past items within the last 12 months:

.. code-block:: sql
   :linenos:

   SELECT id FROM {{table}}
   WHERE DATE(FROM_UNIXTIME(`date_start`)) < DATE(now())
   AND DATE(FROM_UNIXTIME(`date_start`)) >= DATE(now() - INTERVAL 12 month)

Filter by child elements of a parent element
*********************************************

"Find all child elements for a given parent element via the alias parameter —
e.g. to output all associated 'child elements' on a detail page."

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM mm_child
   WHERE `pid` = (
     SELECT `id`
     FROM mm_parent
     WHERE
     `parent_alias` = {{param::get?name=auto_item}}
     LIMIT 1
   )


Filter by parent element of a child element
********************************************

"Find the parent element for a given child element via the alias parameter —
e.g. to output the associated 'parent element' on a detail page."

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM mm_parent
   WHERE `id` = (
     SELECT `pid`
     FROM mm_child
     WHERE
     `child_alias` = {{param::get?name=auto_item}}
     LIMIT 1
   )

or shorter

.. code-block:: sql
   :linenos:

   SELECT `pid` as id
   FROM mm_child
   WHERE `child_alias` = {{param::get?name=auto_item}}


.. _rst_cookbook_filter_custom-sql_sortierung-der-ausgabe-nach-mehr-als-einem-attribut-fest:
Sorting output by more than one attribute (fixed)
**************************************************

"Sort 'teams' by points descending + games ascending + priority descending."
See also `Forum <https://community.contao.org/de/showthread.php?62625-Zweite-Sortierung>`_

Note that this SQL rule is placed as the *first rule* in the filter. In the first rule —
if it returns a list of items — the "base set" and the order of items is determined, and
subsequent rules can only reduce this set. The sort direction in MySQL is always ASC by
default — for a different direction, specify it for each sort column.

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM mm_teams
   ORDER BY `points` DESC, `games` ASC, `prio` DESC


Sorting output by a number and NULL values or random
*****************************************************

Note that this SQL rule is placed as the *first rule* in the filter.
Display items by a custom sort number, with all items without a number (NULL) at the end:

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM mm_sv_categories
   ORDER BY ISNULL(`sort_number`), `sort_number` ASC

You can also display certain items first (attribute "prio_slider" = 1) and the
rest in random order:

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM mm_sv_trainings
   ORDER BY `prio_slider` DESC, rand()


Sorting output by referenced MetaModel and name
************************************************

For example, if you have a MetaModel for products where each product references a partner
via single select [select], and you want to output the products sorted first by the
partner's manual sorting (sorting) and then by the actual product name, this can be
achieved with the following code:

.. code-block:: sql
   :linenos:

   SELECT pro.id FROM mm_products AS pro
   LEFT JOIN mm_partners AS part ON pro.partner = part.id
   WHERE pro.published = 1
   ORDER BY part.sorting, pro.product_code

In the output list, this could be used to output an intermediate heading for each new
partner. Store the current partner ID in a temporary variable and check for equality on
each loop pass — if not equal, output "partner name".


Dynamic default value
**********************

In custom SQL, default values are possible via 'default=<value>', which are used when
the filter parameter is not set. Currently, nesting of insert tags or use of MySQL
functions is not possible within the param tag, so dynamic defaults require a workaround
using "SQL IF".
See also `GitHub #880 <https://github.com/MetaModels/core/issues/880>`_

.. code-block:: sql
   :linenos:

   SELECT `id` FROM mm_months
   WHERE FROM_UNIXTIME(`from_date`) <= IF(
      {{param::get?name=von_datum}},
      {{param::get?name=von_datum}},
      CURDATE()
   )
   ORDER BY `from_date` DESC

Default value ''
*****************

In custom SQL, default values are possible via 'default=<value>', which are used when
the filter parameter is not set. Currently, entering `''` or `""` in the param tag is
cast in a way that causes incorrect filtering; this applies for example to checkbox values.

.. code-block:: sql
   :linenos:

   SELECT `id` FROM mm_employees
   WHERE `driver_licence` = IF(
      {{param::get?name=driver_licence}},
      {{param::get?name=driver_licence}},
      ''
   )

Passing multiple values for `IN()`
***********************************

Multiple values can be passed to the query as a comma-separated list or array — depending
on the type of the input, the `aggregate` parameter uses the value `list` or `set`.

.. code-block:: sql
   :linenos:

   -- as list
   -- domain.tld/en/list?id=13,15,19
   SELECT id FROM {{table}}
   WHERE id IN ({{param::get?name=id&aggregate=list}})

   -- as array
   -- domain.tld/en/list?id[]=13&id[]=15&id[]=19
   SELECT id FROM {{table}}
   WHERE id IN ({{param::get?name=id&aggregate=set}})

Filter tags for an item
************************

Employees have a multi-select [tags] attribute linked to the MetaModel "Softskills".
For the detail view of an employee, these should be retrieved — the detail view is
filtered via the "auto_item" using the alias.

The softskills are displayed as a separate list on the detail page, but must be filtered
accordingly. To retrieve the data, the relation table "tl_metamodel_tag_relation" must
be used. Important: determine the attribute ID for "rel.att_id", i.e. in the "Employees"
attributes, the multi-select has e.g. ID 5 (find it via the info button).

.. code-block:: sql
   :linenos:

   SELECT DISTINCT(rel.value_id) as id FROM mm_employees as ma
   LEFT JOIN tl_metamodel_tag_relation rel ON (ma.id = rel.item_id AND rel.att_id=5)
   WHERE
   ma.alias = {{param::get?name=auto_item}}

Filter items by single select property
***************************************

Employees have a single select [select] attribute linked to the MetaModel "Department".
For a list view of employees, only those working in a department with a "score" greater
than 99 should be shown.

.. code-block:: sql
   :linenos:

   SELECT `id` FROM mm_employees
   WHERE `department` IN (
      SELECT `id` FROM mm_departments
      WHERE `score` > 99
   )

or

.. code-block:: sql
   :linenos:

   SELECT ma.id FROM mm_employees ma
   LEFT JOIN mm_departments rel ON (ma.department = rel.id)
   WHERE rel.score > 99


Filter employees for a page assigned via multi-select [tags]
*************************************************************

Employees have a multi-select attribute linked to the `tl_page` table, to represent
an employee as responsible on specific pages. On those pages, an MM list element can be
inserted that outputs the associated employees. The following query can be used for
filtering:

.. code-block:: sql
   :linenos:

   SELECT ma.id FROM mm_employees ma
   LEFT JOIN tl_metamodel_tag_relation rel ON (ma.id = rel.item_id)
   WHERE
   rel.att_id = 79 AND             -- 79 ID of the [tags] attribute
   rel.value_id = {{page::id}} AND -- variable page ID
   ma.published = 1
   ORDER BY ma.name


Filtering a select in the BE for a non-MM table
***********************************************

If the single select [select] attribute is configured with a table that is not an MM
table, a "WHERE restriction" input is available as filtering option. For example, if
you want a connection to the member table "tl_member" but restrict it so that each
member can only be selected once, use the following:

.. code-block:: sql
   :linenos:

   (SELECT tl_member.id FROM tl_member
    LEFT JOIN mm_member
           ON mm_member.memberId=tl_member.id
      WHERE
            mm_member.memberId IS NULL
      AND
            tl_member.id=sourceTable.id)


Extracting an ID from a GET parameter after '::'
*************************************************

For filtering in the backend or for frontend editing, you may need access to the ID
from the GET parameter in the URL. However, it is coupled with a table name via '::'
and must be separated for use in a custom SQL query. This can be done using
`SUBSTRING_INDEX` in the query, as shown in the following example:

.. code-block:: sql
   :linenos:

   -- URL: ....&id=mm_employees::51&...
   SELECT * FROM mm_employees
   WHERE `id` = SUBSTRING_INDEX({{param::get?name=id}},'::',-1)


Filter for select/tags in the input mask
*****************************************

The single and multi-select attributes (Select and Tags) can be given a filter for
the input mask. If this filter should dynamically respond to another attribute in the
input mask, the "Custom SQL" filter rule can be used with dynamic parameters.

As a dynamic parameter, for example the URL with GET parameters, or the POST parameters
from a `submitonchange` of an attribute in the input mask, can be evaluated. With GET
you start with the record ID, with POST you start with the value(s) of the triggering
attribute.

For example, when the department is selected, the list of selectable employees should
be restricted to those who belong to the same department. The POST parameter of the
department is "listened to", and then the employee list can be narrowed down via
QUERY-P (POST) or QUERY-G (GET).

.. code-block:: sql
   :linenos:

   SELECT `id` FROM mm_employees
   WHERE IF (
         {{param::post?name=department}} != 'NULL', (QUERY-P), (QUERY-G)
    )

You can also make two select dropdowns dependent on each other. If you have a table for
categories ``mm_market_category`` and a child table with subcategories
``mm_market_subcategory``, as well as a table ``mm_market_machine`` where both tables are
included as single selects: when a category is selected in the machine input mask, only
the associated subcategory elements should appear. The following SQL filter rule should
be added in the subcategory model:

.. code-block:: sql
   :linenos:

   SELECT subcategory.id FROM mm_market_subcategory AS subcategory
   WHERE IF (
       {{param::post?name=category}} != 'NULL',
       subcategory.pid = (
           SELECT category.id FROM mm_market_category AS category
           WHERE category.alias = {{param::post?name=category}}
           LIMIT 1
       ),
       subcategory.pid = (
           SELECT machine.category
           FROM mm_market_machine AS machine
           WHERE machine.id = SUBSTRING_INDEX({{param::get?name=id}},'::',-1)
           LIMIT 1
       )
   )

For restricting a multi-select, some trickery is needed, as the IF condition in
subqueries does not allow multiple return values. However, it is possible to use
GROUP_CONCAT to create a single string of IDs that can be evaluated by IN.

For example, the possible selections for attribute "travel modules" should be
restricted to the selection of attribute "travel destinations". The following template
is intended as inspiration — there may be more elegant solutions.

.. code-block:: sql
   :linenos:

   SELECT rb.id FROM mm_travel_modules AS rb
   WHERE rb.region IN (
       SELECT IF(
           {{param::post?name=travel_destinations}} != 'NULL',
           (SELECT GROUP_CONCAT(rz.id) FROM mm_travel_destinations AS rz
               WHERE rz.alias IN ({{param::post?name=travel_destinations}}) GROUP BY rz.pid),
           (SELECT GROUP_CONCAT(rel.value_id) AS id FROM tl_metamodel_tag_relation AS rel
               WHERE rel.att_id = '42'
               AND rel.item_id = SUBSTRING_INDEX({{param::get?name=id}},'::',-1) GROUP BY rel.att_id)
       ) as id
   )

Filter for multi-select in the input mask: only unselected items
*****************************************************************

For example, if you have a table for regions with a multi-select attribute linked to
countries, and you want to restrict the selection to countries not yet assigned, you
can activate a filter on the multi-select attribute (ID: 42) for countries. In the
filter, a "Custom SQL" filter rule can be set up as follows:

.. code-block:: sql
   :linenos:

   SELECT `id`
   FROM mm_countries
   WHERE `id` NOT IN (
       SELECT `value_id` as id
       FROM tl_metamodel_tag_relation
       WHERE `att_id` = '42'
   ) OR id IN (
       SELECT `value_id` as id
       FROM tl_metamodel_tag_relation
       WHERE `att_id` = '42'
       AND `item_id` = SUBSTRING_INDEX({{param::get?name=id}},'::',-1)
   )

Distinguishing between frontend and backend
*******************************************

.. note:: In MM 2.3, a selection was added to the filter rule to specify the execution environment,
   e.g. "Backend only". This allows the SQL queries to be simplified.

When filtering with custom SQL, it may be necessary to distinguish between frontend and
backend. Since MM 2.2, filters configured for Select and Tags attributes are also applied
in the frontend, which can cause issues with filter rules intended only for the input mask.

You can query the current request string and search for your model name, e.g.
"mm_employees", as the first word.

.. code-block:: sql
   :linenos:

   SELECT artd.id FROM mm_article_details artd
   LEFT JOIN tl_metamodel_tag_relation rel ON (artd.id = rel.item_id)
   WHERE
   IF (SUBSTRING_INDEX(SUBSTRING_INDEX('{{env::request}}', '/', -1), '?', 1) = 'mm_employees',
      rel.att_id = 43 AND                                             -- 43 ID of the [tags] attribute
      rel.value_id = SUBSTRING_INDEX({{param::get?name=id}},'::',-1), -- variable ID from URL for article/product
      1=1
   )

.. note:: With MM version 2.3-beta1, the routing in the BE changed — instead of
   ``domain.de/contao?do=metamodel_mm_employees&act=edit...`` it is now
   ``domain.de/contao/metamodel/mm_employees?act=edit``, meaning the query
   ``SUBSTRING_INDEX(SUBSTRING_INDEX('{{env::request}}', '/', -1), '?', 1)``
   previously returned the value "contao".


Comments in SQL queries
***********************

SQL queries can sometimes become quite complex and contain fixed values such as
attribute IDs etc. To keep track later or when working in a team, comments can be
added here as well — more at the `MySQL reference manual <https://dev.mysql.com/doc/refman/5.6/en/comments.html>`_.

Example:
|img_sql-comment|


.. |img_about| image:: /_img/icons/about.png
.. |img_help| image:: /_img/icons/help.svg
.. |img_sql-comment| image:: /_img/screenshots/cookbook/filter/sql-comment.jpg
