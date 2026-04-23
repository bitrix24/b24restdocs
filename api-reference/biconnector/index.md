# BIconnector: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

BIconnector is a Bitrix24 module that provides methods for connecting external data sources and adapting them for the "Analyst Workspace" section.

In the BIconnector section, there are three groups of methods available: connectors, sources, and datasets. They form a unified workflow for working with data, from setting up the connection to the external service to obtaining a dataset for reporting.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [BI Builder: Analytics hub](https://helpdesk.bitrix24.com/open/25744889/)

{% note info "" %}

Methods only work in the context of the [application](../../settings/app-installation/index.md).

{% endnote %}

## Connectors

A connector defines the rules for integration with an external system. It specifies which endpoints to use to check access, retrieve table structures, and load data. The connector also indicates which authorization parameters need to be requested from the source.

The connector does not store a connection to a specific database or API. It acts as a template that can be reused for multiple sources of the same type. For example, a single MySQL connector can connect to several different MySQL databases.

To work with connectors, use the methods [biconnector.connector.*](./connector/index.md).

## Sources

A source is a working connection created based on a connector.

The source is linked to the connector via `connectorId` and stores specific values for connection parameters: URL, login, password, and other settings described in the connector. This allows one connector to be used to create multiple sources with different parameters, such as for test and production databases.

To work with sources, use the methods [biconnector.source.*](./source/index.md).

## Datasets

A dataset is a description of the data set that is transferred from the source to the "Analyst Workspace."

The dataset is linked to the source via `sourceId` and defines the data structure for analytics: which fields are available, their types, and how the data can be requested. From one source, separate datasets can be created for use in different reports.

To work with datasets, use the methods [biconnector.dataset.*](./dataset/index.md).

{% note tip "User Documentation" %}

- [BI Builder: Datasets](https://helpdesk.bitrix24.com/open/24553112/)

{% endnote %}

## Getting Started

1. Create a connector using the method [biconnector.connector.add](./connector/biconnector-connector-add.md) to set the integration parameters and the URL of the external endpoints.
2. Create a source using the method [biconnector.source.add](./source/biconnector-source-add.md) and pass the `connectorId` of the desired connector.
3. Create a dataset using the method [biconnector.dataset.add](./dataset/biconnector-dataset-add.md), then configure its fields via [biconnector.dataset.fields.update](./dataset/biconnector-dataset-fields-update.md).

## Verification and Debugging

To check the current structure at each level, use the `*.fields` methods:

- [biconnector.connector.fields](./connector/biconnector-connector-fields.md) — which fields can be passed to the connector
- [biconnector.source.fields](./source/biconnector-source-fields.md) — which fields are expected for the source
- [biconnector.dataset.fields](./dataset/biconnector-dataset-fields.md) — description of the dataset fields

{% note tip "User Documentation" %}

- [Request debugging in Bitrix24 BI Connector](https://helpdesk.bitrix24.com/open/17499442/)
- [FAQ: BI analytics and BI Builder](https://helpdesk.bitrix24.com/open/24380778/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../scopes/permissions.md)
>
> Who can execute methods: a user with access to the "Analyst Workspace" section

### Connectors

#| 
|| **Method** | **Description** ||
|| [biconnector.connector.add](./connector/biconnector-connector-add.md) | Adds a new connector ||
|| [biconnector.connector.update](./connector/biconnector-connector-update.md) | Updates an existing connector ||
|| [biconnector.connector.get](./connector/biconnector-connector-get.md) | Retrieves information about the connector ||
|| [biconnector.connector.list](./connector/biconnector-connector-list.md) | Retrieves a list of available connectors ||
|| [biconnector.connector.delete](./connector/biconnector-connector-delete.md) | Deletes a connector ||
|| [biconnector.connector.fields](./connector/biconnector-connector-fields.md) | Retrieves the description of the connector fields ||
|#

### Sources

#| 
|| **Method** | **Description** ||
|| [biconnector.source.add](./source/biconnector-source-add.md) | Adds a new source ||
|| [biconnector.source.update](./source/biconnector-source-update.md) | Updates an existing source ||
|| [biconnector.source.get](./source/biconnector-source-get.md) | Retrieves information about the source ||
|| [biconnector.source.list](./source/biconnector-source-list.md) | Retrieves a list of available sources ||
|| [biconnector.source.delete](./source/biconnector-source-delete.md) | Deletes a source ||
|| [biconnector.source.fields](./source/biconnector-source-fields.md) | Retrieves the description of the source fields ||
|#

### Datasets

#| 
|| **Method** | **Description** ||
|| [biconnector.dataset.add](./dataset/biconnector-dataset-add.md) | Adds a new dataset ||
|| [biconnector.dataset.update](./dataset/biconnector-dataset-update.md) | Updates an existing dataset ||
|| [biconnector.dataset.get](./dataset/biconnector-dataset-get.md) | Retrieves information about the dataset ||
|| [biconnector.dataset.list](./dataset/biconnector-dataset-list.md) | Retrieves a list of available datasets ||
|| [biconnector.dataset.delete](./dataset/biconnector-dataset-delete.md) | Deletes a dataset ||
|| [biconnector.dataset.fields](./dataset/biconnector-dataset-fields.md) | Retrieves the description of the dataset fields ||
|| [biconnector.dataset.fields.update](./dataset/biconnector-dataset-fields-update.md) | Updates the fields of the dataset ||
|#