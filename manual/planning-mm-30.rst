.. _planning_mm30:

Planning for MM 3.0
====================

.. seealso:: This list is continuously being expanded

The following are some points being planned for MM 3.0. With the new major version, more fundamental
changes to MM can be made and the underlying architecture further modernized.

Suggestions are welcome as tickets in `GitHub <https://github.com/MetaModels/core/issues>`_ — preferably
with the title prefix "[MM 3.0]"

* Migration to UUID (e.g. for export/import support)
* Individual MMs should be organizable within a "project" — the project level would sit "above" the
  MMs (currently done with a "project sub-prefix" like "mm__proj1*", "mm__proj2*"). The "project levels"
  would need to extend through all attribute tables; ideally it should then be possible to export and import
  all tables of Project A and Project B independently of each other.
* Configuration via YAML/XML — similar to CustomElements from RST (https://app.intco.it/rsce-visual-editor/index.html)
  — the existing "GUI" in the backend (via DCG?) remains available...

  * Required for export/import
  * Storage/tracking of modifications (e.g. Git)
* Attributes split into classes:

  * Refactoring of the MM API
  * Virtual attributes (for things like geodistance)
  * Strict separation of attributes with no more dependencies (especially for language keys etc.)
  * Alias aware interface https://github.com/MetaModels/core/issues/904, https://github.com/MetaModels/core/tree/feature-aliasaware
  * Templates in Twig
* Database adjustments:

  * Fewer queries
  * ACL at database level
  * Hierarchy/Trees => possibly Nested Set
  * Logging/Audit Trail
  * Versioning/Undo
  * Translations
  * e.g. => http://symfony.com/doc/master/bundles/StofDoctrineExtensionsBundle/index.html
* Schema management (extraction of DB schema manipulations of attributes into independent classes, ... + update handlers etc.)

  * Feature schema management: https://github.com/MetaModels/core/pull/1267
  * Splitting of the relation table into separate tables (also important for export/import)
* Symfony Forms (DCG 3.0)
* API approach for MM to communicate via e.g. REST, Hydra-LD, GraphQL
* ASC/DESC etc. as constants
* Filter refactoring:

  * Better caching,
  * Multiple sorting,
  * Sorting of select/checkboxes/radio,
  * Hierarchical filtering,
  * Passing ID list object instead of array
  * UI/usability BE: (including DCG)
  * CSS/Templates
* Cleanup/reorganization of settings
* Financing:

  * EAP

.. |br| raw:: html

   <br />
