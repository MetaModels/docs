.. _rst_extended_health-check:

MetaModels Health-Check
========================

Finds and cleans up inconsistent MetaModels data in the backend - e.g. rows left behind in the
attribute-specific storage tables (tags, multi-/table text, ratings and their translated
variants) after an attribute or a record has been deleted, because the DCG never gets to see
these additional tables - a risk that increases with complex data structures and is not
completely prevented unless enforced by foreign keys at the database level.


Prerequisites
--------------

* as of MetaModels 2.5
* own Composer package, no dependency on other extensions apart from MetaModels itself


Installation via Contao Manager or Composer
---------------------------------------------

.. code-block:: bash

   composer require metamodels/health-check


Access
------

After installation, an additional menu item "Health-Check" appears at the top right of the "All
MetaModels" list, next to "Edit multiple".

|img_health-button|

Like the entire MetaModel administration, the **Health-Check is only accessible to admins** -
enforced server-side, not only hidden in the navigation.


The View
--------

|img_health-overview|

**Checks:** Each check appears as its own row with a checkbox on the left, name and description,
the result of the last run, the time of the last check as well as its own buttons. The checkbox
at the top left of the header row can be used to select or deselect all checks at once; "Run
checks" then only runs the checked checks. An individual check can independently be triggered at
any time via its own "Check" button. The page does not automatically run any check when opened -
the result of the last actual run is displayed (or "Not yet checked").

.. warning:: Above the check list is a notice to carefully review the data before a cleanup and
   to create a backup beforehand - a cleanup cannot be undone.

The following checks are included:

* Orphaned rows in the attribute-specific storage tables (multi-table values, table text, tags
  assignments, ratings, translated URL values as well as their translated variants) - rows whose
  attribute or associated record no longer exists.
* Broken parent-child references: child records (:ref:`child tables
  <component_relations_child-tables>`) whose parent record no longer exists.
* Orphaned file references: attributes of type "File" or "Translated file" whose stored reference
  no longer points to a file in the file manager - comparable to a file that was deleted directly
  in the file system instead of via Contao. This check is **purely informational and is not fixed
  automatically**, since a correction here would have to specifically rewrite a (sometimes
  serialized) value instead of just deleting a row - the risk of an automatic cleanup is too high
  for that.

**Details:** Every check that actually inspects records behind the scenes (just as much for the
purely informational file references check as for the fixable ones) has its own "Details" button
that shows the affected records in a table - even before deciding on a cleanup. For the generic
"orphaned rows" checks (and the parent-child check), all columns of the respective table are
automatically determined from the database schema for this - this works generically for any
connected table, including custom checks; long or binary values (e.g. a blob column) are
summarized as "(N bytes)" instead of being output raw. For the file references check, the table
is curated instead: MM table, attribute, record ID, language (for "Translated file") and count
(how many orphaned references this one record contributes - the column adds up to the check's
total count, even if a record with several orphaned files in a multi-field produces fewer table
rows than total hits). The table in the popup shows a maximum of 50 rows with a "... and N more"
notice for more hits; on the console (see below) this limit does not apply.

For the fixable checks there are two further buttons: **"Fix preview"** shows how many rows would
be removed, without deleting anything; **"Fix run"** actually deletes. Both open a popup for this
with their own "Start" button - this deliberate intermediate step replaces an additional "Are you
sure?" confirmation - and additionally show the same detail table as above alongside the count;
for "Fix run" it is captured before deletion, so that it still shows what was removed afterwards.
"Fix preview"/"Fix run"/"Details" only become usable once "Check" has actually found something -
before that they are greyed out. Every cleanup that is actually run is recorded in the cleanup log
at the bottom of the page (date, check, number of rows removed, executing user).

A check only appears in the list if it can apply to the current installation at all - e.g. the
check for multi-table values only appears if ``metamodels/attribute_tablemulti`` is installed.
This way, "no problems found" is never falsely shown for a table that does not even exist in your
own installation.

Below the description of each check is also its console id (see below,
:ref:`rst_extended_health-check_console`) - the value passed to ``metamodels:health:run``.

**Backup:** "Create backup now" can be used to trigger a database backup directly from the page -
via the same mechanism that Contao itself uses (System > Maintenance > Backup). Restoring a
backup deliberately does **not** happen on this page, but as usual via the Contao Manager or the
console command ``contao:backup:restore``.


.. _rst_extended_health-check_console:

Console Commands
------------------

Every check can also be invoked via the console - e.g. for cron jobs or CI.

.. code-block:: bash

   # List available checks with their id, last check time and last result
   php bin/console metamodels:health:list

   # Run a single check by its id (see "metamodels:health:list" for the id)
   php bin/console metamodels:health:run orphaned_tag_relation

   # Run all checks at once
   php bin/console metamodels:health:run --all

   # Also list every affected record - unlike the backend popup, without any
   # limit, so it can also be redirected to a file, for example
   php bin/console metamodels:health:run orphaned_tag_relation --details

   # Preview: how many rows would a cleanup remove?
   php bin/console metamodels:health:repair orphaned_tag_relation --dry-run

   # Actually clean up
   php bin/console metamodels:health:repair orphaned_tag_relation --force

``metamodels:health:list`` and ``metamodels:health:run`` are purely read-only - but every run is
recorded in the "Last check" field just like a run via the backend. ``metamodels:health:run``
exits with a non-zero exit code as soon as at least one check has found problems - so the command
can be directly integrated as a monitoring check.

``metamodels:health:repair`` is the console equivalent of "Fix preview (dry run)" and "Fix now":
exactly one of the two options ``--dry-run``/``--force`` is required, there is deliberately no
silent default case. A cleanup actually run via ``--force`` ends up in the same cleanup log as one
run via the backend - the user shown there is "-", since there is no logged-in backend user here
(e.g. for a cron job). There is deliberately no ``--all`` here: a cleanup should always be a
deliberate decision per check.


Implementing Custom Checks
----------------------------

The checks are built modularly - custom checks can be added without modifying this package
itself. **There is no EventListener for this**, but a regular Symfony service registered via a DI
tag - exactly how, for example, Contao's own migrations
(``Contao\CoreBundle\Migration\MigrationInterface``) work.

A check implements ``MetaModels\HealthCheckBundle\HealthCheck\HealthCheckInterface``:

.. code-block:: php

   interface HealthCheckInterface
   {
       public function getId(): string;
       public function getLabel(): string;
       public function getDescription(): string;
       public function check(): HealthCheckResult;
   }

``check()`` is always purely read-only and returns a ``HealthCheckResult`` with a list of
``HealthCheckIssue`` (each with a description + number of affected rows). If the check should
also be able to clean up itself, additionally implement
``MetaModels\HealthCheckBundle\HealthCheck\RepairableHealthCheckInterface``:

.. code-block:: php

   interface RepairableHealthCheckInterface extends HealthCheckInterface
   {
       public function repair(bool $dryRun): HealthCheckRepairResult;
   }

``repair()`` determines the affected rows freshly on every call (not from a possibly outdated
list from a previous ``check()``) and only deletes them if ``$dryRun`` is false.

The custom check is registered in your own ``services.yml`` with the tag
``metamodels_health_check.check``:

.. code-block:: yaml

   services:
     App\HealthCheck\MyCustomCheck:
       arguments:
         - '@database_connection'
         - '@translator'
       tags: ['metamodels_health_check.check']

This makes the custom check automatically appear in the list on the Health-Check page - without
any change to ``metamodels/health-check`` itself.

The order in the list (backend as well as ``metamodels:health:list``) follows the ``priority`` of
the tag - a regular Symfony DI feature, not a custom development of this package. Higher priority
appears further up, the default is 0, and for equal priority the registration order decides. The
included checks use the values 100 to 10 (in steps of ten, from "Orphaned multi-table values" to
"Orphaned file references"); a custom check without a value therefore automatically ends up after
them:

.. code-block:: yaml

   services:
     App\HealthCheck\MyCustomCheck:
       arguments:
         - '@database_connection'
         - '@translator'
       tags:
         - { name: 'metamodels_health_check.check', priority: 50 }


.. |img_health-button| image:: /_img/screenshots/extended/health-check/health-button.png
.. |img_health-overview| image:: /_img/screenshots/extended/health-check/health-overview.png
