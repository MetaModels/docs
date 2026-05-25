.. _rst_cookbook_specials_picker-for-icons:

Icon Picker
===========

If you want to add an icon to records, you can use the File attribute or, when
icons are embedded via a font, the corresponding CSS classes. However, selecting
icons this way is not very user-friendly.

There are various Contao extensions that provide a dedicated icon picker.

To make this functionality available in MM as well, you could create a custom
attribute. Most icon pickers only store strings or serialised arrays, so you can
also use the Text attribute with small DCA and template adjustments.

The following presents the adjustments for common picker extensions:

- :ref:`RockSolid Icon Picker <picker_rst>`
- :ref:`Marco Cupic Font Awesome Icon Picker <picker_mcfa>`
- :ref:`NetGroup IconToolkit <picker_ng>`
- :ref:`Lukas Bableck SVG Icon-Picker <picker_lbsvg>`

Please observe the respective licence terms for the icons and fonts!


Prerequisites
-------------

First, a **Text attribute** must be created, including migration and integration
in the input mask and render settings.

For DCA adjustments, a PHP file ``contao/dca/mm_employees.php`` must be created.
If "LABEL NOT SET" appears instead of the label,
:ref:`please fix it as described <component_translations_lns>`.

The DCA adjustment can be taken from the respective examples — adapt the
MetaModel name (``mm_employees``) and the attribute column name ``*_icon``
(e.g. ``rst_icon``).

For the output, custom templates — derived from ``mm_attr_text`` — must be
created and selected in the render settings for the attribute. Additional CSS
settings for size or colour can also be stored there.


.. _picker_rst:

RockSolid Icon Picker
---------------------

`Extension on Github <https://github.com/madeyourday/contao-rocksolid-icon-picker>`_.

The extension works with its own `icon font <https://github.com/madeyourday/RockSolid-Icon-Font>`_
— the SVG files can be converted with the
`SVG Font Generator <https://github.com/madeyourday/SVG-Icon-Font-Generator>`_
and custom SVG icons can also be added. Users of an
`RST theme <https://rocksolidthemes.com/de/contao-themes>`_ will find the
ready-made font files in the corresponding theme package.

DCA adjustment:

.. code-block:: php
   :linenos:

   <?php
   // contao/dca/mm_employees.php
   $GLOBALS['TL_DCA']['mm_employees']['fields']['rst_icon']['inputType']        = 'rocksolid_icon_picker';
   $GLOBALS['TL_DCA']['mm_employees']['fields']['rst_icon']['eval']['iconFont'] = '/files/themes/iconfont/rocksolid-icons.svg';

Template:

.. code-block:: php
   :linenos:

   <?php
   // templates/mm_attr_text_rst_icon.html5
   <span class="<?= $this->additional_class ?>" data-icon="&#x<?= $this->raw ?>;"></span>

CSS:

.. code-block:: css
   :linenos:

   /* files/themes/css/icons.css */
   /* Icon font */
   @font-face {
     font-family: "RockSolid Icons";
     src: url("../iconfont/rocksolid-icons.woff2") format("woff2"), url("../iconfont/rocksolid-icons.svg") format("svg");
     font-weight: normal;
     font-style: normal;
   }

   /* Icon attribute */
   *[data-icon]:before,
   *[class^="icon-"]:before,
   *[class*=" icon-"]:before {
     font: 100%/1 "RockSolid Icons";
     -webkit-font-smoothing: antialiased;
     font-smoothing: antialiased;
     text-rendering: geometricPrecision;
     text-indent: 0;
     display: inline-block;
     position: relative;
     margin-right: 0.26667em;
   }
   *[data-icon]:before {
     content: attr(data-icon);
   }
   *[data-icon].after:before {
     content: none;
   }
   *[data-icon].after:after {
     font: 100%/1 "RockSolid Icons";
     content: attr(data-icon);
     -webkit-font-smoothing: antialiased;
     font-smoothing: antialiased;
     text-rendering: geometricPrecision;
     text-indent: 0;
     display: inline-block;
     position: relative;
     margin-left: 0.26667em;
   }

Output BE & FE:

|img_rst_01.png|

|img_rst_02.png|


.. _picker_mcfa:

Marco Cupic Font Awesome Icon Picker
------------------------------------

`Extension on Github <https://github.com/markocupic/fontawesome-icon-picker-bundle>`_.

From version 7, no entries in ``config.yaml`` are required for the widget —
however, the icon data is fetched directly from the Fontawesome server. If you
do not want this, you can also embed the files directly on the web server. To do
this, download the icon package from the `website <https://fontawesome.com/download>`_
and unzip it. The folders ``js/``, ``metadata/`` and ``webfonts/`` must be placed
on the web server in a suitable folder under ``files/``.

The configuration then looks like this, for example:

.. code-block:: php
   :linenos:

   # config/config.yaml
   markocupic_fontawesome_icon_picker:
     fontawesome_source_path: 'files/themes/fa7_icons/js/all.min.js'
     fontawesome_allowed_styles:
       - fa-regular
       - fa-solid
       - fa-brands
     fontawesome_meta_file_path: 'files/themes/fa7_icons/metadata/icons.yml'

Depending on the configuration of ``fontawesome_allowed_styles`` and the
available icons, the labels R Regular, S Solid, B Brands, etc. are available as
selection buttons — the order determines the icon style displayed in the widget.

Users of an FA Pro variant should refer to the
`Readme <https://github.com/markocupic/fontawesome-icon-picker-bundle?tab=readme-ov-file#configuration>`_.

For both variants, note that the icon fonts for the frontend must be embedded
separately via CSS.

DCA adjustment:

.. code-block:: php
   :linenos:

   <?php
   // contao/dca/mm_employees.php
   $GLOBALS['TL_DCA']['mm_employees']['fields']['mcfa_icon']['inputType'] = 'fontawesomeIconPicker';

Template:

Since storage is done as a serialised array, both the ``html5`` and ``text``
templates must be created.

.. code-block:: php
   :linenos:

   <?php
   // templates/mm_attr_text_mcfa_icon.html5
   /** After deserialisation, an array with three entries is available, e.g.
   * Array
   * (
   *     [0] => circle-check
   *     [1] => fa-regular
   *     [2] => f058
   * )
   */

   $mcfaData = \Contao\StringUtil::deserialize($this->raw, true);
   ?>
   <i class="<?= $mcfaData[1] ?? '' ?> fa-<?= $mcfaData[0] ?? '' ?><?= $this->additional_class ?>"></i>


.. code-block:: php
   :linenos:

   <?php
   // templates/mm_attr_text_mcfa_icon.text
   <?php
   $mcfaData = \Contao\StringUtil::deserialize($this->raw, true);
   ?>
   <?= $mcfaData[1] ?? '' ?> fa-<?= $mcfaData[0] ?? '' ?>

Output BE & FE:

|img_mcfa_01.png|

|img_mcfa_02.png|


.. _picker_ng:

NetGroup IconToolkit
--------------------

`Extension on Github <https://github.com/netgroupgmbh/contao-icontoolkit>`_.

The extension is designed for `Font Awesome <https://fontawesome.com/>`_ and
ships with version 7.1. It is also possible to load custom icon fonts or a more
recent icon set. A frontend module is provided for embedding the font.

DCA adjustment:

.. code-block:: php
   :linenos:

   <?php
   // contao/dca/mm_employees.php
   use NetGroup\IconToolkit\Classes\Contao\Widgets\IconPickerWidget;

   $GLOBALS['TL_DCA']['mm_employees']['fields']['ng_icon']['inputType'] = IconPickerWidget::TYPE;

Template:

.. code-block:: php
   :linenos:

   <?php
   // templates/mm_attr_text_ng_icon.html5
   <i class="<?= $this->raw ?><?= $this->additional_class ?>"></i>

CSS:

.. code-block:: css
   :linenos:

   /* classes 'fa-2x fa-green' in render settings */
   .fa-green {
     color: #6bb710;
   }

Output BE & FE:

|img_ng_01.png|

|img_ng_02.png|


.. _picker_lbsvg:

Lukas Bableck SVG Icon-Picker
------------------------------

`Extension on Github <https://github.com/lukasbableck/contao-svg-icon-picker-bundle>`_.

The extension is designed for `Font Awesome <https://fontawesome.com/>`_ — but
it is also possible to load custom SVG icons such as
`Lucide <https://lucide.dev/icons/>`_ or `Bootstrap <https://icons.getbootstrap.com>`_.

The icons are output as "real" SVGs, which allows customisation of colours, etc.

DCA adjustment:

.. code-block:: php
   :linenos:

   <?php
   // contao/dca/mm_employees.php
   $GLOBALS['TL_DCA']['mm_employees']['fields']['lbsvg_icon']['inputType']                 = 'svgIconPicker';
   $GLOBALS['TL_DCA']['mm_employees']['fields']['lbsvg_icon']['eval']['sourceDirectory']   = '/files/themes/svg-icons/svgs-full/regular';
   $GLOBALS['TL_DCA']['mm_employees']['fields']['lbsvg_icon']['eval']['metadataDirectory'] = '/files/themes/svg-icons/metadata';

Template:

.. code-block:: php
   :linenos:

   <?php
   // templates/mm_attr_text_lbsvg_icon.html5
   use Contao\System;
   use Lukasbableck\ContaoSVGIconPickerBundle\Twig\Extension;

   $rootDir = System::getContainer()->getParameter('kernel.project_dir');
   $svgTool = new Extension($rootDir);

   $svg = \str_replace('class="', 'class="' . \trim($this->additional_class) . ' ', $svgTool->renderSVG($this->raw));
   ?>

   <?= $svg ?>

CSS:

.. code-block:: css
   :linenos:

   /* classes 'lbsvg_icon lbsvg_green' in render settings */
   svg.lbsvg_icon {
     width: 33px;
     height: 33px;
   }

   svg.lbsvg_green {
     color: #6bb710;
   }


Output BE & FE:

|img_lbsvg_01.png|

|img_lbsvg_02.png|


Notes on Font Awesome
---------------------

The Font Awesome icon pack can be downloaded from the
`website <https://fontawesome.com/download>`_. The "Free" variant includes
Regular, Solid, and Brands. If you want to use the SVG icons, it is recommended
to use the ``svg-full/`` folder — all icons here are square with a corresponding
border.

If Font Awesome CSS is also output in the frontend as with NG IconToolkit, the
corresponding styling classes such as ``fa-2x`` for double size can be specified
in the render settings. An overview of these options can be found in the
`FA documentation <https://docs.fontawesome.com/web/style/style-cheatsheet>`_.


Notes on Lucide Icons
---------------------

From version 5.5, Contao uses icons from the `Lucide <https://lucide.dev/icons/>`_
package in the backend. To use these in the frontend as well, the easiest
approach is the :ref:`SVG Icon-Picker <picker_lbsvg>` extension.

The entire package can be downloaded from Github via
`"Code > Download ZIP" <https://github.com/lucide-icons/lucide/archive/refs/heads/main.zip>`_
and unzipped. The ``icons/`` folder contains all SVG icons and must be placed on
the web server in a suitable folder under ``files``.

The folder must then be specified in the configuration — e.g.

.. code-block:: php
   :linenos:

   <?php
   // contao/dca/mm_employees.php
   $GLOBALS['TL_DCA']['mm_employees']['fields']['lbsvg_icon']['inputType']                 = 'svgIconPicker';
   $GLOBALS['TL_DCA']['mm_employees']['fields']['lbsvg_icon']['eval']['sourceDirectory']   = '/files/themes/lucide-icons';
   //$GLOBALS['TL_DCA']['mm_employees']['fields']['lbsvg_icon']['eval']['metadataDirectory'] = '/files/themes/svg-icons/metadata';

Lucide does not provide the icons automatically as a font. However, the files can
be converted to a font. The SVG files must first be converted from strokes to
fills — e.g. using the
`Iconly tool "Convert SVG Strokes to Fills" <https://iconly.io/tools/svg-convert-stroke-to-fill>`_
or the `npm package "svg-outline-stroke" <https://www.npmjs.com/package/svg-outline-stroke>`_.
The Lucide icon font can then be generated with
`IcoMoon <https://icomoon.io/>`_ or the
`npm package "fantasticon" <https://github.com/tancredi/fantasticon>`_.


Notes on Bootstrap Icons
------------------------

Bootstrap 5 includes its own icon package with SVG files and a font as woff and
woff2.

The package can be downloaded from the
`Bootstrap website <https://icons.getbootstrap.com/#download>`_, unzipped and
placed in a suitable folder under ``files``.

For using the SVG files, the :ref:`SVG Icon-Picker <picker_lbsvg>` extension
can be used — adjust the path for ``sourceDirectory`` in the DCA configuration
accordingly.

If you prefer `font output <https://icons.getbootstrap.com/#icon-font>` in the
frontend, when using the :ref:`SVG Icon-Picker <picker_lbsvg>` extension you can
use the SVG icons for backend selection and adjust the template for the frontend
— e.g.

.. code-block:: php
   :linenos:

   // templates/mm_attr_text_lbsvg_bs5_icon.html5
   <i class="bi bi-<?= basename($this->raw, '.svg') ?><?= $this->additional_class ?>"></i>

Additionally, the BS CSS ``bootstrap-icons.min.css`` must be included for the
frontend — the file is included in the download package.

Icon styling can be adjusted e.g. with the
`"text-*" classes <https://getbootstrap.com/docs/5.3/utilities/colors/#colors>`_ or
`"fs-*" classes <https://getbootstrap.com/docs/5.3/utilities/text/#font-size>`_;
the classes can be entered in the render settings for the attribute. For this,
the standard `Bootstrap CSS <https://getbootstrap.com/docs/5.3/getting-started/download/>`_
must also be included.


.. |img_lbsvg_01.png| image:: /_img/screenshots/cookbook/specials/icon_picker/lbsvg_01.png
.. |img_lbsvg_02.png| image:: /_img/screenshots/cookbook/specials/icon_picker/lbsvg_02.png
.. |img_mcfa_01.png| image:: /_img/screenshots/cookbook/specials/icon_picker/mcfa_01.png
.. |img_mcfa_02.png| image:: /_img/screenshots/cookbook/specials/icon_picker/mcfa_02.png
.. |img_ng_01.png| image:: /_img/screenshots/cookbook/specials/icon_picker/ng_01.png
.. |img_ng_02.png| image:: /_img/screenshots/cookbook/specials/icon_picker/ng_02.png
.. |img_rst_01.png| image:: /_img/screenshots/cookbook/specials/icon_picker/rst_01.png
.. |img_rst_02.png| image:: /_img/screenshots/cookbook/specials/icon_picker/rst_02.png

.. |br| raw:: html

   <br />
