# Configurable CRM Activities: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Configurable activities are CRM activities created by an application. An application configures the appearance of the activity card, timeline blocks, buttons, and badges.

{% note warning %}

Methods `crm.activity.configurable.add`, `crm.activity.configurable.update`, and `crm.activity.configurable.get` work only within the context of an [application](../../../../../settings/app-installation/index.md). Calling them via an incoming webhook will return error `ERROR_WRONG_CONTEXT`.

{% endnote %}

> Quick navigation: [All Methods](#all-methods)

## Getting Started

1. Prepare the application from which the methods will be called and obtain an OAuth token
2. Create a configurable activity using the [crm.activity.configurable.add](./crm-activity-configurable-add.md) method. If a custom activity type is required, first register it using the [crm.activity.type.add](../types/crm-activity-type-add.md) method with the `IS_CONFIGURABLE_TYPE = Y` field
3. Update the structure or data of an activity using the [crm.activity.configurable.update](./crm-activity-configurable-update.md) method
4. Retrieve an activity by ID using the [crm.activity.configurable.get](./crm-activity-configurable-get.md) method
5. Find configurable activities using the [crm.activity.list](../activity-base/crm-activity-list.md) method with the `PROVIDER_ID = CONFIGURABLE_REST_APP` filter
6. Delete an activity using the [crm.activity.delete](../activity-base/crm-activity-delete.md) method

## Relationships with Other Sections

**Custom Activity Types.** The [crm.activity.type.add](../types/crm-activity-type-add.md), [crm.activity.type.list](../types/crm-activity-type-list.md), and [crm.activity.type.delete](../types/crm-activity-type-delete.md) methods manage the types that can be passed to the `typeId` field of a configurable activity. For `typeId`, different from `CONFIGURABLE`, the type must be created by the same application with `IS_CONFIGURABLE_TYPE = Y`.

**Card Structure.** The [Configurable Activity Structure](./structure/layout.md) section describes `layout`: the icon, heading, body, footer, and actions within the card.

**Badges.** The [Configurable Activity Badges](./badges/index.md) section describes the badge codes that can be passed to the `badgeCode` field.

## Additional

- [{#T}](./structure/layout.md)
- [{#T}](./badges/index.md)

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [crm.activity.configurable.add](./crm-activity-configurable-add.md) | Adds a new configurable activity to the timeline ||
|| [crm.activity.configurable.update](./crm-activity-configurable-update.md) | Updates a configurable activity ||
|| [crm.activity.configurable.get](./crm-activity-configurable-get.md) | Retrieves information about the activity ||
|| [crm.activity.delete](../activity-base/crm-activity-delete.md) | Deletes a configurable activity by its identifier ||
|| [crm.activity.list](../activity-base/crm-activity-list.md) | Retrieves a list of all configurable activities for a CRM entity filtered by `PROVIDER_ID` = `CONFIGURABLE_REST_APP` ||
|#
