# About Configurable Activity Badges

> Quick navigation: [all methods](#all-methods)

A `badge` is an icon on the card of a CRM entity in the kanban. The badge helps highlight entities that require attention. If multiple badges are added to an entity, only the most recently added badge will be displayed.

![Last badge](./_images/badge.png)

Badges can be added for a [configurable activity](../index.md) in the `badgeCode` field. The badge will be displayed on the kanban of the object to which the activity is linked until the activity is closed.

## Badge Record Fields

#|
|| **Field** | **Description** ||
|| **code**
[`string`](../../../../data-types.md) | Badge code, for example `missedCall` ||
|| **title**
[`string`\|`array`](../../../../data-types.md) | Title of the badge. Can be a string or an array of strings for different languages ||
|| **value**
[`string`\|`array`](../../../../data-types.md) | Value of the badge. Can be a string or an array of strings for different languages ||
|| **type**
[`string`](../../../../data-types.md) | [Badge type](#badge-type) ||
|#

If **title** or **value** contains an array, the keys should be language codes, and the values should be text in those languages, for example:

```json
{
    "de": "Achtung",
    "en": "Alarm"
}
```

If a translation for the current language is not found, the English version will be used. If the English translation is also not found, the first element of the array will be used.

## Badge Type

In Bitrix24, there are several standard badges for different scenarios. The badge type can take the following values:

- **success** — Green background
- **failure** — Red background
- **warning** — Yellow background
- **primary** — Blue background
- **secondary** — Gray background

# Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [crm.activity.badge.add](./crm-activity-badge-add.md) | Adds a new badge ||
|| [crm.activity.badge.get](./crm-activity-badge-get.md) | Retrieves information about a badge ||
|| [crm.activity.badge.list](./crm-activity-badge-list.md) | Retrieves a list of badges ||
|| [crm.activity.badge.delete](./crm-activity-badge-delete.md) | Deletes a badge by code ||
|#

## Additional

- [{#T}](../crm-activity-configurable-add.md)
- [{#T}](../crm-activity-configurable-update.md)
- [{#T}](../crm-activity-configurable-get.md)