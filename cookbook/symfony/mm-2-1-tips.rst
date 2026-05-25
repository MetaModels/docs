.. _rst_cookbook_symfony_mm-2-1-tips:

Symfony and MM 2.x Tips
========================

A few tips for working with MM 2.x under Symfony — as a starting point or
as a quick reference.

All console commands are run from the Contao installation directory — i.e.
where the composer.json file is located. Before all calls, first switch to that
directory:

``cd /var/www/my-contao``

Then make sure that the same PHP version is running on the console as for the
website (see the Contao Manager under Tools > PHPINFO). You can check the
console version with:

``php -v``

If the PHP version is not the same, you must call the commands with a path to
the PHP binary. The path is shown e.g. during the Contao Manager system check
or in the provider's documentation/wiki.

``/usr/bin/php82 -v``


Composer Update
---------------

The following command initiates an update:

``/usr/bin/php82 web/contao-manager.phar.php composer update -v``

or with memory and execution time allocation:

``/usr/bin/php82 -d memory_limit=-1 -d max_execution_time=900 public/contao-manager.phar.php composer update -v``

or for older installations using the ``web`` path:

``/usr/bin/php82 -d memory_limit=-1 -d max_execution_time=900 web/contao-manager.phar.php composer update -v``

The ``-v``, ``-vv``, or ``-vvv`` parameters provide different levels of output
detail. The additional ``--dry-run`` parameter performs a dry run as a test.

After an update, run the install tool if necessary so that database changes are
applied (often forgotten :D).

The composer.phar should be updated regularly — run the following command to
do so:

``/usr/bin/php82 web/contao-manager.phar.php self-update``

The database migration can be triggered as follows — :ref:`see schema manager <component_schema-manager>`:

``/usr/bin/php82 vendor/bin/contao-console contao:migrate``

Determining Package Versions
-----------------------------

When reporting errors or asking developers for support, the installed version
of an extension is important. This can be determined via the package name,
e.g. for DC_General:

``/usr/bin/php82 public/contao-manager.phar.php composer show | grep dc-general``

With:

``/usr/bin/php82 public/contao-manager.phar.php composer show``

all packages are listed.

For a development version obtained e.g. from the "EAP", there is no version
number yet — but you can find the current commit number in ``composer.lock``.
Search the file e.g. for ``"name": "metamodels/core"``. The commit number is in
the ``reference`` node — the first eight characters are sufficient, e.g.
``8da81418``.


Clearing the Cache
-------------------

Clear the Contao cache after making changes:

"Soft" (recommended):

``/usr/bin/php82 vendor/bin/contao-console cache:clear --env=prod`` |br|
``/usr/bin/php82 vendor/bin/contao-console cache:warmup``

or the "hard way":

``rm -rf var/cache/{dev,prod}``

which deletes "everything" from dev and prod.


.. _rst_cookbook_symfony_mm-2-1-tips_toolbar:

Symfony Toolbar
---------------

The Symfony toolbar makes it easier to display template values and debug during
the development of a project with MetaModels.

Debug mode can be activated from the backend or Contao Manager, or permanently
via an entry in the environment file ``.env`` or ``.env.local`` with the entry:

``APP_ENV=dev``

Permanent activation should only be done locally or on otherwise protected pages.

In debug mode, Contao caching is also disabled so you don't need to clear the
cache as often — but this also means the page may "look different". Additionally,
the Symfony debug toolbar is shown in the browser.

|img_symfony-toolbar|

If something in the source code needs to be debugged, use Symfony's ``debug()``
function — the output then appears in the debug toolbar and can be viewed via
the "crosshair icon".

For debugging templates, there is a description here: :ref:`rst_cookbook_debug_templates`.

To inspect the SQL calls e.g. of a "Custom SQL" filter rule, go to "Doctrine"
in the debug toolbar — all SQL calls are listed there. Using the browser's
search and some identifiable parts such as the table name, the query can usually
be found quickly. The toolbar provides the SQL code in various formats, making
it easy to copy the query into phpMyAdmin for testing.


Disabling Warnings in Debug Mode
----------------------------------

When viewing debug output, a warning message may sometimes prevent the display
from appearing. The warning may come from a theme or another extension and have
nothing to do with MetaModels. To still get your desired output in the Symfony
toolbar, you can suppress warnings. Add the following entry to ``config.yml``:

.. code-block:: php
   :linenos:

    // config/config.yml
    framework:
      profiler:
        only_exceptions: true
    # or
    contao:
        error_level: 8181


.. |img_symfony-toolbar| image:: /_img/screenshots/cookbook/debug/symfony-toolbar.jpg

.. |br| raw:: html

   <br />
