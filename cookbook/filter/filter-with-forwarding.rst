.. _rst_cookbook_filter_filter-with-forwarding:

Filter with Forwarding
=======================

If you want to spread the filtering of an MM list across multiple pages, you can use the
"Forwarding" option and specify the target page there. For example, on a travel website you
could add the query for travel start date and number of persons as a "pre-filter" on different
pages and then forward to the actual list page with further filter options.

In MM, a filter element is also responsible for converting POST parameters — sent by the filter
form — into GET parameters for list filtering. For this reason, a filter must be present on the
target page to handle this conversion.

Contao controls form processing responsibility via the value of the "FORM_SUBMIT" field — this
value must be identical. There are two ways to achieve this:

**FE Module MM Filter**

If you create an FE module of type MM Filter and embed it on the desired pages via the CE module
selection, the value of "FORM_SUBMIT" will always be the same. The downside is that the filter
widget selection from the module settings is also identical everywhere. This can be mitigated by
adding a second filter on the target page for additional options. If the URL parameters of the
filter widgets are the same across both filters, the second filter will also respond to existing
GET parameters.

**CE MM Filter with Custom Form ID**

.. note:: The form ID can be customised from MM 2.3.

You can create CE MM Filter elements on the starting page(s) and the target page, and enter the
same value in the "Form ID" field. The filter on the target page then handles the corresponding
processing of POST parameters. The advantage here is that the filter widgets to be displayed
can be more easily selected per CE MM Filter.

The filter — whether FE module or CE — that handles the "POST-to-GET" conversion does not
necessarily need to be visible on the target page and can be hidden if needed.


