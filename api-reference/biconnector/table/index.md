# Tables: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A table is the description of a set of data that an application delivers to BI Builder. It defines the composition of columns, while Bitrix24 requests the data itself from the application through the connector endpoints.

{% note warning "" %}

The methods work only in the context of an [application](../../../settings/app-installation/index.md) and only with the tables that the application created itself. When called via a webhook, the methods return the `ACCESS_DENIED` error. The [biconnector.table.fields](./biconnector-table-fields.md) method is an exception: it returns the description of the object fields and is available to a webhook

{% endnote %}

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [BI Builder: Datasets](https://helpdesk.bitrix24.com/open/24553112/)

The rules common to the entire section — [response forms](../index.md#responses), [error format](../index.md#errors), and [pagination](../index.md#pagination) — are described in the BIconnector module overview.

## How a Table Relates to a Connector and a Source

A table is the last level in the data hierarchy within the BIconnector module:

- **Connector** describes the application endpoints and the codes of the authorization parameters. Methods — [biconnector.connector.*](../connector/index.md)
- **Source** retains the values of these parameters. It is linked to the connector via `connectorId`. Methods — [biconnector.source.*](../source/index.md)
- **Table** defines the composition of columns that the application delivers over this connection. It is linked to the source via `sourceId`. Methods — [biconnector.table.*](#all-methods)

## A Table and a Dataset Are Different Objects {#table-vs-dataset}

In BI Builder, a table and a dataset are two different objects:

- **Table** — a set of columns linked to a source. It is created by the methods of this section and by the interface of the Analytics hub > Tables section.
- **Dataset** — the object that reports are built on. Datasets are created on top of a table, in its card, in the "Linked datasets" block. One table can have several datasets.

The current REST API methods create only tables — a dataset is created by the user in the BI Builder interface. The deprecated family left one exception: the [biconnector.dataset.add](../dataset/biconnector-dataset-add.md) method creates both objects at once, and such tables have the `externalId` field filled in.

## Getting Started

1. Create a connector using the [biconnector.connector.add](../connector/biconnector-connector-add.md) method or select an existing connector using the [biconnector.connector.list](../connector/biconnector-connector-list.md) method.
2. Create a source using the [biconnector.source.add](../source/biconnector-source-add.md) method and pass the `connectorId` of the required connector.
3. Create a table using the [biconnector.table.add](./biconnector-table-add.md) method. There are five required fields: the `sourceId` of the source, `name`, `externalName`, `externalCode`, and the `fields` column set.
4. If necessary, change the composition of columns using the [biconnector.table.fields.update](./biconnector-table-fields-update.md) method.

## Description of Table Fields {#table}

Below is the composition of the object that the `add` and `update` methods accept in the `fields` parameter. The object identifier is not part of it: the `update`, `get`, `delete`, and `fields.update` methods accept it as a separate required `id` parameter.

The "Required" column shows when a field has to be passed in `fields`, "Read" shows whether it is returned in the response, and "Write" shows whether the methods accept it as input. A check mark in the "Write" column does not mean that the field can be changed after creation: the restrictions are given in the field description.

#|
|| **Name**
`type` | **Description** | **Required** | **Read** | **Write** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the table | No | ✅ | ❌ ||
|| **sourceId**
[`integer`](../../data-types.md) | Identifier of the source the table is linked to. It is set once and cannot be changed. Of all the methods, only [biconnector.table.list](./biconnector-table-list.md) returns this field | On creation | ✅ | ✅ ||
|| **type**
[`string`](../../data-types.md) | Table type. For tables created via the REST API, the value is always `rest` | No | ✅ | ❌ ||
|| **name**
[`string`](../../data-types.md) | Table name. It cannot be changed. The name must start with a letter and may contain only lowercase Latin letters `a-z`, digits, and the `_` sign. The maximum name length is 230 characters | On creation | ✅ | ✅ ||
|| **description**
[`string`](../../data-types.md) | Table description. It is the only field that the [biconnector.table.update](./biconnector-table-update.md) method changes | No | ✅ | ✅ ||
|| **externalCode**
[`string`](../../data-types.md) | External code of the table — the name the application knows the table by. It cannot be changed. The maximum length is 512 characters | On creation | ✅ | ✅ ||
|| **externalName**
[`string`](../../data-types.md) | External name of the table. It cannot be changed. The maximum length is 512 characters | On creation | ✅ | ✅ ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date the table was created, in the `Y-m-d H:i:s` format | No | ✅ | ❌ ||
|| **dateUpdate**
[`datetime`](../../data-types.md) | Date the table was updated, in the `Y-m-d H:i:s` format. For a table that has never been updated, the value is `null` | No | ✅ | ❌ ||
|| **createdById**
[`integer`](../../data-types.md) | Identifier of the user who created the table | No | ✅ | ❌ ||
|| **updatedById**
[`integer`](../../data-types.md) | Identifier of the user who updated the table. For a table that has never been updated, the value is `0` | No | ✅ | ❌ ||
|| **externalId**
[`integer`](../../data-types.md) | Identifier of the BI Builder dataset created together with the object by the [biconnector.dataset.add](../dataset/biconnector-dataset-add.md) method. Datasets that the user built on the table themselves do not get here. For tables created with the [biconnector.table.add](./biconnector-table-add.md) method, the value is always `0` | No | ✅ | ❌ ||
|| **csvDelimiter**
[`string`](../../data-types.md) | Column delimiter in a CSV file | No | ✅ | ❌ ||
|| **csvEncoding**
[`string`](../../data-types.md) | Encoding of a CSV file | No | ✅ | ❌ ||
|| **csvHasHeaders**
[`boolean`](../../data-types.md) | Indicates that the first row of a CSV file holds the column headers | No | ✅ | ❌ ||
|| **fields**
[`array`](../../data-types.md) | List of [columns](#fields) included in the table. Afterwards it is changed with the [biconnector.table.fields.update](./biconnector-table-fields-update.md) method. Of all the methods, only [biconnector.table.get](./biconnector-table-get.md) returns this field | On creation | ✅ | ✅ ||
|#


The `csvDelimiter`, `csvEncoding`, and `csvHasHeaders` fields apply to tables uploaded from a file: for tables of a REST source, `csvDelimiter` and `csvEncoding` arrive as empty strings and `csvHasHeaders` arrives as `false`. These fields are not part of the schema of the [biconnector.table.fields](./biconnector-table-fields.md) method, so they cannot be passed in `select`, `filter`, or `order`.

{% note info "" %}

The composition of the response may grow: the module returns the entire object, so over time the response includes fields of adjacent scenarios. Unknown keys should be ignored rather than treated as an error

{% endnote %}

### Description of the Fields Field {#fields}

#|
|| **Name**
`type` | **Description** | **Required** | **Read** | **Write** ||
|| **id**
[`integer`](../../data-types.md) | Column identifier | No | ✅ | ❌ ||
|| **datasetId**
[`integer`](../../data-types.md) | Identifier of the table the column belongs to. The key is named this way for historical reasons | No | ✅ | ❌ ||
|| **type**
[`string`](../../data-types.md) | Data type. It is set once when the column is created. Available types:
`int` — integer
`string` — string
`double` — float, dot separator
`date` — date, format `Y-m-d`
`datetime` — date with time, format `Y-m-d H:i:s`
`money` — monetary value, retained as a number, the currency is not retained
`timezone` — time zone identifier. The offset is not applied to the data of REST sources | On creation | ✅ | ✅ ||
|| **name**
[`string`](../../data-types.md) | Column name. It is set once when the column is created. The name must start with a letter and may contain only uppercase Latin letters `A-Z`, digits, and the `_` sign. The maximum name length is 32 characters | On creation | ✅ | ✅ ||
|| **externalCode**
[`string`](../../data-types.md) | External code of the column — the name the application knows the column by. It is set once when the column is created | On creation | ✅ | ✅ ||
|| **visible**
[`boolean`](../../data-types.md) | Column visibility flag. The default value is `true`. A column with `visible = false` remains in the table but is not included in the schema that goes to BI Builder. It is the only column attribute that the [biconnector.table.fields.update](./biconnector-table-fields-update.md) method changes | No | ✅ | ✅ ||
|| **description**
[`string`](../../data-types.md) | Column description. It is not filled in via the REST API and is always returned as an empty string | No | ✅ | ❌ ||
|#

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the methods: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

#|
|| **Method** | **Description** ||
|| [biconnector.table.add](./biconnector-table-add.md) | Creates a new table ||
|| [biconnector.table.update](./biconnector-table-update.md) | Updates the description of the table ||
|| [biconnector.table.get](./biconnector-table-get.md) | Returns the table together with its columns ||
|| [biconnector.table.list](./biconnector-table-list.md) | Returns a list of available tables ||
|| [biconnector.table.delete](./biconnector-table-delete.md) | Deletes a table ||
|| [biconnector.table.fields.update](./biconnector-table-fields-update.md) | Updates the column set of the table ||
|| [biconnector.table.fields](./biconnector-table-fields.md) | Returns the description of the table fields ||
|#

The same objects were created by the deprecated `biconnector.dataset.*` family. If your integration uses it, check the [method mapping table](../dataset/index.md#replacement): for five methods out of seven only the name in the call changes, while `dataset.add` and `dataset.delete` behave differently — the differences are covered there.


## Continue Learning

- [{#T}](../index.md)
- [{#T}](../connector/index.md)
- [{#T}](../source/index.md)
- [Example of creating a connector based on B24PHPSDK](https://github.com/bitrix24/b24sdk-examples/tree/main/php/special/biconnector)

