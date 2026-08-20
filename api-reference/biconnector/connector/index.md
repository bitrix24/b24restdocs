# Connector: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The connector is a tool for integrating Bitrix24 with external data systems. It is responsible for connecting to third-party APIs and databases, importing data for analytics and reporting.

Each connector contains settings for working with a specific source:
- endpoints — URLs that Bitrix24 uses to reach the external system
- authorization parameters whose values are set by the source

{% note warning "" %}

Methods work only in the context of an [application](../../../settings/app-installation/index.md) and only with the connectors that the application created itself. When called via a webhook, the methods return the `ACCESS_DENIED` error. The exception is [biconnector.connector.fields](./biconnector-connector-fields.md): it returns the field description and is available to a webhook

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [BI Builder: Analytics hub](https://helpdesk.bitrix24.com/open/25744889/)

## Connector's Relationship with Sources and Datasets

The connector is the top level in the data hierarchy within the BIconnector module:
- **Connector** describes how Bitrix24 reaches the external system. Methods — [biconnector.connector.*](#all-methods)
- **Source** defines which specific data is available from the connected service. The source is linked to the connector via `connectorId` and retains the values of the authorization parameters. Methods — [biconnector.source.*](../source/index.md)
- **Dataset** forms the final set of data that can be used in reports and analytics. The dataset is linked to the source via `sourceId`. Methods — [biconnector.dataset.*](../dataset/index.md)

The connector retains only the codes of the authorization parameters in the [settings](#settings) field. The values are set by the source.

## Getting Started

1. Place four [endpoints](#endpoints) on your side: for checking the connection, listing tables, describing a table, and retrieving data
2. Check the list of available fields using [biconnector.connector.fields](./biconnector-connector-fields.md) if you need to know in advance which fields can be passed
3. Create a connector using [biconnector.connector.add](./biconnector-connector-add.md): pass the name, logo, endpoint URLs, and the list of authorization parameters in the [settings](#settings) field. The response returns the connector `id`
4. Create a source using [biconnector.source.add](../source/biconnector-source-add.md): pass the connector `id` in the `connectorId` parameter and the values of the authorization parameters. When the source is created, Bitrix24 calls the [urlCheck](#urlCheck) endpoint
5. Update the connector settings using [biconnector.connector.update](./biconnector-connector-update.md) or delete the connector using [biconnector.connector.delete](./biconnector-connector-delete.md). A connector can be deleted only after all of its sources are deleted

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
[`integer`](../../data-types.md) | Sorting order | ✅ | ✅ ||
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
|| **supportMapping**
[`boolean`](../../data-types.md) | Support for mapping dataset fields to external system fields. The default value is `false` | ✅ | ✅ ||
|| **sourceCode**
[`string`](../../data-types.md) | String code of the external system. A service field, maximum length is 64 characters | ✅ | ✅ ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date of connector creation | ✅ | ❌ ||
|#

### Settings Field {#settings}

The `settings` field contains an array of parameters necessary for configuring data sources. Each parameter is an object with the following structure:

- `code` — parameter code. It is used as the parameter identifier: these are the names under which the parameters are passed to the external system. Maximum length is 512 characters
- `name` — parameter name displayed in the interface, in the Analytics hub section. Maximum length is 512 characters
- `type` — parameter type. The supported values are `STRING` and `INT`, the type determines the input field in the interface

```json
[
    {
        "code": "login",
        "name": "Login",
        "type": "STRING"
    },
    {
        "code": "password",
        "name": "Password",
        "type": "STRING"
    }
]
```

## Field Description Structure {#description}

The methods [biconnector.connector.fields](./biconnector-connector-fields.md), [biconnector.source.fields](../source/biconnector-source-fields.md), and [biconnector.dataset.fields](../dataset/biconnector-dataset-fields.md) return the `fields` array. Each element of the array describes one field of the object:

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Field name ||
|| **type**
[`string`](../../data-types.md) | Field type ||
|| **isRequired**
[`boolean`](../../data-types.md) | Required field ||
|| **isReadOnly**
[`boolean`](../../data-types.md) | Field is read-only ||
|| **isImmutable**
[`boolean`](../../data-types.md) | Field value can only be set once and only when creating a new element. After that, the field value cannot be changed ||
|| **isMultiple**
[`boolean`](../../data-types.md) | Multiple field. If true, values in the field are passed as an array ||
|#

## Connector Endpoints {#endpoints}

The endpoints are hosted on the external system side. Bitrix24 calls them on its own when a user configures the connection or requests data for a report.

Common rules for all four endpoints:

- the request arrives as POST, the request body is in the `application/x-www-form-urlencoded` format. Nested objects are passed in the `connection[login]=user` notation
- the `connection` object carries the authorization parameters: the key is the parameter `code` from the connector [settings](#settings) field, the value is what the user specified when creating the source
- the response must be returned with HTTP status **200**, otherwise Bitrix24 considers the connection unavailable
- the response timeout is 250 seconds
- by default, Bitrix24 does not call addresses on a local network

### urlCheck {#urlCheck}

The `urlCheck` endpoint performs two tasks:

1. Checks the availability of the connection to the external system
2. Checks the authorization parameters specified by the user

Bitrix24 calls the endpoint:

- when creating a new connection
- when editing an existing connection
- when creating a dataset

Request parameters:

- `connection` — object with the source authorization parameters

Request body for a connector with the `login` and `password` parameters:

```
connection[login]=user&connection[password]=secret
```

The response format is flexible, but the body must not be empty — Bitrix24 checks only the fact of a successful response. A verification request can be sent from the interface, in the Analytics hub section.

### urlTableList {#urlTableList}

The `urlTableList` endpoint returns a list of available tables. Bitrix24 calls it when creating a dataset through the interface and passes the search string — the external system selects the tables by it on its own.

Request parameters:

- `searchString` — search string entered by the user. It can be empty
- `connection` — object with the source authorization parameters

Request body:

```
searchString=sales&connection[login]=user&connection[password]=secret
```

The response is an array of objects in `JSON` format:

- `code` — table code in the external system. Bitrix24 retains it as the dataset `externalCode`
- `title` — table name that the user sees when making a selection

```json
[
    {
        "code": "sales_2024",
        "title": "Sales 2024"
    }
]
```

Both fields are required. If `code` or `title` is empty for at least one table, Bitrix24 considers the response invalid and displays an error.

### urlTableDescription {#urlTableDescription}

The `urlTableDescription` endpoint returns a list of fields of a specific table. Bitrix24 calls it when creating a dataset through the interface.

Request parameters:

- `table` — code of the table for which the description is requested. This is the `code` value from the response of the [urlTableList](#urlTableList) endpoint
- `connection` — object with the source authorization parameters

Request body:

```
table=sales_2024&connection[login]=user&connection[password]=secret
```

The response is an array of objects in `JSON` format:

- `code` — field code in the external system
- `name` — field name that the user sees
- `type` — field type. The supported values are `int`, `string`, `double`, `date`, `datetime`. Bitrix24 handles an unknown type as `string`

```json
[
    {
        "code": "deal_id",
        "name": "Deal",
        "type": "int"
    },
    {
        "code": "created_at",
        "name": "Creation date",
        "type": "datetime"
    }
]
```

All three fields are required. If `code`, `name`, or `type` is empty for at least one field, Bitrix24 considers the response invalid and displays an error.

### urlData {#urlData}

The `urlData` endpoint returns data of a specific table.

Bitrix24 calls the endpoint:
- when creating a dataset through the interface
- when synchronizing dataset fields through the interface
- when executing requests to retrieve data for the BI Builder

Request parameters:

- `table` — code of the table for which data is requested
- `select` — list of dataset fields to return. The parameter can be absent from the request
- `filter` — filter by dataset fields. The parameter can be absent from the request
- `limit` — maximum number of rows in the response. The parameter can be absent from the request
- `mapFields` — field mapping: the key is the field code in the external system, the value is the name of this field in the dataset. If `select` is specified, only the selected fields remain in the object
- `connection` — object with the source authorization parameters

Request body:

```
table=sales_2024&limit=100&mapFields[deal_id]=Deal&connection[login]=user&connection[password]=secret
```

The response is an array of arrays in `JSON` format. The first element holds the field codes, the remaining elements are data rows in the same order:

```json
[
    ["deal_id", "created_at"],
    [101, "2024-08-30T12:19:57+02:00"],
    [102, "2024-08-30T13:41:12+02:00"]
]
```

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Analytics hub section

#|
|| **Method** | **Description** ||
|| [biconnector.connector.add](./biconnector-connector-add.md) | Adds a new connector ||
|| [biconnector.connector.update](./biconnector-connector-update.md) | Updates an existing connector ||
|| [biconnector.connector.get](./biconnector-connector-get.md) | Returns information about the connector ||
|| [biconnector.connector.list](./biconnector-connector-list.md) | Returns a list of available connectors ||
|| [biconnector.connector.delete](./biconnector-connector-delete.md) | Deletes a connector ||
|| [biconnector.connector.fields](./biconnector-connector-fields.md) | Returns the description of the connector fields ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../source/index.md)
- [{#T}](../dataset/index.md)
- [Example of creating a connector based on B24PHPSDK](https://github.com/bitrix24/b24sdk-examples/tree/main/php/special/biconnector)