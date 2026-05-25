.. _rst_cookbook_templates_fe_template_ce_elements:

Creating FE Templates via Content Elements
==========================================

.. _rst_cookbook_templates_fe_template_ce_elements_youtube:
CE YouTube
----------

For more complex outputs such as the YouTube CE, there are various additional parameters beyond
the YT ID that can be configured and output. You can embed these CEs via the MM template and use
them for output. CEs can be embedded both in the render settings template
(``metamodels_prerendered.html5``) and in the attribute templates (``mm_attr_*.html5``).

The following illustrates the approach using the YouTube CE as an example. Create a Text
attribute to store the YT ID, and create a new template ``mm_attr_text_video_yt.html5``, which
is then selected in the attribute settings of the render settings.

Enter the following code in the template:

.. code-block:: php
   :linenos:

   <?php

   use Contao\ContentModel;
   use Contao\ContentYouTube;

   $contentData['type']           = 'youtube';
   $contentData['youtube']        = $this->raw . '?rel=0';
   $contentData['youtubeOptions'] = serialize(['youtube_nocookie']);
   $contentData['playerSize']     = serialize(['560', '315']);
   $contentData['playerAspect']   = '16:9';

   $model = new ContentModel();
   $model->setRow($contentData);

   $content = new ContentYouTube($model);

   echo $content->generate();


The basic structure is now in place and can be extended as needed. The available parameters can
be found in the Contao element classes — e.g.
`ContentYouTube <https://github.com/contao/contao/blob/6cfb659affeb526539d776b430bcafa4b0324849/core-bundle/src/Resources/contao/elements/ContentYouTube.php>`_.

Parameters can be hardcoded as in the example — but it is also possible to include values from
additional attributes via ``$this->row['yt_aspect']`` in the attribute template or
``$arrItem['text']['yt_aspect']`` in the render settings template.

In the render settings, input via the CE/FE module parameters is also possible —
see :ref:`rst_cookbook_templates_fe_list_parameters`.

For compact display and input in the input mask, the MCW attribute can also be used to create a
single-line "multi-input" — see :ref:`rst_extended_attribute_mcw`.


.. _rst_cookbook_templates_fe_template_ce_elements_rstslider:
FE Module RockSolid Slider
--------------------------

If you want to output pre-built sliders such as the
`RockSolid Slider <https://rocksolidthemes.com/de/contao/plugins/responsive-slider>`_ as content
in MM, there are as always various approaches — the following is one suggestion:

First, create a slider — e.g. an image slider — via the corresponding navigation item in the
backend and select the desired images. To simplify the configuration settings, also create an FE
module of type "RockSolid Slider" and configure the desired settings for size, animation,
navigation, etc. — the ID of the FE module, e.g. ``55``, will be needed in the template.

In MM, create an attribute of type Select with the following settings:

* Source table: tl_rocksolid_slider
* ID column: id
* Value column: name
* Alias column: id
* Selection sorting: name

This allows a slider to be selected in the input mask — the attribute must of course also be
added to the input mask.

Also create a custom template, e.g. ``mm_attr_select_rst_slider.html5``, with the following
content:

.. code-block:: php
   :linenos:

   <?php
   $moduleData['type']                      = 'rocksolid_slider';
   $moduleData['rsts_id']                   = $this->raw['id'];
   $moduleData['rsts_import_settings']      = 1;
   $moduleData['rsts_import_settings_from'] = 55;

   $model = new \Contao\ModuleModel();
   $model->setRow($moduleData);

   $module = new MadeYourDay\RockSolidSlider\Module\Slider($model);

   echo $module->generate();

The type must be specified so that the correct CSS classes for the slider are included in the
source code; ``55`` is the module ID of the settings.

In the render settings for the output, the attribute is also included and the template
``mm_attr_select_rst_slider`` is selected in the attribute settings.

How to pass parameters dynamically to the template is described in the section above and can be
applied here in the same way.
