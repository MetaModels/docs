.. _rst_features:

Feature Overview
================

Data Models
-----------

MetaModels enables the convenient and (almost) unlimited definition of data models in the Contao
backend — without any programming. Various :ref:`relations <component_relations>` are available for
building complex data models, to store data in separate tables and link them —
see `normalization <https://en.wikipedia.org/wiki/Database_normalization>`_.

Various data types are available for data fields (attributes) in the data models, such as text, images,
numbers, date, files — :ref:`here is an overview of data types for attributes <component_data-in-attributes>`
and a :ref:`listing of all attributes <component_attribute_index>`. If a limitation is reached because the
desired data type is not available, a custom implementation is possible using the :ref:`MM API <ref_api>`.

It is also possible to link tables to other Contao Core tables, to create
:ref:`"parent-child connections" <component_relations_child-tables>` or to implement
:ref:`variant inputs <component_relations_variants>`.


Input Forms
-----------

Complex input forms can be defined for the backend, which keep "editors" in the familiar "look & feel"
of Contao. Within an input form, you can react to the input of values or checkboxes to optionally show
different sub-palettes — see :ref:`component_dca_visibility-conditions`.

For easy orientation in the data, the display can be extended with various filters, search and grouping
functions.

The flexible rights system developed for MetaModels allows defining different backend views for editor
and administrator user groups.

The backend can be further customized so that only certain groups have access to individual input fields,
and their order can also be individually adjusted per user group.


Multilingual Support
--------------------

MetaModels was developed from the beginning with multilingual requirements in mind.
Attributes can therefore support the translation of the data they store into multiple languages.
You simply need to switch to the desired language in the backend using the language selector,
and can immediately edit the record in the selected language.

The best part is that attributes that are not translatable are simply not translated. This makes it
possible, for example, to make only the names and descriptions of products translatable, but not the
product number and measurements. This approach reduces the redundancy of data that needs to be entered.

:ref:`More about the settings and handling of multilingual support in the components of a
MetaModel. <component_multi-language>`


Filters
-------

MetaModels has a powerful filter concept that can also handle complex tasks. The website administrator
can completely freely adapt the filter interactions to their needs. This is achieved through the
configuration and combination of filter settings and their parameters.

MetaModels comes with various filter settings to create filter input fields in the frontend, such as
select boxes, range filters, free-text search, etc. — :ref:`an overview can be found here
<component_filter_list>` and :ref:`here, which attributes can be filtered in which way
<component_data-in-attributes>`.

Combining this filter with filter settings such as AND/OR conditions or custom SQL queries creates
complex and interactive filters.

MetaModels places no restrictions on the combination of filters and handles extremely complex filter
scenarios. Thanks to the open structure of the :ref:`MM API <ref_api>`, custom filter rules can also
be programmed with little effort.


Dynamic Views
-------------

Using the output settings, MetaModels has implemented Contao's "partial" template concept in an
extended form. The user can customize every aspect of the views at the attribute and record level.

Various general settings can be defined in the backend configuration. However, these can also be
overridden, finely adjusted or completely ignored by specifying a custom template at the attribute or
record level. These output settings offer the most flexible way to define 'data views'.

The designer can define a completely different view for each purpose, whether it's a simple list output,
a teaser for the homepage or a detail view of a record, and also when and where it should be used.

On the ":ref:`component_templates`" page, the hierarchy and structure of templates is explained.


Extensions
----------

For special tasks, there are additional packages that supplement and extend MetaModels' functionality,
such as perimeter search, wish list, frontend editing or an interface to the `Isotope` shop.

An overview can be found here: :ref:`extended_index`


Outlook
-------

The range of MetaModels features continues to be worked on continuously. A quick implementation of
further features is only possible with financial support or the release of commissioned programming —
for information, visit the `MetaModels project website <https://now.metamodel.me>`_ or contact the
MM team directly: `mail@metamodels.me <mailto:mail@metamodels.me>`_.
