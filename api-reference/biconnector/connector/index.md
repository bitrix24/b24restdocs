# Connectors: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A connector describes how Bitrix24 reaches an external system and pulls data from it for analytics and reports.

Each connector retains the settings for working with the external system:

- endpoints — URLs that Bitrix24 uses to reach the external system
- authorization parameters whose values are set by the source

{% note warning "" %}

Methods work only in the context of an [application](../../../settings/app-installation/index.md) and only with the connectors that the application created itself. When called via a webhook, the methods return the `ACCESS_DENIED` error. The exception is [biconnector.connector.fields](./biconnector-connector-fields.md): it returns the field description and is available to a webhook

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [BI Builder: Analytics hub](https://helpdesk.bitrix24.com/open/25744889/)

## How a Connector Relates to Sources and Tables

The connector is the top level in the data hierarchy within the BIconnector module:

- **Connector** describes how Bitrix24 reaches the external system: the endpoint addresses and the codes, names, and types of the authorization parameters in the [settings](#settings) field. Methods — [biconnector.connector.*](#all-methods)
- **Source** is a working connection to the external system. It is linked to the connector via `connectorId` and retains the values of the authorization parameters. Methods — [biconnector.source.*](../source/index.md)
- **Table** defines the set of columns that the application delivers over this connection. The table is linked to the source via `sourceId`. Methods — [biconnector.table.*](../table/index.md)

The rules common to the entire section — [response forms](../index.md#responses), [error format](../index.md#errors), and [pagination](../index.md#pagination) — are described in the BIconnector module overview.

## Getting Started

1. Place four [endpoints](#endpoints) on your side: for checking the connection, listing tables, describing a table, and retrieving data.
2. Create a connector using the [biconnector.connector.add](./biconnector-connector-add.md) method: pass the name, logo, endpoint URLs, and the list of authorization parameters in the [settings](#settings) field. The response returns the connector `id`.
3. Create a source using the [biconnector.source.add](../source/biconnector-source-add.md) method: pass the connector `id` in the `connectorId` parameter and the values of the authorization parameters. When the source is created, Bitrix24 calls the [urlCheck](#urlCheck) endpoint.

If you need to know in advance which fields the connector accepts, request the schema using the [biconnector.connector.fields](./biconnector-connector-fields.md) method.

The connector settings are changed with the [biconnector.connector.update](./biconnector-connector-update.md) method, and the connector is deleted with the [biconnector.connector.delete](./biconnector-connector-delete.md) method. A connector can be deleted only after all of its sources are deleted: otherwise the method returns the `CONNECTOR_DELETE_RESTRICTED` error.

## Description of Connector Fields {#fields}

The table describes the composition of the object that the `add` and `update` methods accept in the `fields` parameter. The object identifier is not part of it: the `update`, `get`, and `delete` methods accept it as a separate required `id` parameter.

The "Required" column shows when a field has to be passed in `fields`, "Read" shows whether it is returned in the response, and "Write" shows whether the methods accept it as input.

#|
|| **Name**
`type` | **Description** | **Required** | **Read** | **Write** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the connector | No | ✅ | ❌ ||
|| **title**
[`string`](../../data-types.md) | Connector name, maximum length is 512 characters | On creation | ✅ | ✅ ||
|| **logo**
[`string`](../../data-types.md) | Connector logo: an image URL or the image itself as a data URI, for example `data:image/svg+xml;base64,PHN2Zy4uLg==` | On creation | ✅ | ✅ ||
|| **description**
[`string`](../../data-types.md) | Connector description | No | ✅ | ✅ ||
|| **sort**
[`integer`](../../data-types.md) | Sorting order. The default value is 100 | No | ✅ | ✅ ||
|| **urlCheck**
[`string`](../../data-types.md) | [URL for checking the connection](#urlCheck) | On creation | ✅ | ✅ ||
|| **urlData**
[`string`](../../data-types.md) | [URL for retrieving data](#urlData) | On creation | ✅ | ✅ ||
|| **urlTableList**
[`string`](../../data-types.md) | [URL for the table list](#urlTableList) | On creation | ✅ | ✅ ||
|| **urlTableDescription**
[`string`](../../data-types.md) | [URL for the table description](#urlTableDescription) | On creation | ✅ | ✅ ||
|| **settings**
[`array`](../../data-types.md) | [Connector settings](#settings) | On creation | ✅ | ✅ ||
|| **supportMapping**
[`boolean`](../../data-types.md) | Support for mapping the table fields to the fields of the external system. The default value is `false` | No | ✅ | ✅ ||
|| **sourceCode**
[`string`](../../data-types.md) | String code of the external system, maximum length is 64 characters. It does not affect data transfer. In the interface, the `mysql` and `pgsql` values remove the built-in tile with a hint about the corresponding DBMS from the list of sources | No | ✅ | ✅ ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date the connector was created | No | ✅ | ❌ ||
|#

When a connector is updated, there are no required fields: the [biconnector.connector.update](./biconnector-connector-update.md) method accepts any set of fields and changes only the ones that were passed.

### Settings Field {#settings}

The `settings` field contains an array of parameters that the user fills in when creating a source. Each parameter is an object with three keys:

- `code` — parameter code. It is used as the parameter identifier: these are the names under which the parameters are passed to the external system. Maximum length is 512 characters
- `name` — parameter name displayed in the interface, in the Analytics hub section. Maximum length is 512 characters
- `type` — parameter type. The `STRING` and `INT` values are supported, and the type determines the input field in the interface. The value is case-sensitive: a lowercase `string` causes the `VALIDATION_SETTINGS_INVALID_TYPE` error

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

## Connector Endpoints {#endpoints}

The endpoints are hosted on the side of the external system. Bitrix24 calls them itself when the user sets up the connection or requests data for a report. The maximum length of each address is 2048 characters.

An endpoint failure surfaces differently depending on the scenario in which it happened. If the endpoint does not respond while a source is being created, the [biconnector.source.add](../source/biconnector-source-add.md) method returns the `SOURCE_CREATE_CONNECTION_ERROR` error. If the failure happens while working with a table or requesting data for a report, only the user sees the error in the Analytics hub section — the REST response does not contain it.

Common rules for all four endpoints:

- the request arrives as POST, the request body is in the `application/x-www-form-urlencoded` format. Nested objects are passed in the `connection[login]=user` notation
- the `connection` object carries the authorization parameters: the key is the parameter `code` from the connector [settings](#settings) field, the value is what the user specified when creating the source
- the response must be returned with HTTP status **200**, otherwise Bitrix24 considers the connection unavailable
- 250 seconds are allotted for establishing the connection, and a separate, shorter limit of 60 seconds applies to transferring the response body. A long upload breaks off even if the connection was established immediately
- by default, Bitrix24 does not call addresses on a local network

### urlCheck {#urlCheck}

The `urlCheck` endpoint performs two tasks:

- confirms that the connection to the external system is available
- confirms that the authorization parameters specified by the user are correct

Bitrix24 calls the endpoint:

- when creating a new connection
- when editing an existing connection
- when creating a table

Request parameters:

- `connection` — object with the source authorization parameters

Request body for a connector with the `login` and `password` parameters:

```
connection[login]=user&connection[password]=secret
```

The response format is arbitrary, but the body must not be empty — Bitrix24 checks only the fact of a successful response.

A check request can be sent in the interface, in the Analytics hub section.

The endpoint has no separate failure format: to report that the connection does not work, respond with any HTTP status other than 200 or return an empty body. The [biconnector.source.add](../source/biconnector-source-add.md) method then returns the `SOURCE_CREATE_CONNECTION_ERROR` error, and the source is not created.

### urlTableList {#urlTableList}

The `urlTableList` endpoint returns a list of available tables. Bitrix24 calls it when a table is created through the interface and passes a search string. The external system itself performs the selection by this string.

Request parameters:

- `searchString` — search string entered by the user. It can be empty
- `connection` — object with the source authorization parameters

Request body:

```
searchString=sales&connection[login]=user&connection[password]=secret
```

The response is an array of objects in `JSON` format:

- `code` — table code in the external system. Bitrix24 retains it as the `externalCode` of the table
- `title` — table name that the user sees when making a selection
- `id` — optional identifier of the table in the external system. If it is passed, the `externalCode` of the table receives exactly this value, and `code` goes into the table description

```json
[
    {
        "code": "sales_2024",
        "title": "Sales 2024"
    }
]
```

The `code` and `title` fields are required. If at least one table has one of them empty, Bitrix24 treats the response as invalid and shows an error.

### urlTableDescription {#urlTableDescription}

The `urlTableDescription` endpoint returns the list of fields of a specific table. Bitrix24 calls it when a table is created through the interface.

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
- `type` — field type. The `int`, `string`, `double`, `date`, `datetime`, `money`, and `timezone` values are supported. Bitrix24 handles an unknown type as `string`

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

Bitrix24 retains `money` values as a number: everything except digits, the minus sign, and the decimal separator is dropped from the value — the currency is not retained. The `timezone` type contains a time zone identifier, but the offset is not applied to the data of sources created via REST.

All three fields are required. If at least one field has an empty `code`, `name`, or `type`, Bitrix24 treats the response as invalid and shows an error. An empty array is also treated as an invalid response: a table must have at least one field.

### urlData {#urlData}

The `urlData` endpoint returns data of a specific table.

Bitrix24 calls the endpoint:

- when creating a table through the interface
- when synchronizing the table fields through the interface
- when executing requests for data for BI Builder

Request parameters:

- `table` — code of the table for which data is requested
- `select` — array of names of the table fields to return
- `filter` — filter by the table fields: the key is the field name, the value is the selection condition
- `limit` — maximum number of rows in the response, an integer
- `mapFields` — field mapping: the key is the field code in the external system, the value is the name of this field in the table. If `select` is specified, only the selected fields remain in the object
- `connection` — object with the source authorization parameters

The `select`, `filter`, and `limit` parameters do not arrive in every request — the handler must work without them as well.

There will be no other parameters in the request: Bitrix24 passes only the ones listed.

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
> Who can execute the methods: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions


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
- [{#T}](../table/index.md)
- [Example of creating a connector based on B24PHPSDK](https://github.com/bitrix24/b24sdk-examples/tree/main/php/special/biconnector)
