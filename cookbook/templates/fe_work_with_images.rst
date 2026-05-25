.. _rst_cookbook_templates_fe_work_with_images:

Working with Images in Templates
=================================

For outputting one or more images in MetaModels, the File attribute is available. To select a
file or multiple files, the input mask provides a button that opens a popup for browsing the
file manager.

In the basic attribute settings, various options can be configured such as the base path,
single or multiple selection, file types, etc.

In most cases, image files should also be output as actual images. The relevant settings for the
attribute are found in the render settings. Here, the checkbox "Use as image field with preview
image" must be enabled and an image size selected (this also applies to display in the backend
list view). Optionally, a placeholder image can be selected and a lightbox view can be enabled.

This setting applies to both frontend and backend display (if images are shown there).

In a custom FE template, the ``html5`` node must be output for image rendering — |br|
e.g. ``<?= $arrItem['html5']['my_image'] ?>``.

By selecting or customising the :ref:`attribute template <component_templates>`, additional
output requirements can be met such as listing as ``ul`` or ``div``, markup for galleries or
sliders, etc.

These output options cover a wide range of requirements. However, if images need to be output
that are included via a :ref:`relation <component_relations>` — through the ``raw`` node —
they are only available as the original image via path or UUID.

To also manipulate these images in a custom template, the following snippets provide guidance.

More on the available methods can be found in the Contao documentation under
`Image processing <https://docs.contao.org/dev/framework/image-processing/index.html>`_.

On the topic of `responsive images`, there is a (still relevant)
`talk by Peter Müller from CK2016 <https://www.youtube.com/watch?v=ub8yROSQyQ4>`_.


Outputting an Image Included via Single Select
----------------------------------------------

Insert Tags
...........

see `Insert Tags <https://docs.contao.org/manual/en/article-management/insert-tags/#miscellaneous>`_

.. code-block:: php
   :linenos:

   <?php if (!empty($arrItem['raw']['speaker']['biography_image']): ?>
       {{image::<?= $arrItem['raw']['speaker']['biography_image'] ?>?width=180&height=180&mode=crop&class=img--circle}}
       <!-- OR -->
       {{picture::<?= $arrItem['raw']['speaker']['biography_image'] ?>?size=_image_circle}}
       <!-- OR -->
       {{figure::<?= $arrItem['raw']['speaker']['biography_image'] ?>?size=_image_circle&metadata[title]=<?= $arrItem['raw']['speaker']['full_name'] ?>}}
   <?php endif; ?>

Example for ``$size`` (`see <https://docs.contao.org/dev/framework/image-processing/image-sizes/index.html>`_):

.. code-block:: yaml
   :linenos:

   # config/config.yml
   contao:
       image:
           sizes:
               _defaults:
                   formats:
                       jpg: [webp, jpg]
                       webp: [webp, jpg]
                       png: [webp, png]
                   densities: 0.5x, 2x, 3x
                   lazy_loading: true
                   resize_mode: proportional
               image_circle:
                   width: 180
                   height: 180
                   resize_mode: crop
                   zoom: 100
                   css_class: img--circle


Image Studio FigureRenderer
...........................

* ``$from``: path to the file
* ``$size``: see above
* ``$configuration``: configuration options e.g. metadata
* ``$template``: output template

.. code-block:: php
   :linenos:

   <?php
   if (!empty($arrItem['raw']['speaker']['biography_image'])) {
      $from          = $arrItem['raw']['speaker']['biography_image'];
      $size          = '_image_circle';
      $configuration = [];
      $template      = 'image';
      echo $container->get('contao.image.studio.figure_renderer')->render($from, $size, $configuration, $template);
   }
   ?>


Image Studio FigureBuilder
..........................

see `FigureBuilder <https://docs.contao.org/dev/framework/image-processing/image-studio/index.html#using-the-figurebuilder>`_

* ``fromPath``: path to the file
* ``setSize``: see above
* ``$configuration``: configuration options e.g. metadata
* ``$template``: output template

.. code-block:: php
   :linenos:

   <?php
   if (!empty($arrItem['raw']['speaker']['biography_image'])) {
       $figure = $container
         ->get('contao.image.studio')
         ->createFigureBuilder()
         ->fromPath($arrItem['raw']['speaker']['biography_image'])
         ->setSize('_image_circle')
         ->build();

       $template = new FrontendTemplate('image');

       $figure->applyLegacyTemplateData($template);
       //$template->setData($figure->getLegacyTemplateData()); // Alternative
       echo $template->parse();
   }
   ?>


Image Studio PictureFactory
...........................

see `PictureFactory <https://docs.contao.org/dev/framework/image-processing/image-picture-factory/index.html#picture-factory>`_

* ``setSize``: see above
* ``$data``: image data + metadata
* ``$pictureTemplate``: output template

.. code-block:: php
   :linenos:

   <?php
   // would typically be extracted into a helper
   use Contao\FrontendTemplate;
   use Contao\StringUtil;
   use Contao\System;

   $container      = System::getContainer();
   $rootDir        = $container->getParameter('kernel.project_dir');
   $pictureFactory = $container->get('contao.image.picture_factory');

   // ...
   if (!empty($arrItem['raw']['speaker']['biography_image'])) {
      $staticUrl = $container->get('contao.assets.files_context')->getStaticUrl();
      $picture   = $pictureFactory->create($rootDir . '/' . $arrItem['raw']['speaker']['biography_image'], '_image_circle');

      $data = [
         'img'     => $picture->getImg($rootDir, $staticUrl),
         'sources' => $picture->getSources($rootDir, $staticUrl),
         'alt'     => StringUtil::specialcharsAttribute(''),
         'class'   => StringUtil::specialcharsAttribute(''),
      ];

      $pictureTemplate = new FrontendTemplate('picture_default');
      $pictureTemplate->setData($data);

      echo $pictureTemplate->parse();
   }
   ?>

.. note:: This page is open to further snippets — once MM also supports Twig templates, the
   page will be updated accordingly.


.. |br| raw:: html

   <br />
