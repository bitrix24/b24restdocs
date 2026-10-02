# Timeline Entry Logotypes: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Logotypes help visually distinguish configurable activities in the CRM timeline.

Using the methods in this section, you can add a custom logotype, retrieve data by code, display a list of available logotypes, and delete a logotype.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Considerations Before Calling Methods

- The methods [crm.timeline.logo.add](./crm-timeline-logo-add.md) and [crm.timeline.logo.delete](./crm-timeline-logo-delete.md) can only be managed by an administrator.
- The methods [crm.timeline.logo.get](./crm-timeline-logo-get.md) and [crm.timeline.logo.list](./crm-timeline-logo-list.md) are available to any user.
- To create a logotype, pass `fileContent` in `base64`. Use a `PNG` file sized `60x60` pixels.

## How to Work with Logotypes

1. Retrieve the list of available codes through [crm.timeline.logo.list](./crm-timeline-logo-list.md).
2. Add a new logotype using the method [crm.timeline.logo.add](./crm-timeline-logo-add.md).
3. Check the logotype by code using the method [crm.timeline.logo.get](./crm-timeline-logo-get.md).
4. Delete the custom logotype using the method [crm.timeline.logo.delete](./crm-timeline-logo-delete.md) if it is no longer in use.

## Relation to Other Objects

**Configurable Activities.** Pass the logotype code in the `layout.body.logo.code` field of the [crm.activity.configurable.add](../../activities/configurable/crm-activity-configurable-add.md) and [crm.activity.configurable.update](../../activities/configurable/crm-activity-configurable-update.md) methods. The [LogoDto](../../activities/configurable/structure/body.md#logo-dto) object describes the field structure.

**Log Entry Icons.** A logotype and an icon refer to different timeline elements. For the `fields.iconCode` field of the [crm.timeline.logmessage.add](../crm-timeline-logmessage-add.md) method, retrieve codes using the [crm.timeline.icon.*](../icons/index.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [crm.timeline.logo.add](./crm-timeline-logo-add.md) | Adds a new logotype ||
|| [crm.timeline.logo.get](./crm-timeline-logo-get.md) | Retrieves information about a logotype ||
|| [crm.timeline.logo.list](./crm-timeline-logo-list.md) | Retrieves a list of all available logotypes ||
|| [crm.timeline.logo.delete](./crm-timeline-logo-delete.md) | Deletes a logotype ||
|#
