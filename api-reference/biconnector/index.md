# BIconnector: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

BIconnector is a Bitrix24 module that provides methods for connecting external data sources and adapting them for the "Analyst Workspace" section.

The module combines three current method groups: connectors, sources, and tables. Together they form a data chain — from setting up a connection to an external service to delivering a dataset for reports. The fourth group, `biconnector.dataset.*`, is deprecated — what to use instead is described below.

{% note warning "" %}

The methods work only in the context of an [application](../../settings/app-installation/index.md) and only with the objects that the application created itself. When called via a webhook, the methods return the `ACCESS_DENIED` error. The `*.fields` methods are an exception: they return the description of the object fields and are available to a webhook

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [BI Builder: Analytics hub](https://helpdesk.bitrix24.com/open/25744889/)

## Connectors

A connector defines the rules of integration with an external system. It stores the addresses of four required endpoints: connection check, table list, table description, and data retrieval. The connector also declares which authorization parameters the user fills in when creating a source.

A connector does not store the values of the authorization parameters — the source sets them. That is why a single connector can serve several sources: for example, a test and a production database that return data through the same endpoints but with different credentials.

To work with connectors, use the [biconnector.connector.*](./connector/index.md) methods. For the set of fields, see the [connector fields](./connector/index.md#fields) table.

## Sources

A source is a working connection created based on a connector.

A source is linked to a connector through `connectorId` and stores the values of the authorization parameters declared by the connector. The set of parameters is arbitrary: the connector itself decides what to request from the user — a token, a login with a password, or a workspace identifier. The connector declares the parameter type as `STRING` or `INT`.

To work with sources, use the [biconnector.source.*](./source/index.md) methods. For the set of fields, see the [source fields](./source/index.md#fields) table.

## Tables

A table is the description of a dataset that the application delivers to the "Analyst Workspace" section.

A table is linked to a source through `sourceId` and defines the data structure for analytics: which columns are available, what types they have, and which of them are visible in reports. Several tables can be created from a single source and used in different reports.

To work with tables, use the [biconnector.table.*](./table/index.md) methods. For the set of fields, see the [table fields description](./table/index.md#table).

{% note info "" %}

Previously, the same objects were created by the `biconnector.dataset.*` family — it is deprecated. If your integration uses it, check the [method mapping table](./dataset/index.md#replacement): for five methods out of seven only the name in the call changes, while `dataset.add` and `dataset.delete` behave differently — the differences are covered there

{% endnote %}

## Getting Started

1. Deploy [four endpoints](./connector/index.md#endpoints) on your side: connection check, table list, table description, and data retrieval. A connector cannot be created without them — the addresses of all four are required.
2. Create a connector with the [biconnector.connector.add](./connector/biconnector-connector-add.md) method: pass the name, the logo, the addresses of all four endpoints, and the codes of the authorization parameters — these are the required fields. The response returns the `id` of the connector.
3. Create a source with the [biconnector.source.add](./source/biconnector-source-add.md) method: pass `connectorId`, the name, and the values of the authorization parameters. On creation, Bitrix24 immediately calls the connection check endpoint — if it does not respond, the source is not created.
4. Create a table with the [biconnector.table.add](./table/biconnector-table-add.md) method. There are five required fields: the `sourceId` of the source, `name`, `externalName`, `externalCode`, and the `fields` column set. Later, the column set is changed with the [biconnector.table.fields.update](./table/biconnector-table-fields-update.md) method.

## How to Check the Set of Fields

The `*.fields` methods return the schema of an object: field name, type, whether it is required, and the read-only, immutable, and multiple flags. The schema is the same in any Bitrix24 and does not depend on what you have already created — use it to check the set of fields, not the current data:

- [biconnector.connector.fields](./connector/biconnector-connector-fields.md) — connector fields
- [biconnector.source.fields](./source/biconnector-source-fields.md) — source fields
- [biconnector.table.fields](./table/biconnector-table-fields.md) — table fields

The response also contains read-only fields — they cannot be passed to `add` or `update`.

{% note tip "User Documentation" %}

- [Request debugging in Bitrix24 BI Connector](https://helpdesk.bitrix24.com/open/17499442/)
- [FAQ: BI analytics and BI Builder](https://helpdesk.bitrix24.com/open/24380778/)

{% endnote %}

## Response Format {#responses}

The forms of a successful response are the same for all three method groups, with one exception:

#|
|| **Methods** | **What is returned** ||
|| `*.add` | `result.id` — the identifier of the created object ||
|| `*.get` | `result.item` — the entire object. The exception is [biconnector.source.get](./source/biconnector-source-get.md): the source's own fields are placed in the nested `result.item.connection` object ||
|| `*.list` | `result` — an array of objects without a wrapper ||
|| `*.update`, `*.delete`, `*.fields.update` | `result: true` ||
|| `*.fields` | `result.fields` — an array with the field descriptions ||
|#

The composition of an object may grow: responses include fields of adjacent scenarios, for example the CSV parsing parameters of a table. Ignore unfamiliar keys instead of treating them as an error.

### Errors Are Returned Inside result {#errors}

{% note warning "" %}

The methods of this section return an error **inside the `result` field** and with HTTP status 200 — unlike the other methods of the Bitrix24 REST API

{% endnote %}

```json
{
    "result": {
        "error": {
            "error": "ACCESS_DENIED",
            "error_description": "Access denied."
        }
    }
}
```

The `time` block is returned as usual in such a response — it cannot be used to tell an error from a success. Check for the error in `result.error`, not in the root of the response. The ready-made SDK wrappers parse only the top level, so they treat such a response as successful: the call returns an object with an `error` field instead of the expected data.

There is one exception to this rule. If the `order` parameter of the list methods receives a sorting direction other than `ASC` and `DESC`, the response is formed not by the module but by a lower level: the request ends with HTTP status 400, and the `ERROR_ARGUMENT` error is placed in the root of the response rather than in `result`.

Error codes common to the entire section:

#|
|| **Code** | **When it is returned** ||
|| `ACCESS_DENIED` | There are three reasons: the user lacks one of the two permissions; the method was called via a webhook; the method was called outside the application context. The `*.fields` methods are available to a webhook, so only the permission reason remains for them ||
|| `CONNECTOR_NOT_FOUND`, `SOURCE_NOT_FOUND`, `DATASET_NOT_FOUND` | The object does not exist or belongs to another application. An application does not see objects of other applications, so "no access" is returned as "not found". The `biconnector.table.*` methods also report a table that was not found with the `DATASET_NOT_FOUND` code: the module has no separate `TABLE_NOT_FOUND` ||
|| Codes of the `VALIDATION_` family | A required field is not filled in, a read-only or immutable field was passed, or an unknown parameter was specified — this is checked by `add`, `update`, and `fields.update`. The `list` methods have their own codes: an invalid type of `select`, `filter`, or `order` and an invalid field inside them ||
|| `BX_ERROR` | A general error of the module. It is returned, for example, when a source cannot be deleted because of the tables linked to it. For a connector, the same case is returned with the separate `CONNECTOR_DELETE_RESTRICTED` code ||
|#

The codes specific to a particular method are listed on its page.

### Pagination in the list Methods {#pagination}

The page size is fixed at 50 items. The page number is passed in the optional `page` parameter of the [`integer`](../data-types.md) type, numbering starts at one, and the default value is 1. The `start` parameter, common to most REST API methods, does not work here and is silently ignored.

The methods return neither the total number of items nor a link to the next page, so automatic traversal of lists is not applicable. There is only one sign that the selection has ended: the page returned fewer than 50 items.

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../scopes/permissions.md)
>
> Who can execute the methods: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

### Connectors

#| 
|| **Method** | **Description** ||
|| [biconnector.connector.add](./connector/biconnector-connector-add.md) | Adds a new connector ||
|| [biconnector.connector.update](./connector/biconnector-connector-update.md) | Updates an existing connector ||
|| [biconnector.connector.get](./connector/biconnector-connector-get.md) | Returns information about the connector ||
|| [biconnector.connector.list](./connector/biconnector-connector-list.md) | Returns a list of available connectors ||
|| [biconnector.connector.delete](./connector/biconnector-connector-delete.md) | Deletes a connector ||
|| [biconnector.connector.fields](./connector/biconnector-connector-fields.md) | Returns the description of the connector fields ||
|#

### Sources

#| 
|| **Method** | **Description** ||
|| [biconnector.source.add](./source/biconnector-source-add.md) | Adds a new source ||
|| [biconnector.source.update](./source/biconnector-source-update.md) | Updates an existing source ||
|| [biconnector.source.get](./source/biconnector-source-get.md) | Returns information about the source ||
|| [biconnector.source.list](./source/biconnector-source-list.md) | Returns a list of available sources ||
|| [biconnector.source.delete](./source/biconnector-source-delete.md) | Deletes a source ||
|| [biconnector.source.fields](./source/biconnector-source-fields.md) | Returns the description of the source fields ||
|#

### Tables

#| 
|| **Method** | **Description** ||
|| [biconnector.table.add](./table/biconnector-table-add.md) | Creates a new table ||
|| [biconnector.table.update](./table/biconnector-table-update.md) | Updates the description of the table ||
|| [biconnector.table.get](./table/biconnector-table-get.md) | Returns the table together with its columns ||
|| [biconnector.table.list](./table/biconnector-table-list.md) | Returns a list of available tables ||
|| [biconnector.table.delete](./table/biconnector-table-delete.md) | Deletes a table ||
|| [biconnector.table.fields.update](./table/biconnector-table-fields-update.md) | Updates the column set of the table ||
|| [biconnector.table.fields](./table/biconnector-table-fields.md) | Returns the description of the table fields ||
|#
## Continue Learning

- [{#T}](./connector/index.md)
- [{#T}](./source/index.md)
- [{#T}](./table/index.md)
- [Example of creating a connector based on B24PHPSDK](https://github.com/bitrix24/b24sdk-examples/tree/main/php/special/biconnector)

