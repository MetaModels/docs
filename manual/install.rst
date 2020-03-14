.. _manual_install:

Install and update MetaModels
==============================

The current MetaModels 2.1 is extensively tested and approved for LTS 4.4.

**MetaModels 2.2 for Contao 4.9** (new LTS) is in progress - but can be installed immediately.
For more information see `Installing MM 2.2 <#installation-of-mm-2-2>`_

The installation of MM 2.1 requires PHP version 7.1 or higher - PHP 7.2 is recommended.

MetaModels 2.1 can be installed via the Contao-Manager or via the console via Composer.
see the following section.

.. seealso:: For a re-financing of the extensive work, the MM team asks for financial
   Gift. The scope of the project to be realised should be taken as a guideline.
   and about 10% will be taken into account - based on the experience of the last grants, are
   the amounts between 100€ and 500€ (net) - an invoice incl. VAT will of course always be
   is on display. `More... <https://now.metamodel.me/de/unterstuetzer/spenden>`_

Installation of MM 2.2
-----------------------

MetaModels 2.2 brings full compatibility with Contao 4.9 and several features and
Optimizations. For example, MM 2.2 is compatible with the `strict mode` of higher
MySQL versions or current MariaDB or manual file sorting.

The installation requirements for MetaModels 2.2 are

* a running Contao 4.9.x (LTS) and
* PHP 7.2/7.3
* MySQL from 5.5.5 (InnoDB), MariaDB

**MetaModels 2.2 is now ready for use** and can be started via the Composer (console)
or the Contao Manager can be installed. get access to the currently protected repository
about our "eary adopter program" - more about this under Fundrasing on the
`MM website <https://now.metamodel.me/en/supporters/fundraising#metamodels_2-2>`_.

Further features of MM 2.2 (will be added continuously):

* compatible with the `strict mode` of MySQL and MariaDB
* manual file sorting (including translated files)

Installing MM 2.1
-----------------

The installation requirements for MetaModels 2.1 are:

* a running Contao 4.4.x (LTS) and
* PHP 7.1/7.2
* MySQL from 5.5.5 (InnoDB), MariaDB (without `strict mode`)

Higher versions of Contao and/or PHP are possible, but not officially supported.

In the Contao manager you can enter `metamodels/` to get all available packages
are listed. The basic package `metamodels/core` has to be installed - and in addition 
additional attributes and filters can be added depending on the task.

In addition to the individual packages, there are `bundles` which contain different packages for a
simplified installation.

For an introduction to MetaModels we recommend the bundle `metamodels/bundle_start` - herewith
the core as well as the most important attributes and filters are defined without e.g. the packages for multilingualism
installed.

As in MetaModesls 2.0, there is also the `metamodels/bundle_all` bundle, which is installed next to the
`bundle_start` will also install the multilanguage packages (note: the `translatedselect` packages are also installed).
`translated tags` are no longer included here, as they are only to be used for special cases).

Further modules like "Register filter", "Radius search", "Rating" etc. are available as separate packages
to add.

In addition to the Contao Manager, the installation of packages and bundles can be done directly via the console via
Composer possible - e.g. with

``php web/contao-manager.phar.php composer require metamodels/core``

or

``php web/contao-manager.phar.php composer require metamodels/bundle_start``

Instead of `php` you may have to specify the path to the corresponding PHP binary.

After the installation a **update of the database is not possible via the install tool of Contao.
to forget!**

With a conversion (2.0 -> 2.1) or a new installation it is a good opportunity to only use the attributes and filters
which are necessary for the project. Was previously e.g. `metamodels/bundle_all` in use,
you can query the really used attributes and filters with the following SQL commands:

.. code-block:: sql
   :linenos:
   
   -- Attribute
   SELECT type FROM `tl_metamodel_attribute` GROUP BY type ORDER BY type
   
   -- Filter
   SELECT type FROM `tl_metamodel_filtersetting` GROUP BY type ORDER BY type


Testing of special packages via Composer
----------------------------------------

The bundle 'bundle_all' contains all currently available and released MetaModels packages. Additionally there are packages with bugfixes or brandnew functions that have to be tested. For the MetaModels core this could be e.g. a package called "dev-hotfix-xyz". You can see those packages inter alia on Github within the corresponding repository (e.g. MetaModels/core) in the
`'branches' tab <https://github.com/MetaModels/core/branches>`_.

In case that you want to test a package like this, you'll have to separately select and install it in the package management.
For the selection in the package management, check the checkbox "dependencies i nstalled" and then click on the corresponding package, e.g. 'metamodels/core' and aditionally in the following options click on e.g. 'dev-hotfix-xyz'.

After "Reserve package for installation" you'll have to make some small changes to Composer-JSON. To do this go to the package manager to "settings" and there click onto "expert mode". The displayed JSON file has to be extended with the entry "as 2.0.0" within the node "require". If you happen to have several extra packages you have to do this for every entry.

for example: |br|
``"metamodels/core": "^2.1"`` modify to |br|
``"metamodels/core": "dev-hotfix/2.1.25 as 2.1.25"``

After the installation via "update packages" you should delete the Composer cache in the "settings" of the package management.

As MetaModels is closely interlinked with the DC_general (DCG), you will frequently need to update to a newer version here as well for testing.
The procedure is the same as for MetaModels including the adjustment of the JSON entry with the "as 2.0.0".

To come back to the initial version , just delete the package in the package management.

Please never forget to provide the MetaModels developer team with your valuable feedback after your test on  
`Github <https://github.com/MetaModels>`_. 


.. |br| raw:: html

   <br />