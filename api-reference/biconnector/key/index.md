# BI Connector Access Keys: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A BI connector access key is a secret string that external analytics systems, such as Microsoft Power BI or Google Looker Studio, use to retrieve Bitrix24 data through the BI connector. The `biconnector.key.*` methods let an application create such keys, retrieve their values, enable, disable, and delete keys.

{% note warning "" %}

The methods work only in the context of an [application](../../../settings/app-installation/index.md) and only with the keys that this application created. When called via a webhook, the methods return the `insufficient_auth_type` error

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Manage keys in BI analytics](https://helpdesk.bitrix24.com/open/18374896/)

## How a Key Works

A key is created by the [biconnector.key.add](./biconnector-key-add.md) method. Bitrix24 generates the key value itself: it is a string of 32 Latin letters and digits. You cannot set your own value.

A key belongs to the application that created it and to the user on whose behalf the method was called. Other applications do not see this key. The [biconnector.key.list](./biconnector-key-list.md) method returns the key value in the `ACCESS_KEY` field only if the key was created by the same user. For keys created by other users of this application, the response has no `ACCESS_KEY` field.

{% note warning "" %}

Do not publish the key value or store it in public code: anyone with the key can retrieve Bitrix24 data through the BI connector. If the value becomes known to others, delete the key with the [biconnector.key.delete](./biconnector-key-delete.md) method and create a new one

{% endnote %}

## How to Get Started

1. Create a key with the [biconnector.key.add](./biconnector-key-add.md) method and the value `ACTIVE` = `Y`: without it, the key is created disabled and no data is returned for it. The response contains the key identifier.
2. Retrieve the key value with the [biconnector.key.list](./biconnector-key-list.md) method: pass the key identifier in `filter`, and the value arrives in the `ACCESS_KEY` field.
3. Pass the value to the external analytics system. How the key is passed in requests to the BI connector is shown in the article [Request debugging in Bitrix24 BI Connector](https://helpdesk.bitrix24.com/open/17499442/).
4. To revoke access temporarily, disable the key with the [biconnector.key.update](./biconnector-key-update.md) method and the value `ACTIVE` = `N`. To revoke access permanently, delete the key with the [biconnector.key.delete](./biconnector-key-delete.md) method.

## Key Fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Key identifier. In the [biconnector.key.list](./biconnector-key-list.md) response, it is returned as a string ||
|| **ACTIVE**
[`string`](../../data-types.md) | Whether the key is active. Possible values:

- `Y` — the key is enabled, data can be retrieved with it
- `N` — the key is disabled ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Date and time the key was created in ISO 8601 format, for example `2026-11-24T14:02:26+01:00` ||
|| **TIMESTAMP_X**
[`datetime`](../../data-types.md) | Date and time the key was last modified in ISO 8601 format ||
|| **CREATED_BY**
[`user`](../../data-types.md) | Identifier of the user who created the key. It is returned as a string ||
|| **CONNECTION**
[`string`](../../data-types.md) | Database connection from which the BI connector returns data for the key. In cloud Bitrix24, it is `bi` ||
|| **ACCESS_KEY**
[`string`](../../data-types.md) | Key value. Returned by the [biconnector.key.list](./biconnector-key-list.md) method only if the key was created by the user on whose behalf the method is called ||
|#

## Differences from Other BIconnector Methods

The key methods work differently from the methods for [connectors](../connector/index.md), [sources](../source/index.md), and [tables](../table/index.md):

- errors arrive at the root of the response, not [inside the `result` field](../index.md#errors)
- access requires the permission to manage BI connector keys; the "Access to BI Builder" and "Access to Analytics Hub" permissions are not checked
- pagination in [biconnector.key.list](./biconnector-key-list.md) is set with the `limit` and `offset` parameters, not `page`
- fields are passed and returned in upper case: `ACTIVE`, `CONNECTION`

## Overview of Methods {#all-methods}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with the permission to manage BI connector keys, for example an administrator

#|
|| **Method** | **Description** ||
|| [biconnector.key.add](./biconnector-key-add.md) | Creates an access key ||
|| [biconnector.key.update](./biconnector-key-update.md) | Enables or disables a key or changes its connection ||
|| [biconnector.key.list](./biconnector-key-list.md) | Retrieves the list of the application's keys ||
|| [biconnector.key.delete](./biconnector-key-delete.md) | Deletes a key ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../connector/index.md)
- [Request debugging in Bitrix24 BI Connector](https://helpdesk.bitrix24.com/open/17499442/)
