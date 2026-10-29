# CRM Custom Activity Types: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Applications can register custom activity types: upload a custom icon and specify the type name. For example, you can create your own activity type with an icon and name of your application.

{% note warning %}

Methods `crm.activity.type.add`, `crm.activity.type.list`, and `crm.activity.type.delete` work only within the context of an [application](../../../../../settings/app-installation/index.md) and are available to the administrator. Calling them via an inbound webhook will return error `Application context required`.

{% endnote %}

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Timeline in CRM Item](https://helpdesk.bitrix24.com/open/25816403/)

## Getting Started

1. Prepare the name and icon for the activity type
2. Register the type using the [crm.activity.type.add](./crm-activity-type-add.md) method. If the type is required for configurable activities, pass `IS_CONFIGURABLE_TYPE = Y`
3. Retrieve the list of registered types using the [crm.activity.type.list](./crm-activity-type-list.md) method. The method returns all types of the application at once, without pagination
4. Delete the custom type using the [crm.activity.type.delete](./crm-activity-type-delete.md) method if it is no longer needed

## Activity Type Requirements

The `TYPE_ID` code and the `NAME` are required. The code must be no longer than 100 characters and unique among the current application's types, and the name must be no longer than 255 characters. If `TYPE_ID` or `NAME` is empty or exceeds the allowed length, the method returns `false` without an error.

The `ICON_FILE` icon is optional. Pass it as an [array of the file name and its Base64 content](../../../../files/how-to-upload-files.md#array), for example `["icon.png", "iVBORw0KGgo..."]`. Without an icon, the type is registered without a custom icon.

After registration, the `TYPE_ID` value is used as the activity type code. To create a regular activity of this type, call [crm.activity.add](../activity-base/crm-activity-add.md) from the same application and pass `PROVIDER_ID` = `REST_APP` and `PROVIDER_TYPE_ID` = the type code. For example, for a registered `TYPE_ID = CUSTOM_CALL`, specify `PROVIDER_TYPE_ID = CUSTOM_CALL`.

## Linking to Configurable Activities

A custom type with `IS_CONFIGURABLE_TYPE = Y` can be passed to the `typeId` field of the [crm.activity.configurable.add](../configurable/crm-activity-configurable-add.md) method. If `IS_CONFIGURABLE_TYPE = N`, the type cannot be used for a configurable activity.

Types are linked to the application that registered them. The [crm.activity.type.list](./crm-activity-type-list.md) method returns the types of the current application, and [crm.activity.type.delete](./crm-activity-type-delete.md) deletes only them.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [crm.activity.type.add](./crm-activity-type-add.md) | Registers a custom activity type with a name and icon ||
|| [crm.activity.type.list](./crm-activity-type-list.md) | Retrieves a list of custom case types ||
|| [crm.activity.type.delete](./crm-activity-type-delete.md) | Deletes a custom type ||
|#
