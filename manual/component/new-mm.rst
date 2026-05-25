.. _component_new-mm:

|img_new| New MetaModel
========================

.. note:: Create a new MetaModel (database table), optionally enable translation or variants |br|
   To create the mm_* table, run a DB migration — :ref:`see schema manager <component_schema-manager>`


Introduction
------------
Clicking on the "|img_new| New MetaModel" icon opens an input form for creating a new MetaModel.
When the new MetaModel is saved, a new, separate table is created in the database to store the values.

Two input fields are therefore mandatory for saving the new MetaModel: the name of the MetaModel
and the table name.

The name of the MetaModel is used as the label in the backend and can be freely chosen. However,
the label should meaningfully indicate the content, e.g. "Addresses".

The same applies to the table name, where the prefix "mm\_" can be entered as part of the table name
or is automatically added. The table could then be named "mm_address" for example — whether the name
should be singular or plural is a matter of different "opinions".

When the table is created, only some columns necessary for interaction with the MetaModels extension,
such as id, pid, timestamp, etc., are created in it. The additional, individual columns are created as
so-called "attributes" and given their specific options. More about this under :ref:`component_attribute`.


Options
-------

When creating a new MetaModel, there are additional options for "Translation" and "Variants".

If the "Translation" option was selected, after reloading the page, several languages are available for
selection. One of the languages should be activated as "Fallback" — if this is not done, the first
selected language is used as the fallback. If the "Translation" option is activated in the MetaModel,
special multilingual attributes are additionally offered as options.

When multilingual support is activated retroactively, the existing attributes and entered values are
not automatically transferred. Whether multilingual support is required should therefore be clarified
in advance if possible.

If the "Variants" option was selected, you initially see no further change to the MetaModel. When the
option is set, attributes can have the "Override variants" option activated. With all attributes that
have the "Override variants" option set, additional input forms can be created for variant input, e.g.
for "overriding" "parent values". The input forms for variants are accessed via the
"|img_variants| New variant" icon in the list view of the parent elements.

Variants create a "parent-child relationship" within a MetaModel database table, which can be tracked
via various values in the table — e.g. in a custom SQL filter. Parent records are characterized by the
fact that in the database table, the values for varbase equal 1 and vargroup equal the record's own ID.
Child records have varbase equal 0 and vargroup equal the ID of the parent record.


.. |img_variants| image:: /_img/icons/variants.png
.. |img_new| image:: /_img/icons/new.gif


.. |nbsp| unicode:: 0xA0
   :trim:

.. |br| raw:: html

   <br />
