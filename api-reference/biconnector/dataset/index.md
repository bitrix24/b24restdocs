# Datasets: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A dataset is an object of the BI Builder module. Datasets are used to display and process information in Bitrix24 obtained from sources.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [BI Builder: Create a dataset from a CSV file](https://helpdesk.bitrix24.com/open/23655786/)

{% note info "" %}

Methods work only in the context of the [application](../../../settings/app-installation/index.md)

{% endnote %}

## Connection of Dataset with Connector and Sources

A dataset is the final level in the data hierarchy within the BI Builder module:
- **Connector** establishes a connection with an external data source.
- **Source** defines which specific data is available from the connected service.
- **Dataset** forms the final set of data that can be used in reports and analytics.

## Before You Begin

1. Create a connector using the [biconnector.connector.add](../connector/biconnector-connector-add.md) method or select an existing connector using the [biconnector.connector.list](../connector/biconnector-connector-list.md) method
2. Create a source using the [biconnector.source.add](../source/biconnector-source-add.md) method and pass the `connectorId` of the required connector
3. Create a dataset using the [biconnector.dataset.add](./biconnector-dataset-add.md) method and pass the `sourceId` of the source
4. Configure the dataset fields using the [biconnector.dataset.fields.update](./biconnector-dataset-fields-update.md) method

## Description of Dataset Fields {#dataset}

#|
|| **Name** | **Description** ||
|| **id** | Unique dataset identifier ||
|| **type** | Dataset type, value is always equal to `rest` ||
|| **name** | Dataset name ||
|| **description** | Description of the dataset ||
|| **externalCode** | External dataset code ||
|| **externalName** | External dataset name ||
|| **dateCreate** | Dataset create date ||
|| **dateUpdate** | Dataset update date ||
|| **createdById** | Identifier of the user who created the dataset ||
|| **updatedById** | Identifier of the user who updated the dataset ||
|| **externalId** | External dataset identifier ||
|| **fields** | List of [fields](#fields) included in the dataset ||
|#

### Description of the Fields Field {#fields}

#|
|| **Name** | **Description** ||
|| **id** | Field identifier ||
|| **datasetId** | Identifier of the dataset to which the field belongs ||
|| **type** | Data type. Available types:
`int` — integer
`string` — string
`double` — float, dot separator
`date` — date, format `Y-m-d`
`datetime` — date and time, format `Y-m-d H:i:s` ||
|| **name** | Field name. The name must start with a letter, you can use only uppercase Latin letters `A-Z`, numbers and sign `_`. Maximum name length is 32 characters ||
|| **externalCode** | External code of the field ||
|| **visible** | Field visibility flag ||
|#

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute methods: A user with access to the "Analytics hub" section

#|
|| **Method** | **Description** ||
|| [biconnector.dataset.add](./biconnector-dataset-add.md) | Adds a new dataset ||
|| [biconnector.dataset.update](./biconnector-dataset-update.md) | Updates an existing dataset ||
|| [biconnector.dataset.fields.update](./biconnector-dataset-fields-update.md) | Updates the fields of the dataset ||
|| [biconnector.dataset.get](./biconnector-dataset-get.md) | Returns information about the dataset ||
|| [biconnector.dataset.list](./biconnector-dataset-list.md) | Returns a list of available datasets ||
|| [biconnector.dataset.delete](./biconnector-dataset-delete.md) | Deletes a dataset ||
|| [biconnector.dataset.fields](./biconnector-dataset-fields.md) | Returns the description of the dataset fields ||
|#
