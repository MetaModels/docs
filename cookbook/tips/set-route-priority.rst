.. _rst_cookbook_tips_set-route-priority:

Setting the Route Priority
==========================

.. note:: Route priority is available from Contao 4.13 and is handled by MM from version 2.3.

Contao and MetaModels must extract the page from a given URL based on its alias and any
included slug parameters (key-value pairs) and respond accordingly. Conflicts and ambiguities
can arise when interpreting the meaning of URL elements. Route priority allows you to influence
the order in which these are resolved. The following examples illustrate the available options.


Filtering with "folderurl" and "auto_item"
------------------------------------------

A typical structure for displaying data with MM is a list page and a detail page. The detail
page often hides the filter key using the URL parameter "auto_item". The detail page is also
frequently a subpage of the list page. With ``folderurl`` enabled, the pages might be structured
as follows:

* List page alias: ``projects``
* Detail page alias: ``projects/project``

With an MM filter value of ``test``, the full URL would be ``projects/project/test`` (without
domain and suffix).

This could be interpreted as either:

* Alias: ``projects`` with key: ``project`` and value: ``test`` — or
* Alias: ``projects/project`` with key: ``auto_item`` and value: ``test``

Without prioritising the resolution, it would be more or less random which variant is resolved
first. By setting a higher route priority (10) on the detail page than on the list page (0) in
the page properties, the resolution becomes unambiguous and is handled cleanly.

If the detail page alias is e.g. simply ``project-details``, there is no ambiguity and route
priority is not needed.


List and Detail Page with the Same Alias
-----------------------------------------

If you want to serve both the list and the detail page under the same page alias, this can be
achieved with the following settings:

**List page:**

* Title: List
* Alias: list
* Route priority: 0
* Requires item: off
* MM List — in render settings, configure redirect to "Details" page + filter

**Detail page:**

* Title: Details
* Alias: list
* Route priority: 10
* Requires item: on
* MM List with filter rule "Simple lookup" and URL parameter "auto_item"

The list would then be accessible e.g. via the alias ``projects`` and a detail view via
``projects/test``.


Link to a Detail Page in the Navigation
-----------------------------------------

If you want to include one or more MM detail pages in the standard Contao page navigation, you
should use appropriate extensions and their hooks or events.

A quick solution is to create a regular Contao page with e.g. alias ``projects/test`` and a
detail page with alias ``projects``, whose detail view can be called via ``projects/test``
(URL parameter "auto_item").

By giving the MM detail page a higher route priority (10) than the regular Contao page (0), the
link is resolved cleanly.
