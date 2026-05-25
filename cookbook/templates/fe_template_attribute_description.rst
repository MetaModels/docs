.. _rst_cookbook_templates_attribute_description.rst:

Outputting the Attribute Description in a Template
===================================================

In the list template, the name or label of an attribute is available via the ``attributes`` node.

If you also want access to the description from the attribute settings, you can make the
following adjustment in the ``metamodels_prerendered.html5`` template:

.. code-block:: php
   :linenos:

   <?php

   /**
    * Add description.
    */

   use Contao\System;
   use MetaModels\IMetaModel;

   /** @var IMetaModel $model */
   $model      = $this->items->getItem()->getMetaModel();
   $attributes = $model->getAttributes();

   $attributeDescriptions = [];
   foreach ($attributes as $attribute) {
       if (empty($attribute->getColName())) {
           continue;
       }
       $attributeDescriptions[$attribute->getColName()] = $attribute->get('description');
   }

   // Debug.
   if (System::getContainer()->get('kernel')->isDebug()) {
       dump($this->data);
   }
   ?>
   <?php if (\count($this->data)): ?>
       <div class="layout_full">
   // ....

To explain: with ``$this->items->getItem()`` we retrieve one item — since attribute data never
changes, one item is sufficient to query the MetaModel and its attributes. The ``foreach`` is
just for easier handling in the rest of the template. The whole thing could also be extracted
more elegantly into a helper —
`see the CK23 talk <https://www.e-spin.de/contao-metamodels/metamodels-vortrag-contao-konferenz-2023.html>`_

In the further output, the description can be accessed via the column name of the attribute — |br|
e.g. ``<?= $attributeDescriptions['firstname'] ?? '' ?>``

For multilingual models, the description matching the frontend language is output.


.. |br| raw:: html

   <br />
