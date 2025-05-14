# Connector: Overview of Methods

The connector is a tool for integrating Bitrix24 with external data systems. It is responsible for connecting to third-party APIs and databases, importing data for analytics and reporting.

Each connector contains settings for working with a specific source:
- endpoints — URLs for sending HTTP requests,
- authorization parameters.

You can register a connector through the [application](../../app-installation/index.md).

> Quick navigation: [all methods](#all-methods)

## Connector's Relationship with Sources and Datasets

The connector is the top level in the data hierarchy within the BIconnector module:
- **Connector** establishes a connection with an external data source.
- **Source** defines which specific data is available from the connected service.
- **Dataset** forms the final set of data that can be used in reports and analytics.

## Description of Connector Fields {#fields}

#|
|| **Name**
`type` | **Description** | Read | Write ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the connector | ✅ | ❌ ||
|| **title**
[`string`](../../data-types.md) | Name of the connector | ✅ | ✅ ||
|| **logo**
[`string`](../../data-types.md) | URL of the logo or base64 string | ✅ | ✅ ||
|| **description**
[`string`](../../data-types.md) | Description of the connector | ✅ | ✅ ||
|| **sort**
[`integer`](../../data-types.md) | Sort order | ✅ | ✅ ||
|| **urlCheck**
[`string`](../../data-types.md) | [URL for connection check](#urlCheck) | ✅ | ✅ ||
|| **urlData**
[`string`](../../data-types.md) | [URL for data retrieval](#urlData) | ✅ | ✅ ||
|| **urlTableList**
[`string`](../../data-types.md) | [URL for table list](#urlTableList) | ✅ | ✅ ||
|| **urlTableDescription**
[`string`](../../data-types.md) | [URL for table description](#urlTableDescription) | ✅ | ✅ ||
|| **settings**
[`array`](../../data-types.md) | [Connector settings](#settings) | ✅ | ✅ ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date of connector creation | ✅ | ❌ ||
|#

### Settings Field {#settings}

The `settings` field contains an array of parameters necessary for configuring data sources. Each parameter is an object with the following structure:

- `code` — parameter code, used as the identifier for the parameter, which is how parameters are passed to the external source.

- `name` — parameter name that will be displayed in the interface, in the "Analyst Workspace" section.

- `type` — parameter type, currently only `STRING` is supported.

```
{
    "code": "code_value",
    "name": "name_value",
    "type": "type_value"
}
```

### Connector Endpoints

#### urlCheck {#urlCheck}

The `urlCheck` endpoint performs two key functions:

1. Checks the availability of the connection to the external source.
2. Authenticates the user.

The endpoint is called:

- when creating a new connection,
- when editing an existing connection,
- when creating a dataset.

A POST request is sent to the endpoint address with the parameters:

- `setting_code_n` — code of the connector's settings parameter.
- `settings_value_n` — value of this parameter related to a specific source.

```
{
  "connection": {
    "setting_code_1": "settings_value_1",
    ...,
    "setting_code_n": "settings_value_n"
  }
}
```

Depending on availability, the endpoint returns information about the status of the source.

#### urlTableList {#urlTableList}

The `urlTableList` endpoint returns a list of available tables in `JSON` format. The endpoint will return tables that match the search query.

The endpoint is called:

- when creating a dataset,
- when used in the interface.

A POST request is sent to the endpoint address with the parameters:

- `searchString` — value of the search string, searches tables by `externalCode`.
- `setting_code_n` — code of the connector's settings parameter.
- `settings_value_n` — value of this parameter related to a specific source.

```
{
  "searchString": "search_value",
  "connection": {
    "setting_code_1": "settings_value_1",
    ...,
    "setting_code_n": "settings_value_n"
  }
}
```

The response returns an array of objects corresponding to the request, with the structure:

- `code` — name of the dataset that will be used in Bitrix24.
- `title` — name of the dataset in the external source. The search is performed based on this field.

```
	{
	    'code' => 'dataset_name',
	    'title' => 'externalName'
	}
```

#### urlTableDescription {#urlTableDescription}

The `urlTableDescription` endpoint returns a list of fields for a specific table in `JSON` format.

The endpoint is called:
- when creating a dataset,
- when used in the interface.

A POST request is sent to the endpoint address with the parameters:

- `table` — `externalCode` of the dataset for which the description is requested.

- `setting_code_n` — code of the connector's settings parameter.

- `settings_value_n` — value of this parameter related to a specific source.

```
{
  "table": "entityName",
  "connection": {
    "setting_code_1": "settings_value_1",
    ...,
    "setting_code_n": "settings_value_n"
  }
}
```

The response returns an array of fields with the structure:
- `code` — code of the dataset field.

- `title` — name of the dataset field.

- `type` — type of the field, supported values are: `int`, `string`, `double`, `date`, `datetime`.

```
  {
      'code' => "code_value",
      'title' => "title_value",
      'type' => "type_value"
  }
```

#### urlData {#urlData}

The `urlData` endpoint returns data from a specific table in `JSON` format.

The endpoint is called:
- when creating a dataset,
- when synchronizing dataset fields,
- when used in the interface.

A POST request is sent to the endpoint address with the parameters:

- `select` — selection of dataset fields.
- `filter` — filter by dataset fields.
- `limit` — limit of dataset selection.
- `table` — `externalCode` of the dataset for which data is requested.
- `setting_code_n` — code of the connector's settings parameter.
- `settings_value_n` — value of this parameter related to a specific source.

```
{
  "select": **array**,
  "filter": **object**,
  "limit": **int**,
  "table": "entityName",
  "connection": {
    "setting_code_1": "settings_value_1",
    ...,
    "setting_code_n": "settings_value_n"
  }
}
```

The response returns an array of fields with the structure:

- `FIELD_N` — field code.
- `VALUE_ROW_M_N` — values of field `N` in row `M`.

```
[
["FIELD_1","FIELD_2",...,"FIELD_N"],
[VALUE_ROW_1_1,VALUE_ROW_1_2,...,VALUE_ROW_1_N,],
[VALUE_ROW_2_1,VALUE_ROW_2_2,...,VALUE_ROW_2_N,]
 ...
,[VALUE_ROW_M_1,VALUE_ROW_M_2,...,VALUE_ROW_M_N,]
]
```

## Parameters of the biconnector_rest_field_description Field {#description}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Name of the field ||
|| **type**
[`string`](../../data-types.md) | Type of the field ||
|| **isRequired**
[`boolean`](../../data-types.md) | Required field ||
|| **isReadOnly**
[`boolean`](../../data-types.md) | Field is read-only ||
|| **isImmutable**
[`boolean`](../../data-types.md) | Field value can only be set once and only when creating a new element. After that, the field value cannot be changed ||
|| **isMultiple**
[`boolean`](../../data-types.md) | Multiple field. If true, values in the field are passed as an array ||
|#

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can perform methods: user with access to the "Analyst Workspace" section

#|
|| **Method** | **Description** ||
|| [biconnector.connector.add](./biconnector-connector-add.md) | Adds a new connector ||
|| [biconnector.connector.update](./biconnector-connector-update.md) | Updates an existing connector ||
|| [biconnector.connector.get](./biconnector-connector-get.md) | Returns information about the connector ||
|| [biconnector.connector.list](./biconnector-connector-list.md) | Returns a list of available connectors ||
|| [biconnector.connector.delete](./biconnector-connector-delete.md) | Deletes a connector ||
|| [biconnector.connector.fields](./biconnector-connector-fields.md) | Returns a description of the connector fields ||
|#