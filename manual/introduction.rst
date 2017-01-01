Introduction to MetaModels
==========================

What are MetaModels?
--------------------

MetaModels is an extension for the Contao CMS. This extension enables you to input a large variety of structured data and display it on your website following different criteria such as list and detail views, filtering, sorting, pagination, multilingualism and many more..

"Structured data" means content, which is usually stored in a database scheme with different tables and relations.
MetaModels supports different types of field types (attributes) as for example text, selects, check boxes, radio buttons, integers/decimal, yes/no fields, file selection etc.

Possible applications for such data content are in the fields of product catalogues, events, menues, adress or employee lists, houses and rental properties, picture galleries or multilingual text/image content.

With MetaModels, the data models can be created completely in the Contao backend. There is no need for you to code, as for a specific extension.
Both the creation of the input masks for the backend as well as the output for the frontend with optional filters belongs to the creation of a MetaModel.

The MetaModels extension features a high flexibility for data input and output and thereby covers a lot of specific needs.
You can find more details in  :ref:`rst_features`.
Also have a look at some `MetaModels show cases <https://now.metamodel.me/en/showcase>`_ or check the `Contao forum <https://community.contao.org/de/showthread.php?40208-Stellt-eure-MetaModel-Websites-vor/>`_ for further show cases introduced in the german forum.


History of MetaModels
---------------------

Metamodels started out as the next generation of the famous Catalog extension.

Over time 'Catalog' developed into a complex extension and offered many possibilities combined with Contao. But unfortunately it became more and more difficult to maintain the code and to implement new functions.

From the experiences we gained with the development of Catalog 1 and Catalog 2, it became clear to us, that we needed to start from scratch.

That's why we developed "MetaModels": a totally new extension influenced by many modern programming paradigms. Our goal was to develop an extension with a flexible and extensible code base.

The current Metamodels version 2.0 is the result of many hours of discussion about what is "the best solution" and hard programming work.

MetaModels in comparison with other tools
-----------------------------------------

MetaModels works well with the division of labour between administrator and editor which means: the administrator or developer creates one or multiple MetaModels with input masks and output functions and the editor(s) can add the content as they are already used to from other areas of the Contao backend. 

The input masks allow you to accurately specify which data has to be entered (or can be entered) and how. The extensions "[dma_elementgenerator]" or "[rocksolid-custom-elements]" also provide similar functions. The difference is that MetaModels will allow you to even display complex data structures and additionally provides you with various functions for output and filtering.

Before starting a new project you might wonder whether it is better to develop your own extension instead of using MetaModels. But there is no general answer to this question, because both solutions will enable you to solve various problems. The following aspects might help you to make your decision:

**Pro developing own extension:** Is it required to develop a product which can be marketed, as for example a commercial extension which can be made available to other Contao users at the push of a button? Then you should consider developing your own specific extension. 
The basic requirements to do so are appropriate skills in PHP programming and knowledge of the Contao API.

**Pro MetaModels:**
In case that you want to implement a very individual solution which can be quickly customised in the Contao backend, MetaModels is certainly a good choice. If you also need specific functions e.g. supporting multilingualism, MetaModels can play fully on its strengths. MetaModels supports users to develop a solution without programming. 
But it should be noted that only with some basic knowledge in PHP, HTML, and SQL databases, you will be able to make fully use of the opportunities provided by MetaModels. 

Resources
----------

* `MetaModels project website <https://now.metamodel.me>`_
* `MetaModels on Github <https://github.com/MetaModels>`_
* `MetaModels manual on Github <https://github.com/MetaModels/docs>`_
* `MetaModels Contao Wiki <http://en.contaowiki.org/MetaModels>`_
* `MetaModels Contao community subforum <https://community.contao.org/en/forumdisplay.php?184-MetaModels>`_
* `MetaModels IRC Channel on freenode #contao.mm <irc://chat.freenode.net/#contao.mm>`_
