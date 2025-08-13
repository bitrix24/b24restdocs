# Datasets: Overview of Methods

A dataset is an object of the BIconnector module. Datasets are used to display and process information in Bitrix24 obtained from sources.

> Quick navigation: [all methods](#all-methods) 

{% note info "" %}

Methods work only in the context of the [application](../../app-installation/index.md)

{% endnote %}

## Connection of Dataset with Connector and Sources

A dataset is the final level in the hierarchy of data handling in the BIconnector module:
- **Connector** establishes a connection with an external data source.
- **Source** defines which specific data is available from the connected service.
- **Dataset** forms the final set of data that can be used in reports and analytics.

## Description of Dataset Fields {#dataset}

#|
|| **Name**
`type` | **Description** | Read | Write ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the dataset | ✅ | ❌ ||
|| **type**
[`string`](../../data-types.md) | Type of the dataset, the value is always `rest` | ✅ | ❌ ||
|| **name**
[`string`](../../data-types.md) | Name of the dataset | ✅ | ✅ ||
|| **description**
[`string`](../../data-types.md) | Description of the dataset | ✅ | ✅ ||
|| **externalCode**
[`string`](../../data-types.md) | External code of the dataset | ✅ | ✅ ||
|| **externalName**
[`string`](../../data-types.md) | External name of the dataset | ✅ | ✅ ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Creation date of the dataset | ✅ | ❌ ||
|| **dateUpdate**
[`datetime`](../../data-types.md) | Update date of the dataset | ✅ | ❌ ||
|| **createdById**
[`integer`](../../data-types.md) | Identifier of the user who created the dataset | ✅ | ❌ ||
|| **updatedById**
[`integer`](../../data-types.md) | Identifier of the user who updated the dataset | ✅ | ❌ ||
|| **externalId**
[`integer`](../../data-types.md) | External identifier of the dataset | ✅ | ❌ ||
|| **fields**
[`array`](../../data-types.md) | List of [fields](#fields) included in the dataset | ✅ | ✅ ||
|#

### Description of the Fields Field {#fields}

#|
|| **Name**
`type` | **Description** | Read | Write ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the field | ✅ | ❌ ||
|| **datasetId**
[`integer`](../../data-types.md) | Identifier of the dataset to which the field belongs | ✅ | ✅ ||
|| **type**
[`string`](../../data-types.md) | Data type. Available types: 
`int` — integer
`string` — string
`double` — decimal number, separator is a dot
`date` — date, format `Y-m-d`
`datetime` — date with time, format `Y-m-d H:i:s` | ✅ | ✅ ||
|| **name**
[`string`](../../data-types.md) | Name of the field. The name must start with a letter and can only use uppercase Latin letters `A-Z`, digits, and the `_` sign. The maximum length of the name is 32 characters | ✅ | ✅ ||
|| **externalCode**
[`string`](../../data-types.md) | External code of the field | ✅ | ✅ ||
|| **visible**
[`boolean`](../../data-types.md) | Field visibility flag | ✅ | ✅ ||
|#

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can perform methods: a user with access to the "Analyst's Workspace" section

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