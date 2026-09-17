# Sources: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A source is a working connection to an external system in the BIconnector module. It retains the values of the authorization parameters declared by the connector and links tables to them.

{% note warning "" %}

The methods work only in the context of an [application](../../../settings/app-installation/index.md) and only with the sources that the application created itself. When called via a webhook, the methods return the `ACCESS_DENIED` error. The [biconnector.source.fields](./biconnector-source-fields.md) method is an exception: it returns the field description and is available to a webhook

{% endnote %}

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [BI Builder: Datasets](https://helpdesk.bitrix24.com/open/24553112/)

## How a Source Relates to a Connector and Tables

A source is the middle level in the data hierarchy within the BIconnector module:

- **Connector** retains the endpoint addresses and the codes of the authorization parameters. Methods — [biconnector.connector.*](../connector/index.md)
- **Source** retains the values of these parameters. It is linked to the connector via `connectorId`. Methods — [biconnector.source.*](#all-methods)
- **Table** describes a specific set of data. It is linked to the source via `sourceId`. Methods — [biconnector.table.*](../table/index.md)

The rules common to the entire section — [response forms](../index.md#responses), [error format](../index.md#errors), and [pagination](../index.md#pagination) — are described in the BIconnector module overview.

## Getting Started

1. Create a connector using the [biconnector.connector.add](../connector/biconnector-connector-add.md) method or select an existing connector using the [biconnector.connector.list](../connector/biconnector-connector-list.md) method.
2. Retrieve the codes of the authorization parameters from the connector [`settings` field](../connector/index.md#settings).
3. Create a source using the [biconnector.source.add](./biconnector-source-add.md) method and pass `connectorId`, the name, and the values of the authorization parameters in the `settings` field. On creation, Bitrix24 calls the connector [urlCheck](../connector/index.md#urlCheck) endpoint: if the external system does not respond, the method returns the `SOURCE_CREATE_CONNECTION_ERROR` error and the source is not created.
4. Check the source using the [biconnector.source.get](./biconnector-source-get.md) or [biconnector.source.list](./biconnector-source-list.md) method.

A source can be deleted with the [biconnector.source.delete](./biconnector-source-delete.md) method only when it has no tables left: otherwise the `BX_ERROR` error is returned, requiring the linked tables to be deleted first.

## Description of Source Fields {#fields}

The table describes the composition of the object that the `add` and `update` methods accept in the `fields` parameter. The object identifier is not part of it: the `update`, `get`, and `delete` methods accept it as a separate required `id` parameter.

The "Required" column shows when a field has to be passed in `fields`, "Read" shows whether it is returned in the response, and "Write" shows whether the methods accept it as input. A check mark in the "Write" column does not mean that the field can be changed after creation: the restrictions are given in the field description.

#|
|| **Name**
`type` | **Description** | **Required** | **Read** | **Write** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the source | No | ✅ | ❌ ||
|| **title**
[`string`](../../data-types.md) | Source name that is visible in the Analytics hub interface | On creation and update | ✅ | ✅ ||
|| **type**
[`string`](../../data-types.md) | Source type. For sources created via REST, the value is always `rest` | No | ✅ | ❌ ||
|| **code**
[`string`](../../data-types.md) | Source code. It is generated automatically using the `rest_<connectorId>` template | No | ✅ | ❌ ||
|| **description**
[`string`](../../data-types.md) | Source description | No | ✅ | ✅ ||
|| **active**
[`boolean`](../../data-types.md) | Source activity. A new source is always created active, and the value is not changed via REST: activity is controlled by the toggle in the Analytics hub section. An inactive source stops returning data | No | ✅ | ❌ ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date the source was created | No | ✅ | ❌ ||
|| **dateUpdate**
[`datetime`](../../data-types.md) | Date the source was updated | No | ✅ | ❌ ||
|| **createdById**
[`integer`](../../data-types.md) | Identifier of the user who created the source | No | ✅ | ❌ ||
|| **updatedById**
[`integer`](../../data-types.md) | Identifier of the user who updated the source | No | ✅ | ❌ ||
|| **connectorId**
[`integer`](../../data-types.md) | Identifier of the connector the source is linked to. It is set once and cannot be changed | On creation | ✅ | ✅ ||
|| **settings**
[`array`](../../data-types.md) | [Authorization parameters](#settings) | On creation | ✅ | ✅ ||
|#

The flat list above describes the composition of a source, but in the response of the [biconnector.source.get](./biconnector-source-get.md) method the fields sit at two levels: the source's own fields are in the nested `item.connection` object, while `connectorId` and `settings` are directly in `item`. The path to the source name looks like `result.item.connection.title`, not `result.item.title`.

The [biconnector.source.fields](./biconnector-source-fields.md) method returns `active` as writable, but the `add` and `update` methods do not read the value of this field.

### Settings Field {#settings}

The `settings` field contains the values of the authorization parameters declared by the connector. It is arranged differently in the input and in the response.

**What to pass to `add` and `update`.** An object where the key is the `code` of a parameter from the connector `settings` field. The composition of this field is described in the [Settings Field](../connector/index.md#settings) section of the connector, and the codes can be retrieved with the [biconnector.connector.get](../connector/biconnector-connector-get.md) or [biconnector.connector.list](../connector/biconnector-connector-list.md) method. If the connector declares a parameter with the `token` code, a source is created like this:

```json
{
    "fields": {
        "connectorId": 12,
        "title": "Sales source",
        "settings": {
            "token": "12345"
        }
    }
}
```

Keys that are absent from the connector description are dropped without an error. On update, the settings are merged by key: pass only the parameters that have to be changed — the rest keep their previous values. A parameter disappears from a source in one case only — when the connector stops declaring it in its `settings` field. The setting is then removed the next time the source is saved.

**What is returned in the `get` and `list` response.** An array of objects, one for each connector parameter:

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the saved parameter ||
|| **code**
[`string`](../../data-types.md) | Parameter code set by the connector ||
|| **name**
[`string`](../../data-types.md) | Parameter name that the user sees in the interface ||
|| **type**
[`string`](../../data-types.md) | Parameter type: `STRING` or `INT` ||
|| **value**
[`string`](../../data-types.md) | Value specified when the source was created or updated ||
|#

```json
[
    {
        "id": 42,
        "code": "token",
        "name": "Token",
        "type": "STRING",
        "value": "12345"
    }
]
```

{% note warning "" %}

Parameter values are returned in plain text, including passwords and tokens. There is no filtering by parameter code: a setting with the `password` code is returned in the response just like any other

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the methods: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

#|
|| **Method** | **Description** ||
|| [biconnector.source.add](./biconnector-source-add.md) | Adds a new source ||
|| [biconnector.source.update](./biconnector-source-update.md) | Updates an existing source ||
|| [biconnector.source.get](./biconnector-source-get.md) | Returns information about the source ||
|| [biconnector.source.list](./biconnector-source-list.md) | Returns a list of available sources ||
|| [biconnector.source.delete](./biconnector-source-delete.md) | Deletes a source ||
|| [biconnector.source.fields](./biconnector-source-fields.md) | Returns the description of the source fields ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../connector/index.md)
- [{#T}](../table/index.md)
- [Example of creating a connector based on B24PHPSDK](https://github.com/bitrix24/b24sdk-examples/tree/main/php/special/biconnector)

