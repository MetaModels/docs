.. _mm_first_contentelements:

Content elements / Modules for front end output
===============================================

After you have configured all the components for data input, you can set up the data output. There are several options available for data output - in this example we will use the article content element "MetaModel list".

First you need to prepare a page in Contao with an article which will contain the content element "MetaModel list". Create this new content element with the following settings:

* Element type: MetaModel list
* Order by: Name
* Filter settings to apply: Published
* Render settings to apply FE list

|img_contentelements_01_en|

Hit "Save and close" and the content element will be available and you can check it in the front end view.

You should see the sentence "There are no items matching your search" in the front end preview, because you didn't enter data yet. 

To test the front end view it is necessary to create some data entries in the employee list. To do this, go to the left menu in the back end and click on the icon "|img_metamodels| Employee list" and then on the icon "|img_new| New item".

You will then see the input screen with the fields you have defined (attributes). Now you can fill them with initial data (see screenshot).

|img_contentelements_02_en|

After you have hit "Save and close" you will see the new data record. Only the attributes that you have activated in your render setting "BE list" ("Name" and "First name") will be visible here.

|img_contentelements_03_en|

You can edit this entry with a click on the "pencil icon" and you can quickly switch the published/unpublished status with the "eye icon".

Now your front end view should look somehow like this (see seenshot):

|img_contentelements_04_en|

Once you have entered some test data the employee list in the back end will look similar to this (see screenshot)

|img_contentelements_05_en|

and it will look similar like this in the front end

|img_contentelements_06_en|

For front end output the attributes are displayed within individual HTML div container elements including specific CSS classes using the standard template. Now you can format the output by using CSS or by customising the standard template, so that the output takes place in form of a HTML table.

By using some CSS rules e.g. such as follows

.. code:: css
	 
	.ce_metamodel_content .item {
	    display: table;
	    width: 100%;
	}  
	.ce_metamodel_content .item.even {
	    background-color: #f4f2f0;
	    border-bottom: 1px solid #d4cbc5;
	    border-collapse: collapse;
	}
	.ce_metamodel_content .item.odd {
	    background-color: #f6f6f6;
	    border-bottom: 1px solid #d4cbc5;
	    border-collapse: collapse;
	}
  .ce_metamodel_content .item .field {
    display: table-cell;
    width: 25%;
  }


the front end view looks better - see screenshot:

|img_contentelements_07_en|

.. |img_new| image:: /_img/icons/new.gif
.. |img_metamodels| image:: /_img/icons/metamodels.png

.. |img_contentelements_01_en| image:: /_img/screenshots/metamodel_first/contentelements_01_en.png
.. |img_contentelements_02_en| image:: /_img/screenshots/metamodel_first/contentelements_02_en.png
.. |img_contentelements_03_en| image:: /_img/screenshots/metamodel_first/contentelements_03_en.png
.. |img_contentelements_04_en| image:: /_img/screenshots/metamodel_first/contentelements_04_en.png
.. |img_contentelements_05_en| image:: /_img/screenshots/metamodel_first/contentelements_05_en.png
.. |img_contentelements_06_en| image:: /_img/screenshots/metamodel_first/contentelements_06_en.png
.. |img_contentelements_07_en| image:: /_img/screenshots/metamodel_first/contentelements_07_en.png