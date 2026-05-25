.. _component_schema-manager:

Schema Manager
==============

.. note:: The schema manager is implemented from version 2.3.

Quick Info
----------

With the schema manager, the mm_* tables of models and columns of attributes are not created
immediately when saving. It is necessary to run a database migration via console, manager or
install tool — analogous to the procedure when DCA changes to the database are pending. Corresponding
hints are stored in the input form of the model and attributes.


Background
----------

In MetaModels 2.3, a new schema manager was introduced that establishes communication with Doctrine.
Doctrine is the database abstraction in Symfony on which Contao is also built. The use of the new
schema manager arose from necessities such as Contao viewing mm_* tables as "foreign" and wanting to
delete them. Advantages of using it include a clean and secure table structure as it is monitored by
Doctrine. Changing, deleting or copying of attributes no longer needs to be handled by MM, but is
handled by Doctrine.

However, the change also brings a fundamental change in working with MM: when creating a model and
attribute, a DB migration via the usual ways (console, manager or install tool) must always be
performed — just as it has always been necessary with Contao and DCA changes.

For columns, this is only relevant for attributes that create their own column in the mm_* table,
such as Text or Alias (Simple Attributes).

This change anticipates what would have happened at MM 3.0 at the latest. There, the possibility
is also planned to define the database schema of MM via files. This enables versioning, reusability
and import/export. With this feature, schema modifications are only possible by running a DB migration.

The adjustment of your own "MM workflow" certainly requires some getting used to, but the schema
manager has created a technically more expandable foundation for database handling.

Since Doctrine splits changes to existing tables or columns into two actions, there is the option
to "rescue" existing data — see :ref:`Tips <rst_cookbook_tips_change_table_column_name>`.

Via the schema manager, column types can be permanently adjusted — the changes are carried out by
the DB migration — see :ref:`rst_cookbook_inputmask_manipulate-schema`.
