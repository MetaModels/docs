.. _mm_first_conclusion:

Summary and outlook
===================


With the setup of our first MetaModel we have created a simple table and we went through the basic steps of creating a MetaModel.

With the MetaModel "Employee list" we have implemented the input in the back end and the output in the front end. But this is only a small proportion of the possiblities for MetaModels and even this small example can be further enhanced.

Here is a small list of possibilities:
  
* Change the data structure - Put "department" into an own MetaModel and link it       (relation) to "Employee list"
* Add filtering, sorting and search functions to the back end
* Extend the front end with filterings, sorting and search functions
  
Please let the following two screenshots inspire you! The first screenshot is from the back end with a separate MetaModel for the departments. (You'll need to change the attribute "department" from type "Text" to type "Select".)

|img_conclusion_01_en|

This separate MetaModel for the departments will enable you to select from a predefined list in your input mask when you are creating a data record:

|img_conclusion_03_en|

You could also extend the front end view with a filter and a text search field. 
(Here we filter for department "Management" and initial letter "D".)
Additionally here is a customized template in use for the render setting for the front end list ("FE list") to show table headers. 

|img_conclusion_02_en|

In the chapter :ref:`mm_second_index` we will set up a more complex data structure and in the chapter :ref:`mm_special_index` we will cover some aspects such as multilingualism, variants, child tables etc.

.. |img_conclusion_01_en| image:: /_img/screenshots/metamodel_first/conclusion_01_en.png
.. |img_conclusion_02_en| image:: /_img/screenshots/metamodel_first/conclusion_02_en.png
.. |img_conclusion_03_en| image:: /_img/screenshots/metamodel_first/conclusion_03_en.png