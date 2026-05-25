.. _component_searchable-pages:

|img_searchable_pages_32| Indexing
====================================

.. note:: Include detail pages of a MetaModel in the sitemap.xml of Contao

Introduction
------------

Indexing allows the detail pages of a MetaModel rendering (list) to be included in the
generation of sitemap.xml.

This "special treatment" of detail pages compared to normal list views arises from how they
are called. Detail pages created in the Contao page tree must always be called with specific
GET or URL routing parameters to output a (meaningful) detail page with values. The Contao
function for generating sitemap.xml cannot access these parameters from MetaModels and therefore
requires appropriate support.

"Normal list views" do not require this special treatment, and the pages are automatically and
correctly included in search or sitemap via Contao functions.

The "base page" as created by Contao is removed from sitemap.xml, i.e. the page
``domain.tld/my-project/detail.html`` does not appear in sitemap.xml but only the URLs with the
filter parameter, e.g. ``domain.tld/my-project/detail/alias-1.html``, ``.../alias-2.html``, etc.

Detail pages are not included in the "Sitemap" frontend module.

Note that Contao does not index URLs with certain keywords as "keys" such as `id`, `file`,
`year`, etc.; e.g. as URL details/id/my-details-123.html — the keywords are listed in the
array `$GLOBALS['TL_NOINDEX_KEYS'] <https://github.com/contao/core/blob/master/system/modules/core/config/config.php#L419>`_.

Detail pages are more easily included in the (normal) Contao search via links in sitemap.xml —
see `contao:crawl <https://docs.contao.org/manual/en/cli/crawl/>`_.

Options
-------

* **Name**: |br|
  Label for the backend
* **Render settings**: |br|
  Selection of the render settings for the list view that also leads to the detail view
* **Filter set**: |br|
  Selection of a filter set to narrow down the detail pages — e.g. to only output published
  records or include them in sitemap.xml

Procedure
---------

A new indexing is created via the icon "|img_new| New indexing" and after entering the name,
the render setting is selected. The render setting is usually the same as the one chosen for
the CE/module MetaModel list of the frontend output of the "overview list" — but a separate
render setting can also be created.

A filter must be selected if certain detail page URLs should not appear in sitemap.xml —
e.g. to only include published records.

Since Contao 4.11, sitemap.xml is generated dynamically when called and is no longer stored
in the `share` folder.

Tips
----

* :ref:`rst_cookbook_filter_exclude-url-from-search-index`
* :ref:`rst_cookbook_tips_seo_structured-data` or
* :ref:`rst_cookbook_templates_fe_template_schema_org`
* :ref:`rst_cookbook_specials_add_items_at_navigation`


.. |img_searchable_pages_32| image:: /_img/icons/searchable_pages_32.png
.. |img_searchable_pages| image:: /_img/icons/searchable_pages.png
.. |img_new| image:: /_img/icons/new.gif


.. |br| raw:: html

   <br />
