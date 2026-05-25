.. _ref_api_interf_attribute:

Attribute Interfaces
====================

.. warning:: Still under construction!

The attribute interfaces provide access to attributes — i.e. the columns of the
MetaModel table — for setting and reading values or querying information.


.. _ref_api_interf_attribute_iattributefactory:

IAttributeFactory Interface
...........................

The IAttributeFactory interface is the "factory interface" for querying an attribute.

Current information at: `IAttributeFactory <https://github.com/MetaModels/core/blob/master/src/Attribute/IAttributeFactory.php>`_

**Interfaces:**

``createAttribute($arrInformation, $objMetaModel)`` |br|
returns the attribute instance for a given MetaModel and an array of attribute defaults

``addTypeFactory(IAttributeTypeFactory $typeFactory)`` |br|
adds a "type factory" to the given factory

``getTypeFactory($typeFactory)`` |br|
returns the "type factory" for the given factory

``attributeTypeMatchesFlags($factory, $intFlags)`` |br|
checks the attribute against flags to compare

``getTypeNames($varFlags = false)`` |br|
returns the registered type names of the factory

``collectAttributeInformation(IMetaModel $objMetaModel)`` |br|
returns all attribute information for a MetaModel

``createAttributesForMetaModel($objMetaModel)`` |br|
returns all attribute instances for a MetaModel

``getIconForType($strType)`` |br|
returns the icon for a given type name


.. _ref_api_interf_attribute_iattribute:

IAttribute Interface
....................

The IAttribute interface is the fundamental interface for attributes.

Current information at: `IAttributeFactory <https://github.com/MetaModels/core/blob/master/src/Attribute/IAttribute.php>`_

**Interfaces:**

``getName()`` |br|
returns the (human-readable) name or title of an attribute

``getColName()`` |br|
returns the column name of an attribute

``getMetaModel()`` |br|
returns the MetaModel instance of an attribute

``get($strKey)`` |br|
returns the meta information of an attribute for the given key

``set($strKey, $varValue)`` |br|
sets the meta information of an attribute for the given key

``handleMetaChange($strMetaName, $varNewValue)`` |br|
replaces the meta information of an attribute for the given key

``initializeAUX()`` |br|
creates all auxiliary data for an attribute in other tables

``destroyAUX()`` |br|
deletes all auxiliary data for an attribute in other tables

``getAttributeSettingNames()`` |br|
returns all valid setting names

``getFieldDefinition($arrOverrides = array())`` |br|
returns a DCA like "$GLOBALS['TL_DCA']['tablename']['fields']['attribute-name]"
with an optional array of parameters to override

``valueToWidget($varValue)`` |br|
returns a widget-compatible value from a native attribute value

``widgetToValue($varValue, $intItemId)`` |br|
returns an attribute-compatible value from a native widget value

``setDataFor($arrValues)`` |br|
saves the values in the schema "id => value" to the database

``getDefaultRenderSettings()`` |br|
returns the instance of the default render settings for the attribute

``parseValue($arrRowData, $strOutputFormat = 'text', $objSettings = null)`` |br|
returns the converted data for the given output format

``getFilterUrlValue($varValue)`` |br|
returns attribute values after use in a filter URL

``sortIds($strListIds, $strDirection)`` |br|
returns an array of IDs sorted by the given sort direction ("ASC|DESC")

``getFilterOptions($strListIds, $usedOnly, &$arrCount = null)`` |br|
returns attributes in the schema "id => value"

``searchFor($strPattern)`` |br|
returns all items matching a search pattern (e.g. wildcard * or ? for one character)

``filterGreaterThan($varValue, $blnInclusive = false)`` |br|
returns a list of item IDs whose value is greater than the given value;
if the "inclusive" option is set, items equal to the value are also included

``filterLessThan($varValue, $blnInclusive = false)`` |br|
returns a list of item IDs whose value is less than the given value;
if the "inclusive" option is set, items equal to the value are also included

``filterNotEqual($varValue)`` |br|
returns a list of item IDs whose value is not equal to the given value

``modelSaved($objItem)`` |br|
called when a given item is saved


.. _ref_api_interf_attribute_isimple:

ISimple Interface
.................

The ISimple interface is for all "simple" attributes that can be retrieved via the
simple method "SELECT colName FROM mm_table".

Current information at: `ISimple <https://github.com/MetaModels/core/blob/master/src/Attribute/ISimple.php>`_

**Interfaces:**

``getSQLDataType`` |br|
returns the SQL type declaration, e.g. "text NULL"

``createColumn()`` |br|
creates the basic database structure for a given attribute

``deleteColumn()`` |br|
deletes the basic database structure for a given attribute

``renameColumn($strNewColumnName)`` |br|
renames the basic database structure for a given attribute;
Note: existing data in the database will be deleted

``unserializeData($strValue)`` |br|
returns the raw database data unserialized

``serializeData($strValue)`` |br|
returns the data serialized for the database


.. _ref_api_interf_attribute_icomplex:

IComplex Interface
..................

The IComplex interface is for all "complex" attributes that cannot be retrieved via
the simple method "SELECT colName FROM mm_table".

Current information at: `IComplex <https://github.com/MetaModels/core/blob/master/src/Attribute/IComplex.php>`_

**Interfaces:**

``getDataFor($arrIds)`` |br|
returns the values for the given IDs as "id => 'native data'",
where "native data" depends on the respective attribute type

``unsetDataFor($arrIds)`` |br|
deletes the attribute values for the given array of IDs


.. _ref_api_interf_attribute_itranslated:

ITranslated Interface
.....................

The ITranslated interface is for all translated attributes.

Current information at: `ITranslated <https://github.com/MetaModels/core/blob/master/src/Attribute/ITranslated.php>`_

**Interfaces:**

``searchForInLanguages($strPattern, $arrLanguages = array())`` |br|
returns the IDs of items found by the given search pattern (including wildcards)
and the optional array of languages

``setTranslatedDataFor($arrValues, $strLangCode)`` |br|
sets the value for an item in the corresponding language

``getTranslatedDataFor($arrIds, $strLangCode)`` |br|
returns an array of values for the items of the ID array in the corresponding language

``unsetValueFor($arrIds, $strLangCode)`` |br|
deletes the values for the array of item IDs in the corresponding language

.. |br| raw:: html

   <br />
