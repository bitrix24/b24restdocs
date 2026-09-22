# Configurable Activity Badges: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A badge is an icon on the card of a CRM object in the kanban that draws the manager's attention to that object. The icon comes from the `badgeCode` field of an activity, not from a link to the object itself: if an object has several activities with badges, the most recently added one is displayed.

![Last badge](./_images/badge.png)

> Quick navigation: [all methods](#all-methods)

## Link with a Configurable Activity

The code of a registered badge is specified in the `badgeCode` field of a [configurable activity](../index.md) when calling [crm.activity.configurable.add](../crm-activity-configurable-add.md) or [crm.activity.configurable.update](../crm-activity-configurable-update.md).

The badge is displayed on the kanban of the object to which the activity is linked while the activity is not closed, that is, while `completed = false`.

## Considerations Before Calling Methods

- A badge that is already used in activities cannot be deleted — the method returns the `ENTITY_WITH_BADGE_EXISTS` error. Clear the code from the `badgeCode` field of those activities first.
- The methods work both in an app and via an inbound webhook. The app context is required only for the [configurable activities](../index.md) themselves.

## How to Work with Badges

1. Review the codes already in use with the [crm.activity.badge.list](./crm-activity-badge-list.md) method.
2. Register the badge with the [crm.activity.badge.add](./crm-activity-badge-add.md) method.
3. Specify the badge code in the `badgeCode` field of the configurable activity.
4. Retrieve the badge settings by code with the [crm.activity.badge.get](./crm-activity-badge-get.md) method if you need to check them.
5. Delete a badge you no longer need with the [crm.activity.badge.delete](./crm-activity-badge-delete.md) method.

## Badge Fields {#badge-fields}

A badge consists of four fields. The [crm.activity.badge.get](./crm-activity-badge-get.md) and [crm.activity.badge.list](./crm-activity-badge-list.md) methods return it in this form:

```json
{
    "code": "missedCall",
    "title": "Call Status",
    "value": "Missed",
    "type": "failure"
}
```

#|
|| **Field** | **Description** ||
|| **code**
[`string`](../../../../../data-types.md) | Badge code, for example `missedCall`. The badge is specified in the `badgeCode` field of an activity, retrieved, and deleted by this code ||
|| **title**
[`string`\|`object`](../../../../../data-types.md) | Badge name — the label that tells it apart from the others. The icon itself displays `value`, not this field. A string or an object with translations for different languages ||
|| **value**
[`string`\|`object`](../../../../../data-types.md) | Text displayed inside the icon itself. It is shown in uppercase. A string or an object with translations for different languages ||
|| **type**
[`string`](../../../../../data-types.md) | [Badge type](#badge-type), defines the color of the icon ||
|#

Restrictions on field values and error codes are described on the [crm.activity.badge.add](./crm-activity-badge-add.md) page.

If `title` or `value` contains an object, its keys must be language codes that Bitrix24 recognizes, and the values must be the text in those languages, for example:

```json
{
    "de": "Achtung",
    "en": "Alarm"
}
```

If there is no translation for the current language, Bitrix24 uses the English one, and if there is no English either — the first value of the object.

## Badge Type

The badge type takes one of five values:

- `success` — green background
- `failure` — red background
- `warning` — yellow background
- `primary` — blue background
- `secondary` — gray background

Any other value is rejected by the [crm.activity.badge.add](./crm-activity-badge-add.md) method with the `WRONG_TYPE_VALUE` error.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: a user with administrative access to the CRM section — for [crm.activity.badge.add](./crm-activity-badge-add.md) and [crm.activity.badge.delete](./crm-activity-badge-delete.md), any user — for [crm.activity.badge.get](./crm-activity-badge-get.md) and [crm.activity.badge.list](./crm-activity-badge-list.md)

#|
|| **Method** | **Description** ||
|| [crm.activity.badge.add](./crm-activity-badge-add.md) | Adds a new badge ||
|| [crm.activity.badge.get](./crm-activity-badge-get.md) | Retrieves information about a badge ||
|| [crm.activity.badge.list](./crm-activity-badge-list.md) | Retrieves a list of badges ||
|| [crm.activity.badge.delete](./crm-activity-badge-delete.md) | Deletes a badge by code ||
|#
