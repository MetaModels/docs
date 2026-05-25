.. _rst_cookbook_specials_helper-models:

Helper Model — Consolidating Various Selection Data in One MetaModel
=====================================================================

When building a data structure, it is common to need simple selections such as
salutation, academic title, gender, colours, units of measurement, discounts,
etc. Each of these would need to be created as a separate model and referenced
in the desired model via single or multi-select.

This would result in a large number of models, each ultimately consisting of
only two attributes: name and alias.

The setup and maintenance of these "auxiliary entries" can be simplified by
managing the data in a helper model construct and showing only the matching
values in the input mask via a filter.

The disadvantage of this approach is that it is inflexible when adjustments are
needed, and individual value ranges may need to be moved back into a separate
model. For example, if at a later point the "Discounts" entries should also have
a numeric discount value in addition to the label.

An example of a possible setup is as follows: create two MetaModels — one for
the names of the taxonomy groups, e.g. "Taxo Groups", and one for the taxonomy
values, e.g. "Taxo Values". In the model where the values are needed as a
selection, create a reference as a single or multi-select to the "Taxo Values"
model, along with a filter that only shows values from the desired taxonomy
group. The individual steps would be:

**1. Create MetaModel "Taxo Groups"**

* Text or translated text attribute with "Name", required field
* Alias attribute with "Alias" pointing to "Name", unique values and force recreation

**2. Create MetaModel "Taxo Values"**

* Single select attribute with "Taxo Groups" pointing to the "Taxo Groups" model, required field
* Text or translated text attribute with "Name", required field
* Alias or translated alias attribute with "Alias" pointing to "Name", unique values and force recreation
* Sorting on "Taxo Groups", grouping type "First letter" and grouping length 0
* Filter "Select group" with filter rule "Simple lookup" on attribute "Taxo Groups" with "Static parameter" checkbox enabled

|img_helper-models_01|

**3. Integration in MetaModel with selection**

* Single or multi-select attribute pointing to the "Taxo Values" model, filter "Select group", and in the filter parameter select a group, e.g. "Salutation".

|img_helper-models_02|

In addition to this two-table approach, it is also possible to work with a
single table, e.g. using the "Variants" option or render mode "Hierarchy" —
the filter settings must then be adjusted accordingly.


.. |img_helper-models_01| image:: /_img/screenshots/cookbook/specials/helper-models_01.png
.. |img_helper-models_02| image:: /_img/screenshots/cookbook/specials/helper-models_02.png
