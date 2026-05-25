.. _rst_cookbook_templates_fe_template_schema_org:

Outputting Structured Data in the FE Template
=============================================

MM data can be supplemented in the source code with so-called "structured data" to make the
content easier to analyse — for example by search engines. One of the most well-known catalogues
for such markup is available at `Schema.org <https://schema.org>`_.

Schemas can be created in the encodings ``RDFa``, ``Microdata``, and ``JSON-LD`` and embedded
into the page. Up to Contao 4.9, the encoding Contao used was ``Microdata`` — since Contao 4.12,
``JSON-LD`` is used.

To add the markup, create a custom template based on ``metamodels_prerendered.html5`` and adapt
it as shown in the following examples for a job posting — see
`JobPosting <https://schema.org/JobPosting>`_.

The markup can be validated with tools such as:

* `Rich Results Test <https://search.google.com/test/rich-results>`_
* `Schema Validator <https://validator.schema.org/>`_

More on this topic can be found at :ref:`rst_cookbook_tips_seo`.

Markup with ``JSON-LD``
------------------------

.. note:: This is only available from Contao 4.13 with MM 2.3.

.. code-block:: php
   :linenos:

   <?php

   use Contao\CoreBundle\Routing\ResponseContext\JsonLd\JsonLdManager;
   use Contao\System;
   use Spatie\SchemaOrg\JobPosting;
   use Spatie\SchemaOrg\Organization;
   use Spatie\SchemaOrg\Place;
   use Spatie\SchemaOrg\PostalAddress;
   use Spatie\SchemaOrg\PropertyValue;

   $jsonLdGraph     = null;
   $responseContext = System::getContainer()->get('contao.routing.response_context_accessor')->getResponseContext();
   if ($responseContext && $responseContext->has(JsonLdManager::class))
   {
       /** @var JsonLdManager $jsonLdManager */
       $jsonLdManager = $responseContext->get(JsonLdManager::class);
       $jsonLdGraph   = $jsonLdManager->getGraphForSchema(JsonLdManager::SCHEMA_ORG);
   }
   ?>
   <?php if (count($this->data)): ?>
       <div class="layout_full">
           <?php foreach ($this->data as $arrItem): ?>
               <?php
               // Build Schema.org data.
               $schemaData = (new JobPosting())
                   ->identifier((new PropertyValue())->propertyID('jobId')->value($arrItem['raw']['id']))
                   ->hiringOrganization((new Organization())->name($arrItem['text']['corporation_name']))
                   ->title($arrItem['text']['name'])
                   ->datePosted(date('Y-m-d', $arrItem['raw']['created_date']))
                   ->jobLocation((new Place())->address((new PostalAddress())->addressCountry($arrItem['text']['country'])))
                   ->description($arrItem['text']['description']);
               ?>
               <div class="item <?= $arrItem['class'] ?>">
                   <h2 itemprop="title"><?= $arrItem['text']['title'] ?></h2>
                   <div>
                       <p><strong>Location:</strong><?= $arrItem['text']['city'] ?> <?= $arrItem['text']['region'] ?>
                       </p>
                   </div>
                   ...
                   <div class="actions">
                       <?php if (null !== ($href = $arrItem['actions']['jumpTo']['href'] ?? null)) {
                           $schemaData->url($href);
                       } ?>
                       <?php foreach ($arrItem['actions'] as $action): ?>
                           <?php $this->insert('mm_actionbutton', ['action' => $action]); ?>
                       <?php endforeach; ?>
                   </div>
               </div>
               <?php /* Add Schema.org data. */ $jsonLdGraph?->add($schemaData, 'job-' . $arrItem['raw']['id']); ?>
           <?php endforeach; ?>
       </div>
   <?php else : ?>
       <?php $this->block('noItem'); ?>
       <p class="info"><?= $this->noItemsMsg ?></p>
       <?php $this->endblock(); ?>
   <?php endif; ?>

Embedding via ``JSON-LD`` does require a few extra lines of code, but the markup is separated
from the HTML source used for browser rendering. This makes it easier to adapt existing templates
or extend them with additional markup.

When adding multiple records to the graph — e.g. in an MM list output — passing a unique
identifier is required: ``$jsonLdGraph?->add($schemaData, <Unique-ID>)``.


Markup with ``Microdata``
--------------------------

Microdata markup requires more extensive template changes — embedding as JSON-LD is therefore
recommended.

.. code-block:: php
   :linenos:

   <?php if (count($this->data)): ?>
       <div class="layout_full">
           <?php foreach ($this->data as $arrKey => $arrItem): ?>
               <div class="item <?= $arrItem['class'] ?>" itemscope itemtype="https://schema.org/JobPosting">
                   <h2 itemprop="title"><?= $arrItem['text']['title'] ?></h2>
                   <div>
                       <p><strong>Location:</strong> <span itemprop="jobLocation" itemscope
                                                           itemtype="https://schema.org/Place">
                               <span itemprop="address" itemscope itemtype="https://schema.org/PostalAddress">
                               <span itemprop="addressLocality"><?= $arrItem['text']['city'] ?></span>
                                   <span itemprop="addressRegion"><?= $arrItem['text']['region'] ?></span>
                               </span>
                           </span>
                       </p>
                   </div>
                   ...
               </div>
           <?php endforeach; ?>
       </div>
   <?php else : ?>
       <?php $this->block('noItem'); ?>
       <p class="info"><?= $this->noItemsMsg ?></p>
       <?php $this->endblock(); ?>
   <?php endif; ?>
