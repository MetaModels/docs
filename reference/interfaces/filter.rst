.. _ref_api_interf_filter:

Filter Interfaces
=================

The filter interfaces provide access to filters and filter rules defined in the
backend within a MetaModel.

In addition, further filters can be created programmatically or filter parameters
can be set.


.. _ref_api_interf_filter_filterrule:

IFilterRule Interface
.....................

Current information at: `IFilterRule <https://github.com/MetaModels/core/blob/master/src/Filter/IFilterRule.php>`_

**Interfaces:**

``getMatchingIds()`` |br|
returns all IDs matching the given filter rule


.. _ref_api_interf_filter_filter:

IFilter Interface
.................

Current information at: `IFilter <https://github.com/MetaModels/core/blob/master/src/Filter/IFilter.php>`_

**Interfaces:**

``addFilterRule(IFilterRule $objFilterRule)`` |br|
adds a filter rule to the filter chain

``getMatchingIds()`` |br|
returns all IDs matching the given filter rule

``createCopy()`` |br|
creates a copy of the filter


Examples
........

The "filtering" blocks between "Start" and "End" are alternatives to each other. The
called classes should be imported with a qualified "use" statement.

.. code-block:: php
   :linenos:

   <?php
   // Start
   $modelName = 'mm_employees';
   $factory   = \Contao\System::getContainer()->get('metamodels.factory');
   // alternatively
   //$factory = $this->getContainer()->get('metamodels.factory');
   $model  = $factory->getMetaModel($modelName);
   $filter = $model->getEmptyFilter();

   // Filter by fixed ID (list):
   $idList = [1,2,3];
   $filter->addFilterRule(new \MetaModels\Filter\Rules\StaticIdList($idList));

   // Filter by attribute value:
   $value      = 'marketing';
   $languages  = $model->getAvailableLanguages();
   $attribute  = $model->getAttribute('division');
   $filter->addFilterRule(new \MetaModels\Filter\Rules\SearchAttribute($attribute, $value, $languages));

   // Custom SQL *1:
   $query = \sprintf('SELECT * FROM %s WHERE published = 1', $modelName);
   $filter->addFilterRule(new \MetaModels\Filter\Rules\SimpleQuery($query));
   // Alternative see https://www.doctrine-project.org/projects/doctrine-dbal/en/4.2/reference/data-retrieval-and-manipulation.html
   $query = \sprintf('SELECT * FROM %s WHERE published = ?', $modelName);
   $filter->addFilterRule(new \MetaModels\Filter\Rules\SimpleQuery($query, [1]));

   // Filter with multiple rules:
   // Combine with ConditionAnd() or ConditionOr()
   // Comparison with GreaterThan, LessThan, NotEqual possible
   $attribute        = $model->getAttribute('price');
   $compareInclusive = true;
   $andRule          = new \MetaModels\Filter\Rules\Condition\ConditionAnd();
   $andRule
       ->addRule(new \MetaModels\Filter\Rules\Comparing\GreaterThan($attribute, 10, $compareInclusive)) // >= 10
       ->addRule(new \MetaModels\Filter\Rules\Comparing\LessThan($attribute, 20));                      // < 20
   $filter->addFilterRule($andRule);

   // End
   $items    = $model->findByFilter($filter);
   $arrItems = $items->parseAll('text');
   //dump($arrItems);

*1: Custom SQL can also be built using the
`Doctrine DBAL queryBuilder <https://www.doctrine-project.org/projects/doctrine-dbal/en/4.4/reference/query-builder.html>`_
and passed to SimpleQuery. The queryBuilder allows a query to be assembled elegantly when, for
example, various conditions need to be taken into account. Here is an example:

.. code-block:: php
   :linenos:

   <?php

   use Doctrine\DBAL\Connection;
   use MetaModels\Filter\Rules\SimpleQuery;

   // ...

   $modelName = 'mm_employees';
   $model     = $factory->getMetaModel($modelName);
   $filter    = $model->getEmptyFilter();

   $builder = $this->connection->createQueryBuilder()
               ->select('t.id')
               ->from($metaModel->getTableName(), 't');

   if ($checkUpload) {
       $builder->andWhere('t.upload_allowed = 1');
   }

   $filter = $metaModel->getEmptyFilter();
   $filter->addFilterRule(SimpleQuery::createFromQueryBuilder($builder));
   $items = $metaModel->findByFilter($filter, 'name');


.. |br| raw:: html

   <br />
