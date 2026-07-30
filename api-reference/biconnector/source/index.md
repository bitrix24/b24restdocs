# Sources: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A source is a separate connection to an external system in the BI connector module. The source defines which specific data from the external system will be available for use in reports and analytics in Bitrix24.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [BI Builder: Datasets](https://helpdesk.bitrix24.com/open/24553112/)

{% note info "" %}

Methods work only in the context of the [application](../../../settings/app-installation/index.md)

{% endnote %}

## Connection of the Source with the Connector and Datasets

The source is registered through the connector. In the hierarchy of the BI connector module, sources occupy an intermediate level:
- **Connector** establishes a connection with an external data source.
- **Source** defines the access parameters for the data.
- **Dataset** forms the final set of data that can be used in reports and analytics.

## Description of Source Fields {#fields}

#|
|| **Name** | **Description** ||
|| **id** | Unique source identifier ||
|| **title** | Source name ||
|| **type** | Source type, value is always equal to `rest` ||
|| **code** | Source code, service field ||
|| **description** | Source description ||
|| **active** | Source activity status ||
|| **dateCreate** | Source create date ||
|| **dateUpdate** | Source update date ||
|| **createdById** | Identifier of the user who created the source ||
|| **updatedById** | Identifier of the user who updated the source ||
|| **connectorId** | Identifier of the connector to which the source is linked ||
|| **settings** | [Authorization parameters list](#settings) ||
|#

### Settings Field {#settings}

The `settings` field contains a list of parameters for authorization via the connector. Parameters are passed in an object format, where the key is the identifier of the `code` parameter.

The `code` values are stored in the `settings` field of the connector. They can be retrieved using the [biconnector.connector.list](../connector/biconnector-connector-list.md) or [biconnector.connector.get](../connector/biconnector-connector-get.md) methods. If the connector contains a parameter with the code `token`, when creating a source, pass the value in the object `settings`:

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

## Before You Begin

1. Create a connector using the [biconnector.connector.add](../connector/biconnector-connector-add.md) method or select an existing connector using the [biconnector.connector.list](../connector/biconnector-connector-list.md) method
2. Retrieve the authorization parameter codes from the `settings` field of the connector
3. Create a source using the [biconnector.source.add](./biconnector-source-add.md) method and pass `connectorId` and the authorization values into the `settings` field
4. Verify the source using the [biconnector.source.get](./biconnector-source-get.md) or [biconnector.source.list](./biconnector-source-list.md) method

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute methods: A user with access to the "Analytics hub" section

#|
|| **Method** | **Description** ||
|| [biconnector.source.add](./biconnector-source-add.md) | Adds a new source ||
|| [biconnector.source.update](./biconnector-source-update.md) | Updates an existing source ||
|| [biconnector.source.get](./biconnector-source-get.md) | Returns information about the source ||
|| [biconnector.source.list](./biconnector-source-list.md) | Returns a list of available sources ||
|| [biconnector.source.delete](./biconnector-source-delete.md) | Deletes a source ||
|| [biconnector.source.fields](./biconnector-source-fields.md) | Returns the description of the source fields ||
|#
