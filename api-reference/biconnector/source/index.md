# Sources: Overview of Methods

A source is a separate connection to an external system in the BIconnector module. The source defines which specific data from the external system will be available for use in reports and analytics in Bitrix24.

> Quick navigation: [all methods](#all-methods) 

{% note info "" %}

Methods only work in the context of the [application](../../app-installation/index.md)

{% endnote %}

## Connection of the Source with the Connector and Datasets

The source is registered through the connector. In the hierarchy of the BIconnector module, sources occupy an intermediate level:
- **Connector** establishes a connection with the external data source.
- **Source** defines the access parameters for the data.
- **Dataset** forms the final set of data that can be used in reports and analytics.

## Description of Source Fields {#fields}

#|
|| **Name**
`type` | **Description** | Read | Write ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the source | ✅ | ❌ ||
|| **title**
[`string`](../../data-types.md) | Name of the source | ✅ | ✅ ||
|| **type**
[`string`](../../data-types.md) | Type of the source, value is always `rest` | ✅ | ❌ ||
|| **code**
[`string`](../../data-types.md) | Code of the source, system field | ✅ | ❌ ||
|| **description**
[`string`](../../data-types.md) | Description of the source | ✅ | ✅ ||
|| **active**
[`boolean`](../../data-types.md) | Status of the source's activity | ✅ | ✅ ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Creation date of the source | ✅ | ❌ ||
|| **dateUpdate**
[`datetime`](../../data-types.md) | Update date of the source | ✅ | ❌ ||
|| **createdById**
[`integer`](../../data-types.md) | Identifier of the user who created the source | ✅ | ❌ ||
|| **updatedById**
[`integer`](../../data-types.md) | Identifier of the user who updated the source | ✅ | ❌ ||
|| **connectorId**
[`integer`](../../data-types.md) | Identifier of the connector to which the source is linked | ✅ | ✅ ||
|| **settings**
[`array`](../../data-types.md) | [List of authorization parameters](#settings) | ✅ | ✅ ||
|#

### Field settings {#settings}

The `settings` field contains a list of parameters for authorization through the connector. The parameters are passed in the format of an object, where the key is the parameter identifier — `code`. The values of `code` can be obtained using the methods [biconnector.connector.list](../connector/biconnector-connector-list.md) or [biconnector.connector.get](../connector/biconnector-connector-get.md).

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with access to the "Analyst Workspace" section

#|
|| **Method** | **Description** ||
|| [biconnector.source.add](./biconnector-source-add.md) | Adds a new source ||
|| [biconnector.source.update](./biconnector-source-update.md) | Updates an existing source ||
|| [biconnector.source.get](./biconnector-source-get.md) | Returns information about the source ||
|| [biconnector.source.list](./biconnector-source-list.md) | Returns a list of available sources ||
|| [biconnector.source.delete](./biconnector-source-delete.md) | Deletes a source ||
|| [biconnector.source.fields](./biconnector-source-fields.md) | Returns the description of the source fields ||
|#