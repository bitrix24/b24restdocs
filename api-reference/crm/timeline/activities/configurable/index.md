# Configurable CRM Activities: Methods and Events Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Configurable activities are CRM activities created by an application. An application configures the appearance of the timeline entry, its buttons, and badges. The default type of such an activity is `CONFIGURABLE`.

{% note info "" %}

The `crm.activity.configurable.add` and `crm.activity.configurable.update` methods work only within the context of an [application](../../../../../settings/app-installation/index.md). Calling them via an inbound webhook returns the `ERROR_WRONG_CONTEXT` error. Only the application that created the activity can update it — otherwise the method returns the `ERROR_WRONG_APPLICATION` error.

The `crm.activity.configurable.get` method and the badge methods do not require the application context.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)

## Getting Started

1. Prepare the application from which the methods will be called and obtain an OAuth token.
2. Register a custom activity type using the [crm.activity.type.add](../types/crm-activity-type-add.md) method with the `IS_CONFIGURABLE_TYPE = Y` field if the default type does not fit.
3. Register a badge using the [crm.activity.badge.add](./badges/crm-activity-badge-add.md) method if the activity needs an icon in the kanban. The badge code is passed in the `badgeCode` field of the activity.
4. Create a configurable activity using the [crm.activity.configurable.add](./crm-activity-configurable-add.md) method. The response returns the activity identifier — you will need it later.
5. Handle clicks on buttons, tags, and menu items. An [action](./structure/action.md) of the `restEvent` type sends the application the `onCrmTimelineItemAction` event, which you subscribe to with the [event.bind](../../../../events/event-bind.md) method.
6. Update the structure or data of the activity using the [crm.activity.configurable.update](./crm-activity-configurable-update.md) method. The same method releases the entry lock after a click.
7. Retrieve the activity by its identifier using the [crm.activity.configurable.get](./crm-activity-configurable-get.md) method: it returns the activity fields and the `layout` structure.
8. Find configurable activities using the [crm.activity.list](../activity-base/crm-activity-list.md) method with the `PROVIDER_ID = CONFIGURABLE_REST_APP` filter.
9. Delete the activity using the [crm.activity.delete](../activity-base/crm-activity-delete.md) method.

## Relationships with Other Objects

A configurable activity relies on four neighbouring objects: custom activity types, the entry structure, badges, and the shared timeline icons.

**Custom Activity Types.** The [crm.activity.type.add](../types/crm-activity-type-add.md), [crm.activity.type.list](../types/crm-activity-type-list.md), and [crm.activity.type.delete](../types/crm-activity-type-delete.md) methods manage the types that can be passed to the `typeId` field of a configurable activity. For `typeId`, different from `CONFIGURABLE`, the type must be created by the same application with `IS_CONFIGURABLE_TYPE = Y`.

**Entry Structure.** The [Configurable Activity Structure](./structure/layout.md) section describes `layout`: the icon, heading, body, and bottom part. The reaction to a click is set by [ActionDto](./structure/action.md). The same section lists the [structure restrictions](./structure/layout.md#limits) and validation error codes, and ready-made configurations are collected in the [examples](./structure/examples.md).

**Badges.** The [Configurable Activity Badges](./badges/index.md) section describes the kanban icons and the codes passed in the `badgeCode` field of an activity.

**Icons and Logos.** Codes for the `icon` and `body.logo` fields are returned by the [crm.timeline.icon.list](../../logmessage/icons/crm-timeline-icon-list.md) and [crm.timeline.logo.list](../../logmessage/logo/crm-timeline-logo-list.md) methods. Your own images are added with the [crm.timeline.icon.add](../../logmessage/icons/crm-timeline-icon-add.md) and [crm.timeline.logo.add](../../logmessage/logo/crm-timeline-logo-add.md) methods.

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method — activities are available to a user with permissions for the CRM object, adding and deleting badges only to a CRM administrator

### Configurable Activity

#|
|| **Method** | **Description** ||
|| [crm.activity.configurable.add](./crm-activity-configurable-add.md) | Adds a new configurable activity to the timeline ||
|| [crm.activity.configurable.update](./crm-activity-configurable-update.md) | Updates a configurable activity ||
|| [crm.activity.configurable.get](./crm-activity-configurable-get.md) | Retrieves a configurable activity by its identifier ||
|| [crm.activity.list](../activity-base/crm-activity-list.md) | Retrieves a list of all configurable activities for a CRM entity filtered by `PROVIDER_ID` = `CONFIGURABLE_REST_APP` ||
|| [crm.activity.delete](../activity-base/crm-activity-delete.md) | Deletes a configurable activity by its identifier ||
|#

### Badges

Details are in the [Configurable Activity Badges](./badges/index.md) section.

#|
|| **Method** | **Description** ||
|| [crm.activity.badge.add](./badges/crm-activity-badge-add.md) | Adds a new badge ||
|| [crm.activity.badge.get](./badges/crm-activity-badge-get.md) | Retrieves information about a badge ||
|| [crm.activity.badge.list](./badges/crm-activity-badge-list.md) | Retrieves a list of badges ||
|| [crm.activity.badge.delete](./badges/crm-activity-badge-delete.md) | Deletes a badge by code ||
|#

### Events

#|
|| **Event** | **Triggered** ||
|| [onCrmTimelineItemAction](./structure/action.md#sobytie) | When an entry element with a `restEvent` action is clicked — a heading, tag, logo, link, button, or menu item. Delivered only to the application that set the action ||
|#
