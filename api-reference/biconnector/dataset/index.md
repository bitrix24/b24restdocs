# Deprecated biconnector.dataset.* Methods: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "DEPRECATED" %}

Development of the `biconnector.dataset.*` methods has been discontinued. Use [biconnector.table.*](../table/index.md).

{% endnote %}

A dataset is the description of a set of data that an application delivers to BI Builder. It defines the composition of fields, while Bitrix24 requests the data itself from the application through the connector endpoints. The same object is created by the [biconnector.table.*](../table/index.md) methods, and in the interface it is called a table. On this page, the word "dataset" refers to two different objects — [how they differ](#dataset-vs-dataset).

{% note warning "" %}

The methods work only in the context of an [application](../../../settings/app-installation/index.md) and only with the datasets that the application created itself. When called via a webhook, the methods return the `ACCESS_DENIED` error. The [biconnector.dataset.fields](./biconnector-dataset-fields.md) method is an exception: it returns the field description and is available to a webhook

{% endnote %}

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [BI Builder: Datasets](https://helpdesk.bitrix24.com/open/24553112/)

The rules common to the entire section — [response forms](../index.md#responses), [error codes](../index.md#errors), and [pagination](../index.md#pagination) — are described in the BIconnector module overview.

## A REST Dataset and a BI Builder Dataset Are Different Objects {#dataset-vs-dataset}

On this page, the word "dataset" refers to two different objects:

- **REST API dataset** — the object created by the methods of this section. In the Analytics hub interface it is called a table, and the [biconnector.table.*](../table/index.md) methods work with the same object.
- **BI Builder dataset** — the object that reports are built on. It is created on top of a table, and its identifier is stored in the `externalId` field.

The [biconnector.dataset.add](./biconnector-dataset-add.md) method creates both objects at once, while [biconnector.table.add](../table/biconnector-table-add.md) creates only the first one.

## What to Use Instead {#replacement}

No removal date has been announced. Six methods out of seven keep working, and only `biconnector.dataset.delete` does not.

#|
|| **Deprecated method** | **Replacement** ||
|| `biconnector.dataset.add` | [biconnector.table.add](../table/biconnector-table-add.md) ||
|| `biconnector.dataset.update` | [biconnector.table.update](../table/biconnector-table-update.md) ||
|| `biconnector.dataset.get` | [biconnector.table.get](../table/biconnector-table-get.md) ||
|| `biconnector.dataset.list` | [biconnector.table.list](../table/biconnector-table-list.md) ||
|| `biconnector.dataset.delete` | [biconnector.table.delete](../table/biconnector-table-delete.md) ||
|| `biconnector.dataset.fields.update` | [biconnector.table.fields.update](../table/biconnector-table-fields-update.md) ||
|| `biconnector.dataset.fields` | [biconnector.table.fields](../table/biconnector-table-fields.md) ||
|#

The set of parameters, the composition of the response, the validation rules, and the error codes are the same in both families. Only two methods differ:

- `biconnector.dataset.add` additionally creates a dataset for reports in BI Builder, while `biconnector.table.add` creates only a table. A dataset is built on top of a table and is created in the interface, in the table card, in the "Linked datasets" block.
- `biconnector.dataset.delete` does not work: in a Bitrix24 with BI Builder deployed, the call ends with HTTP status 500. Delete tables with the [biconnector.table.delete](../table/biconnector-table-delete.md) method.

There is no need to migrate the data: both families work with the same storage, so the objects created via `biconnector.dataset.*` are visible to the `biconnector.table.*` methods and vice versa. The selection is limited only by the application that created the object.

{% note warning "" %}

If the `externalId` field of an object is filled in, the [biconnector.dataset.add](./biconnector-dataset-add.md) method created a BI Builder dataset together with it. The [biconnector.table.delete](../table/biconnector-table-delete.md) method deletes only the table, so such a dataset is left without one. It can be deleted in the interface, in the Analytics hub section

{% endnote %}

## How a Dataset Relates to a Connector and a Source

A dataset is the last level in the data hierarchy within the BIconnector module:

- **Connector** describes the application endpoints and the codes of the authorization parameters. Methods — [biconnector.connector.*](../connector/index.md)
- **Source** retains the values of these parameters. It is linked to the connector via `connectorId`. Methods — [biconnector.source.*](../source/index.md)
- **Dataset** defines the composition of fields that the application delivers over this connection. It is linked to the source via `sourceId`. Methods — [biconnector.dataset.*](#all-methods)

## Getting Started

The order below describes the work of an integration that is already built on `biconnector.dataset.*`. For a new integration the order is the same, but with the `biconnector.table.*` methods.

1. Create a connector using the [biconnector.connector.add](../connector/biconnector-connector-add.md) method or select an existing connector using the [biconnector.connector.list](../connector/biconnector-connector-list.md) method.
2. Create a source using the [biconnector.source.add](../source/biconnector-source-add.md) method and pass the `connectorId` of the required connector.
3. Create a dataset using the [biconnector.dataset.add](./biconnector-dataset-add.md) method. There are five required fields: the `sourceId` of the source, `name`, `externalName`, `externalCode`, and the `fields` composition.
4. If necessary, change the composition of fields using the [biconnector.dataset.fields.update](./biconnector-dataset-fields-update.md) method.

## Description of Dataset Fields {#dataset}

Below is the composition of the object that the `add` and `update` methods accept in the `fields` parameter. The object identifier is not part of it: the `update`, `get`, `delete`, and `fields.update` methods accept it as a separate required `id` parameter.

The "Required" column shows when a field has to be passed in `fields`, "Read" shows whether it is returned in the response, and "Write" shows whether the methods accept it as input. A check mark in the "Write" column does not mean that the field can be changed after creation: the restrictions are given in the field description.

#|
|| **Name**
`type` | **Description** | **Required** | **Read** | **Write** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the dataset | No | ✅ | ❌ ||
|| **sourceId**
[`integer`](../../data-types.md) | Identifier of the source the dataset is linked to. It is set once and cannot be changed. Of all the methods, only [biconnector.dataset.list](./biconnector-dataset-list.md) returns this field | On creation | ✅ | ✅ ||
|| **type**
[`string`](../../data-types.md) | Dataset type. For datasets created via the REST API, the value is always `rest` | No | ✅ | ❌ ||
|| **name**
[`string`](../../data-types.md) | Dataset name. It cannot be changed. The name must start with a letter and may contain only lowercase Latin letters `a-z`, digits, and the `_` sign. The maximum name length is 230 characters | On creation | ✅ | ✅ ||
|| **description**
[`string`](../../data-types.md) | Dataset description. It is the only field that the [biconnector.dataset.update](./biconnector-dataset-update.md) method changes | No | ✅ | ✅ ||
|| **externalCode**
[`string`](../../data-types.md) | External code of the dataset — the name the application knows it by. It cannot be changed. The maximum length is 512 characters | On creation | ✅ | ✅ ||
|| **externalName**
[`string`](../../data-types.md) | External name of the dataset. It cannot be changed. The maximum length is 512 characters | On creation | ✅ | ✅ ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date the dataset was created, in the `Y-m-d H:i:s` format | No | ✅ | ❌ ||
|| **dateUpdate**
[`datetime`](../../data-types.md) | Date the dataset was updated, in the `Y-m-d H:i:s` format. For a dataset that has never been updated, the value is `null` | No | ✅ | ❌ ||
|| **createdById**
[`integer`](../../data-types.md) | Identifier of the user who created the dataset | No | ✅ | ❌ ||
|| **updatedById**
[`integer`](../../data-types.md) | Identifier of the user who updated the dataset. For a dataset that has never been updated, the value is `0` | No | ✅ | ❌ ||
|| **externalId**
[`integer`](../../data-types.md) | Identifier of the BI Builder dataset that the [biconnector.dataset.add](./biconnector-dataset-add.md) method created together with this object. For objects created with the [biconnector.table.add](../table/biconnector-table-add.md) method, the value is always `0` | No | ✅ | ❌ ||
|| **csvDelimiter**
[`string`](../../data-types.md) | Column delimiter in a CSV file | No | ✅ | ❌ ||
|| **csvEncoding**
[`string`](../../data-types.md) | Encoding of a CSV file | No | ✅ | ❌ ||
|| **csvHasHeaders**
[`boolean`](../../data-types.md) | Indicates that the first row of a CSV file holds the column headers | No | ✅ | ❌ ||
|| **fields**
[`array`](../../data-types.md) | List of [fields](#fields) included in the dataset. Afterwards it is changed with the [biconnector.dataset.fields.update](./biconnector-dataset-fields-update.md) method. Of all the methods, only [biconnector.dataset.get](./biconnector-dataset-get.md) returns this field | On creation | ✅ | ✅ ||
|#

The `csvDelimiter`, `csvEncoding`, and `csvHasHeaders` fields apply to datasets uploaded from a file: for datasets of a REST source they are always empty. These fields are not part of the schema of the [biconnector.dataset.fields](./biconnector-dataset-fields.md) method, so they cannot be passed in `select`, `filter`, or `order`.

{% note info "" %}

The composition of the response may grow: the module returns the entire object, so over time the response includes fields of adjacent scenarios. Unknown keys should be ignored rather than treated as an error

{% endnote %}

### Description of the Fields Field {#fields}

#|
|| **Name**
`type` | **Description** | **Required** | **Read** | **Write** ||
|| **id**
[`integer`](../../data-types.md) | Field identifier | No | ✅ | ❌ ||
|| **datasetId**
[`integer`](../../data-types.md) | Identifier of the dataset the field belongs to | No | ✅ | ❌ ||
|| **type**
[`string`](../../data-types.md) | Data type. It is set once when the field is created. Available types:
`int` — integer
`string` — string
`double` — float, dot separator
`date` — date, format `Y-m-d`
`datetime` — date with time, format `Y-m-d H:i:s`
`money` — monetary value, retained as a number, the currency is not retained
`timezone` — time zone identifier. The offset is not applied to the data of REST sources | On creation | ✅ | ✅ ||
|| **name**
[`string`](../../data-types.md) | Field name. It is set once when the field is created. The name must start with a letter and may contain only uppercase Latin letters `A-Z`, digits, and the `_` sign. The maximum name length is 32 characters | On creation | ✅ | ✅ ||
|| **externalCode**
[`string`](../../data-types.md) | External code of the field — the name the application knows it by. It is set once when the field is created | On creation | ✅ | ✅ ||
|| **visible**
[`boolean`](../../data-types.md) | Field visibility flag. The default value is `true`. A field with `visible = false` remains in the dataset but is not included in the schema that goes to BI Builder. It is the only field attribute that the [biconnector.dataset.fields.update](./biconnector-dataset-fields-update.md) method changes | No | ✅ | ✅ ||
|| **description**
[`string`](../../data-types.md) | Field description. It is not filled in via the REST API and is always returned as an empty string | No | ✅ | ❌ ||
|#

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the methods: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

#|
|| **Method** | **Description** ||
|| [biconnector.dataset.add](./biconnector-dataset-add.md) | Adds a new dataset ||
|| [biconnector.dataset.update](./biconnector-dataset-update.md) | Updates an existing dataset ||
|| [biconnector.dataset.get](./biconnector-dataset-get.md) | Returns information about the dataset ||
|| [biconnector.dataset.list](./biconnector-dataset-list.md) | Returns a list of available datasets ||
|| [biconnector.dataset.delete](./biconnector-dataset-delete.md) | Does not work: delete the dataset with the [biconnector.table.delete](../table/biconnector-table-delete.md) method ||
|| [biconnector.dataset.fields.update](./biconnector-dataset-fields-update.md) | Updates the fields of the dataset ||
|| [biconnector.dataset.fields](./biconnector-dataset-fields.md) | Returns the description of the dataset fields ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../connector/index.md)
- [{#T}](../source/index.md)
- [{#T}](../table/index.md)
- [Example of creating a connector based on B24PHPSDK](https://github.com/bitrix24/b24sdk-examples/tree/main/php/special/biconnector)

