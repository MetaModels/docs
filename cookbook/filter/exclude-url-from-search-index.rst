.. _rst_cookbook_filter_exclude-url-from-search-index:

Excluding Filter URLs from the Contao Search Index
===================================================

If you want an MM list to be included in the search index but not the list calls with filter
parameters, this cannot be configured from the backend.

An FE filter generates various URLs with "key-value pairs" for filtering when passing data to an
MM list. In most cases, including these URLs in the search index is unnecessary or undesirable,
as it only bloats the index without providing meaningful search results.

For filter widgets that generate a URL directly — such as a link list — inclusion in the index
is typically prevented by specifying the ``data-escargot-ignore`` attribute in the widget
template.

However, if a website visitor visits a filter URL, it is still added to the search index,
provided the base page of the list has not been excluded from search. With few filter options,
this is not a problem. But if many filter combinations are possible, this can lead to a
continuously growing search index with little helpful results.

To prevent this, the following code can be used to suppress indexing when a filter is active.
The code snippet must be inserted in the MM list template.

.. note:: For MM 2.4 / Contao 5.3

.. code-block:: php
   :linenos:

   <?php

   use Contao\CoreBundle\Routing\ResponseContext\JsonLd\ContaoPageSchema;
   use Contao\CoreBundle\Routing\ResponseContext\JsonLd\JsonLdManager;
   use Contao\System;

   if (!empty($this->filterParams)) {
       $responseContext = System::getContainer()->get('contao.routing.response_context_accessor')->getResponseContext();
       if ($responseContext?->has(JsonLdManager::class)) {
           /** @var JsonLdManager $jsonLdManager */
           $jsonLdManager = $responseContext->get(JsonLdManager::class);
           $schema        =
               $jsonLdManager->getGraphForSchema(JsonLdManager::SCHEMA_CONTAO)->get(ContaoPageSchema::class);
           $schema->setNoSearch(true);
       }
   }
   ?>

.. note:: For MM 2.3 / Contao 4.13

.. code-block:: php
   :linenos:

    <?php
    if (!empty($this->filterParams)) {
        global $objPage;
        $objPage->noSearch = true;
    }
    ?>
