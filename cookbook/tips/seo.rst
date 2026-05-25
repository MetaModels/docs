.. _rst_cookbook_tips_seo:

Search Engine Optimisation (SEO)
=================================

To help search engine bots find and index MM content on your website, various settings can be
configured.

Contao Search
-------------

For the `Contao search <https://docs.contao.org/manual/en/layout/module-management/website-search>`_,
content is indexed in various ways — either when a page is visited by a user, or by the
`Contao crawler <https://docs.contao.org/manual/en/cli/crawl/>`_, which follows links on pages
and evaluates the ``sitemap.xml``.

For indexing to work, the usual permissions must be granted in the page settings and search must
not be excluded.

To support indexing of detail pages via ``sitemap.xml``, a custom index can be set up in MM —
see :ref:`component_searchable-pages`.

For pages with FE filters that contain link lists, note
:ref:`how to exclude links from crawling <rst_cookbook_filter_exclude-url-from-search-index>`.


SEO for Google & Co.
---------------------

.. _rst_cookbook_tips_seo_url:
"Human-Readable" URLs
.....................

This mostly concerns linking to the detail page, e.g. from a list page. The alias of the item
is typically used for filtering on the detail page. In the settings of the
:ref:`Alias <component_attribute_alias>` or
:ref:`Translated Alias <component_attribute_translatedalias>` attribute, the desired combination
of other attribute values can be defined.


.. _rst_cookbook_tips_seo_metadata-title:
Meta Data: Title and Description
.................................

These settings primarily apply to a detail page. In the CE/FE module MM List, there is a
select field for choosing existing attributes for the title and description.

The description should concisely summarise the content of the page. While
`Google does not enforce a maximum character count <https://developers.google.com/search/docs/appearance/snippet#meta-descriptions>`_,
many SEO resources recommend a maximum of 150–160 characters — Contao itself limits this to
320 characters in ``fe_page``.

For more individual control, custom text attributes can be created for title and description,
allowing editors to optimise these values independently of other attributes.

As a further option, title and description can be set directly in the render template —
see :ref:`component_templates`. The following snippet shows how this can be done in the template:

.. code-block:: php
   :linenos:

   <?php
   // templates/metamodels_prerendered_details.html5
   use Contao\CoreBundle\Routing\ResponseContext\HtmlHeadBag\HtmlHeadBag;
   use Contao\StringUtil;
   use Contao\System;

   $container       = System::getContainer();
   $htmlDecoder     = $container->get('contao.string.html_decoder');
   $responseContext = $container->get('contao.routing.response_context_accessor')->getResponseContext();
   $htmlHeadBag     = $responseContext->get(HtmlHeadBag::class);
   ?>
   ...
   <?php
   $htmlHeadBag->setTitle($htmlDecoder->inputEncodedToPlainText($arrItem['text']['title'] . ' - ' $arrItem['text']['art_no']));
   $htmlHeadBag->setMetaDescription(StringUtil::substr($htmlDecoder->inputEncodedToPlainText($arrItem['text']['title'] . ' - ' $arrItem['text']['description']), 160));
   ?>

The ``$htmlHeadBag`` could also be provided via a helper class with injected services.


.. _rst_cookbook_tips_seo_breadcrumb:
Breadcrumb
..........

The last item in the breadcrumb is automatically generated from the page title. If the
:ref:`page title is generated dynamically from an MM attribute <rst_cookbook_tips_seo_metadata-title>`
on a detail page, it will not appear in the breadcrumb. This is because the FE module for the
breadcrumb is typically placed before the MM list and receives this information too late — or
not at all.

A "BreadcrumbController" that adds the breadcrumb later during page rendering can solve this.
An
`example implementation <https://gist.github.com/fritzmg/e7df2804365fe676b1d88f053a234707?permalink_comment_id=5698503#gistcomment-5698503>`_
is currently available —
`this issue will be fixed in Contao 5.7 <https://youtu.be/nvIPd3OzXhs?t=1787>`_.


.. _rst_cookbook_tips_seo_metadata-hreflang:
Meta Data: hreflang
...................

If a multilingual website also outputs content in one or more other languages, a ``hreflang``
link can be provided to help search engines. To offer language switching to frontend visitors,
various extensions are available — the most common is
"`ChangeLanguage <https://github.com/terminal42/contao-changelanguage>`_".

This extension automatically generates ``hreflang`` metadata, provided the corresponding
relations are configured in the page settings.

The links output in the source code work without further adjustments, but only point to the
page alias of the other language pages — without passing filter parameters such as those used
on a detail page.

The "ChangeLanguage" extension offers an option in the page settings called "Keep query
parameters" where keys can be specified. The corresponding key-value pairs are then appended to
the other language links. However, the key ``auto_item`` is not supported natively.

For language switching on a detail page, the following configuration can be used:

* Filter "Details" with filter rule "Simple lookup" for attribute "Alias" — leave "URL parameter"
  as ``alias``, do not set it to ``auto_item``
* In the page settings under "Keep query parameters", enter ``alias`` on all detail pages

The result would look like this:

.. code-block:: html
   :linenos:

   <link rel="alternate" hreflang="de" href="http://my-domain.tld/de/details/alias/mayer-herbert">
   <link rel="alternate" hreflang="x-default" href="http://my-domain.tld/de/details/alias/mayer-herbert">
   <link rel="alternate" hreflang="en" href="http://my-domain.tld/en/details/alias/mayer-herbert">

If you want to create links using ``auto_item`` or include translated key-value pairs from
multilingual attributes in the URL, a custom implementation is required — for example via the
"`changelanguageNavigation <https://extensions.terminal42.ch/docs/changelanguage/en/developers/>`_"
hook.

:ref:`More on multilingual support in MM. <component_multi-language>`


.. _rst_cookbook_tips_seo_filter-url:
Slug vs. GET Parameters in Filter URLs
.......................................

From MM 2.4, each filter rule has a setting "URL type for the parameter" that controls whether
a filter parameter is passed as part of the URL path (slug) or as a classic GET parameter —
see :ref:`Filter rule settings <component_filter>`.

**Slug parameters (human-readable URL paths)** produce clean, readable URLs — e.g.
``/products/category/with-battery`` instead of ``/products?category=with-battery``. These are
generally the preferred option for indexable content, as search engines treat them as distinct
pages. They are particularly suitable when filter combinations should function as standalone,
indexable landing pages. However, this can quickly lead to a large number of potential URL
variants, which should be carefully managed via canonical tags or indexing rules. With slug
parameters, using ``auto_item`` as the URL parameter hides the key — e.g. for a category:
``/products/with-battery`` — but ``auto_item`` only works for a single URL parameter.

**GET parameters** (e.g. ``?colour=red``) are processed by search engines as well, but are
considered less "readable" and often interpreted as technical variants of the same page. They
are therefore better suited for purely functional filters that should have no independent SEO
relevance and typically do not need to be indexed. Combined with appropriate canonical URLs,
this prevents an unnecessary proliferation of duplicate content variants. Additionally, for
tracking tools such as Google Analytics, certain GET parameters can be excluded from tracking.

**Recommendation:** |br|
Slug parameters should be used for filterable, SEO-relevant combinations, while GET parameters
are better suited for purely interactive or non-indexable filters. The decision ultimately
depends on whether a filter combination should function as a standalone landing page or merely
serve internal navigation.

Note: if a parameter is present as both a slug and a GET parameter, Contao returns a 404.


Pagination of List Output
..........................

Longer output lists should be paginated — both for better user readability and faster page
loading, and for better ranking in search engine performance evaluations.

The output page should include a
`canonical URL <https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls>`_
in its metadata — this can be enabled in the page settings. Google specifies that the first page
(base page) should not be marked as the canonical page — only subsequent pagination pages
should be — see
`Google "Use URLs correctly" <https://developers.google.com/search/docs/specialty/ecommerce/pagination-and-incremental-page-loading#use-urls-correctly>`_.

The use of ``rel="next"`` and ``rel="prev"``
`is no longer honoured by Google <https://developers.google.com/search/docs/specialty/ecommerce/pagination-and-incremental-page-loading#use-urls-correctly>`_ —
other search engines such as Bing may still evaluate them, and some browsers use these meta
links to prefetch pages.

To output these tags in the ``head`` of the page, a custom template ``mm_pagination.html5`` can
be created and the meta tags added there — e.g.

.. code-block:: php
   :linenos:

   <?php
   // templates/mm_pagination.html5
   ...
   <?php if ($this->hasPrevious): ?>
      <?php $GLOBALS['TL_HEAD'][] = '<link rel="prev" href="' . $this->previous['href'] . '" />' ?>
   ...
   <?php if ($this->hasNext): ?>
      <?php $GLOBALS['TL_HEAD'][] = '<link rel="next" href="' . $this->next['href'] . '" />' ?>
   ...

.. _rst_cookbook_tips_seo_structured-data:
Structured Data
...............

For semantic classification of the output content, so-called
"`Structured Data <https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data>`_"
can also be included in the source code.

With this supplementary data, a search engine can classify the content — for example as an FAQ,
event, job listing, property ad, or recipe.

How to embed this data is described in the article
":ref:`rst_cookbook_templates_fe_template_schema_org`".

To include images from the File attribute — as is standard in Contao — in JSON-LD data, a
corresponding template ``mm_attr_file_contao_image.html5`` is available. It must be selected in
the render settings for the attribute, or its content incorporated into a custom template.

There is also a template ``mm_attr_file_contao_image_ofpage.html5``, which additionally includes
the first image as ``primaryImageOfPage`` in the JSON-LD data. If this image entry is present,
it will be displayed e.g. in Contao search results — analogous to the detail views of Contao
news and events.

.. |br| raw:: html

   <br />
