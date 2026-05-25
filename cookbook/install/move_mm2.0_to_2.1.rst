.. _cookbook_move_mm2.0_to_2.1:

Migrating from MetaModels 2.0 to 2.1
======================================

Migrating from MetaModels 2.0 (Contao 3.5) to MetaModels 2.1 (Contao 4.4) follows the same
migration path as the general upgrade from Contao 3 to Contao 4.

There are various approaches — one of them is:

* Update Contao 3.5 with MM 2.0 — update all other extensions as well
* Install Contao 4 from scratch
* Install all extensions that have a Contao 4 bundle via the Contao Manager or directly via Composer
* Copy all extensions that have no bundle to ``/system/modules``
* Copy the database from C3 to C4 — or update the MySQL credentials to point to the old database

Afterwards, the website should be working again — if needed, run another ``composer update``,
clear the cache, and update the database via the install tool.
