.. _rst_cookbook_filter_search-text-at-two-fields:

Text Search Across Two Fields
==============================

If you want to implement a text search across two (or more) fields, you can
either use a dedicated filter extension such as `metamodelsfilter_textcombine
<https://github.com/cogizz/metamodelsfilter_textcombine>`_
or solve this with built-in means.

For the built-in solution, the following steps must be set up as filter rules:

* Filter rule "OR" to combine the text fields / filter rules
* The same URL parameter must be set in the filter rules for each text field

As an example, a filter set for simultaneously searching the attributes "Last name"
and "First name" — a note about the label: the label of the last filter rule is
output in the frontend.

Filter set:

|img_multi-textfilter_01|

Setting of the second text filter:

|img_multi-textfilter_02|

Filtering in the frontend:

|img_multi-textfilter_03|

|img_multi-textfilter_04|



.. |img_multi-textfilter_01| image:: /_img/screenshots/cookbook/filter/multi-textfilter_01.jpg
.. |img_multi-textfilter_02| image:: /_img/screenshots/cookbook/filter/multi-textfilter_02.jpg
.. |img_multi-textfilter_03| image:: /_img/screenshots/cookbook/filter/multi-textfilter_03.jpg
.. |img_multi-textfilter_04| image:: /_img/screenshots/cookbook/filter/multi-textfilter_04.jpg
