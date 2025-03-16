# Contributing to the MM manual

Contributing to the MM manual is very welcome. The data will be managed here on Github and after approval
automatically at [Readthedocs](https://about.readthedocs.com) to an HTML page as
[MM-Handbook](https://metamodels.readthedocs.io/en/latest/index.html).

The easiest way to contribute to the manual is via a Github account with which changes or completely new
pages as a ‘pull request’ (PR).

For minor changes to the page displayed in the manual, you can follow the link ‘Edit on GitHub’ at the top right
and edit the text directly in the browser and create a PR.

For more extensive changes or completely new articles, it is recommended to create a fork of the manual
and edit the clone locally. Feel free to take a look at an existing file - you can usually see the structure
and formatting quite well.

Alternatively, you can also send your article to mail@metamodels.me.

The texts are labelled in [reStructuredText](https://en.wikipedia.org/wiki/ReStructuredText), which is similar to
[Markdown](https://en.wikipedia.org/wiki/Markdown). The conversion on the page Readthedocs
page is performed by the tool ‘[Sphinx](https://www.sphinx-doc.org/)’. If you wish, you can also install Sphinx locally
and convert the entire manual into the desired format such as HTML, EPUB, PDF, etc.

## Notes on writing the texts

The text should address the reader in a neutral way - usually with ‘man’.

Please avoid nested sentences and divide longer paragraphs into logical blocks.

After writing a new article, ‘re-click’ the instructions yourself - this will help you find gaps and errors
in the process.

### Headlines:

```
H1 Headline
===========

H2 Headline
-----------

H3 Headline
...........
```
 
### Images

Images are in the folder ``_img/screenshots/..``

Insert in the text via ‘replacement token’ e.g. Lorem ipsum ``` |img_multi-textfilter_01| ``` bla bla...

and at the bottom of the page

``` .. |img_multi-textfilter_01| image:: /_img/screenshots/cookbook/filter/multi-textfilter_01.jpg ```

### Code

Inline: as `` :code:`das ist mein code` ``

Block:

```
.. code-block:: php
   :linenos:

   // Redirect if data empty.
   if (!count($this->data)) {
       $pageId  = 42; // Page id 
       $page    = \Contao\PageModel::findByPK($pageId);
       $pageURL = $page->getFrontendUrl();
       \Contao\Controller::redirect($pageURL);
   }
```

Please note that the first indentation is three spaces. When specifying ‘code-block’, other specifications are also possible
such as css, yaml, xml are also possible.

### Links

above the heading to be linked
``` .. _rst_features: ```

insert as link e.g. via
``` :ref:`rst_features` ```

or with your own link text
``` :ref:`Neue Funktionen <rst_features>` ```


external links:
``` `Contao <https://www.contao.org>`_ ```
obviously there is no way to specify the ‘target’ attribute...
