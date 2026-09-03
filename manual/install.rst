.. _manual_install:

Installing and Updating MetaModels
===================================

General Information about Installation
---------------------------------------

MetaModels consists of several modules that need to be installed depending on the task at hand.

In the Contao Manager under `Packages`, entering `metamodels/ <https://extensions.contao.org/?q=metamodels>`_
will list all available MetaModels packages. The base package `metamodels/core <https://extensions.contao.org/?p=metamodels%2Fcore>`_
must be installed — in addition, further `attributes and filters <https://extensions.contao.org/?q=metamodels>`_
are necessary depending on the task. A :ref:`checklist <rst_cookbook_checklists_mm-start>` is available
for getting started with MetaModels.

In addition to the individual packages, there are `bundles` that combine various packages for a
simplified installation.

For getting started with MetaModels, the bundle `metamodels/bundle_start <https://extensions.contao.org/?p=metamodels%2Fbundle_start>`_
is recommended — this installs the core as well as the most important attributes and filters.

There is also the bundle `metamodels/bundle_all <https://extensions.contao.org/?p=metamodels%2Fbundle_all>`_,
which installs the multilingual packages in addition to `bundle_start` (note: the packages `translatedselect`
and `translatedtags` are no longer included here since MM 2.1, as they should only be used for special cases).

The two bundles are only intended for evaluation or first steps — in a live system, only the core and the
necessary attributes and filters should be installed as separate packages.

Additional modules such as "register filter", "perimeter search", "rating" etc. must be added as separate
packages — see :ref:`extended_index`.

.. seealso:: If installing via the Contao Manager and packages are already installed that contain an old
   MultiColumnWizard (MCW) package, the manager (or Composer) cannot replace and install at the same time.
   As a workaround, first mark all existing extension packages for an update, then add the MM package(s)
   and apply; alternatively, run `composer update` on the console —
   see `'Forum' <https://community.contao.org/de/showthread.php?72871-MCW-MultiColumnWizard-als-Bundle-f%C3%BCr-Contao-4-(stable)&p=502709&viewfull=1#post502709>`_.

In addition to the Contao Manager, packages and bundles can be installed directly via the console using
Composer — for example with

``php public/contao-manager.phar.php composer require metamodels/core``

or

``php public/contao-manager.phar.php composer require metamodels/bundle_start``

Instead of `php`, the path to the appropriate PHP binary may need to be specified —
see :ref:`rst_cookbook_symfony_mm-2-1-tips`.

After installation, don't forget to **update the database** via the Contao install tool!

Further information about the individual versions of MetaModels follows.


Version Overview
----------------

* C 6.x + MM 3.0 + PHP 8.x - currently in planning...
* C 6.3 + MM 2.6 + PHP 8.4 - currently in development and testing with Contao 6.0
* :ref:`C 5.7 + MM 2.5 + PHP 8.4 <install_mm250>` - currently testing with Contao 5.7
* :ref:`C 5.3 + MM 2.4 + PHP 8.2 <install_mm240>` - access via "EAP"
* :ref:`C 4.13 + MM 2.3 + PHP 8.1 <install_mm230>`
* :ref:`C 4.9 + MM 2.2 + PHP 7.4 <install_mm-old>`
* :ref:`C 4.4 + MM 2.1 + PHP 7.2/7.4 <install_mm-old>`
* :ref:`C 3.5 + MM 2.0 + PHP 5.6 <install_mm-old>`

.. _install_mm250:
Installation of MM 2.5 for Contao 5.7 and PHP 8.4
---------------------------------------------------

MetaModels 2.5 brings full compatibility with Contao 5.7 and PHP 8.4. MM 2.5 is an adaptation of
version 2.4 to the new Contao and PHP version and of course brings
:ref:`all changes and features from MM 2.4 <new_in_mm240>`.

The installation requirements for MetaModels 2.5 are:

* a running Contao 5.7.x (LTS)
* PHP 8.4 or higher
* at least MySQL 5.7.6 or MariaDB 10.4.3
* ``memory_limit`` 512MB or more (recommended)
* until release, access key via the `EAP <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-5>`_
* for smaller projects, `Package "Basic 1" is available <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-5>`_

Higher versions of Contao and/or PHP may be possible but are not officially supported.

For an upgrade or new installation, note the :ref:`changes and new features of MM 2.5 <new_in_mm250>` as
well as the workflow with the :ref:`schema manager <component_schema-manager>` and XLIFF translations
:ref:`component_translations`.

.. toctree::
    :maxdepth: 1

    new-in-mm-25.rst


.. seealso::
   During the development phase, packages provided via git receive new filenames with every change.
   These are stored in the composer.lock. This can cause `composer install` to fail to find packages
   and display an error message. |br|
   In that case, please run `composer update` to update the composer.lock. |br|
   |br|
   Package dependencies are not entered as DEV version — this may mean that, for example, you need to
   manually add `attribute_numeric` for `attribute_timestamp` to the composer.json.
   Support is available for questions.

   If the update shows the message |br|
   ``The checksum verification of the file failed...`` |br|
   please delete the ``composer.lock`` and restart the update.

   If an update has problems, clearing the Composer cache may help: ``composer clearcache``.

   If you see the message |br|
   ``... Failed to connect to packages.cyberspectrum.de port 443: Connection refused...`` |br|
   or |br|
   ``... The "https://token:XXX@packages.cyberspectrum.de/r/packages.json" file could not be downloaded (HTTP/2 404 )...`` |br|
   then the Packagist server is very likely down and Composer cannot pull the packages. Please try the
   update again after a few minutes or contact the MM team.

   After an upgrade, please delete the session data for the user in the backend to avoid displaying
   "pseudo errors". To do this for all users, set the `session` column to `NULL` in the `tl_user` table.
   The error message looks like this: |br|
   ``Cannot assign null to property ContaoCommunityAlliance\DcGeneral\Panel\DefaultLimitElement::$intAmount of type int``

The site should be fully tested before going live. MM 2.5 can be installed via Composer (console) or
the Contao Manager. Access to the currently protected repository is available through our
"**early adopter program**" — more information under Fundraising on the
`MM website <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-5>`_.

**Further features of MM 2.5:** |br|
We have compiled an :ref:`overview page with the changes and features for MM 2.5 <new_in_mm250>` — please
note the :ref:`checklist <check_upgrade_mm240>` when upgrading.


.. _install_mm240:
Installation of MM 2.4 for Contao 5.3 and PHP 8.2
---------------------------------------------------

MetaModels 2.4 brings full compatibility with Contao 5.3 and PHP 8.2. MM 2.4 is an adaptation of
version 2.3 to the new Contao and PHP version and of course brings
:ref:`all changes and features from MM 2.3 <new_in_mm230>`.

The installation requirements for MetaModels 2.4 are:

* a running Contao 5.3.x (LTS)
* PHP 8.2 or higher
* MySQL 5.5.5 or higher (InnoDB), MariaDB (including "strict mode")
* ``memory_limit`` 512MB or more (recommended)
* until release, access key via the `EAP <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-4>`_
  — `MM Core <https://github.com/MetaModels/core/tree/release/2.4.0>`_ is already freely available
* for smaller projects, `Package "Basic 1" is available <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-4>`_

Higher versions of Contao and/or PHP may be possible but are not officially supported.

For an upgrade or new installation, note the :ref:`changes and new features of MM 2.4 <new_in_mm240>` as
well as the workflow with the :ref:`schema manager <component_schema-manager>` and XLIFF translations
:ref:`component_translations`.

.. toctree::
    :maxdepth: 1

    new-in-mm-24.rst

The site should be fully tested before going live. MM 2.4 can be installed via Composer (console) or
the Contao Manager. Access to the currently protected repository is available through our
"**early adopter program**" — more information under Fundraising on the
`MM website <https://now.metamodel.me/de/unterstuetzer/fundraising#metamodels_2-4>`_.

**Further features of MM 2.4:** |br|
We have compiled an :ref:`overview page with the changes and features for MM 2.4 <new_in_mm240>` — please
note the :ref:`checklist <check_upgrade_mm240>` when upgrading.


.. _install_mm-old:
Notes and Instructions for Older Contao and MM Versions
-------------------------------------------------------

* :ref:`Overview page with changes and features for MM 2.3 <new_in_mm230>`
* :ref:`Overview page with changes and features for MM 2.2 <new_in_mm220>`
* :ref:`cookbook_move_mm2.0_to_2.1`
* :ref:`cookbook_install_mm2.0-and-older`


Switching from `metamodels/bundle_*` to Separate Modules
---------------------------------------------------------

When switching, e.g. from 2.0 to a newer version or for a new installation, it is a good opportunity to
install only the attributes and filters that are necessary for the project. If, for example,
`metamodels/bundle_start` or `metamodels/bundle_all` was previously in use, you can use the following
SQL commands to query the actually used attributes and filters:

.. code-block:: sql
   :linenos:

   -- Attributes
   SELECT type FROM `tl_metamodel_attribute` GROUP BY type ORDER BY type
   -- Since MM 2.5, the attribute "levensthein" is called "levenshtein" (migration renames existing entries)

   -- Filters
   SELECT type FROM `tl_metamodel_filtersetting` GROUP BY type ORDER BY type
   -- Filter rules "conditionand, conditionor, customsql, idlist, simplelookup" are included in MM Core
   -- Filter rule "checkbox_published" is in the Checkbox attribute

The resulting list can then be installed via the Contao Manager or the console, and unused modules
can be left out.


Testing Special Packages
------------------------

In addition to the currently available and released MetaModels packages, there are sometimes packages
with bug fixes or new features that can/must be tested — for example, for the MetaModels core, this
could be a package ``hotfix/2.1.25``. The packages can be seen on Github in the corresponding repository
(e.g. MetaModels/core) under the
`'branches' <https://github.com/MetaModels/core/branches>`_ tab. The designation shown there, such as
``hotfix/2.1.25``, must be prefixed with ``dev-`` and suffixed with ``as 2.1.25``.

An overview of the entries in composer.json `here <https://devhints.io/composer>`_.

To test such a package, it must be explicitly specified in the Contao Manager with

``dev-hotfix/2.1.25 as 2.1.25``

or in composer.json

``"metamodels/core": "dev-hotfix/2.1.25 as 2.1.25"``

with its version.

Then run an update via the Contao Manager or on the console.

Since MetaModels is closely integrated with DC_General (DCG), testing often requires updating to a
newer version here as well. The procedure is the same as for MetaModels, including the adjustment of
the JSON entry with "as 2.1.x".

The composer.json for implementing the packages for Core and DCG should have approximately the
following entries in the "require" node (lines 8 and 10):

.. code-block:: json
   :linenos:

   {
       "name": "local/website",
       "description": "A local website project",
       "type": "project",
       "license": "proprietary",
       "require": {
           "contao-community-alliance/composer-client": "~0.12",
           "contao-community-alliance/dc-general": "dev-hotfix/2.1.42 as 2.1.42",
           "metamodels/bundle_all": "^2.1",
           "metamodels/core": "dev-hotfix/2.1.25 as 2.1.25",
           ...
       },
       ...
   }

To return to the original state, reset the packages to their original specification e.g. "^2.1" and
run an update including the database.

It is important to provide feedback to the developer or the MetaModels team via
`Github <https://github.com/MetaModels>`_ after a test.

Two further options are installing a fork or a pull request (PR). The composer.json must be adjusted
for installation.

For a fork (if necessary, enter your own Github oAuth token in the package manager settings), e.g.

.. code-block:: json
   :linenos:

   {
       "name": "local/website",
       "description": "A local website project",
       "type": "project",
       "license": "proprietary",
       "require": {
           "contao-community-alliance/composer-client": "~0.12",
           "contao-community-alliance/dc-general": "^2.1",
           "metamodels/bundle_all": "^2.1",
           "byteworks/metamodelsattribute_multi": ">=1.0.5.0,<1.1-dev",
           ...
       },
       ...
       "repositories": [
           ...
           {
               "type": "vcs",
               "url": "https://github.com/byteworks-ch/contao-metamodelsattribute_multi.git"
           },
           {
               "type": "git",
               "url": "git@gitlab.com:MetaModels/filter_parent.git"
           }
       ],
       ...
   }

or for a PR with the commit hash — which can be found on Github for the PR under the "Commits" tab.

.. code-block:: json
   :linenos:

   {
       "name": "local/website",
       "description": "A local website project",
       "type": "project",
       "license": "proprietary",
       "require": {
           "contao-community-alliance/composer-client": "~0.12",
           "contao-community-alliance/dc-general": "^2.1",
           "metamodels/bundle_all": "^2.1",
           "metamodels/attribute_alias": "dev-master#a97ec461ae1254fa616811c3ce234515238fb3c7 as 2.1.42",
           ...


.. |br| raw:: html

   <br />
