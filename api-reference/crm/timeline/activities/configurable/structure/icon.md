# Icon

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

`IconDto` is the icon to the left of the [timeline entry](../index.md). It tells the user at a glance what the entry is about: a call, a document, a message. An icon is not a logo: [`LogoDto`](./body.md#logo-dto) is a large image inside the content area, while the icon occupies a narrow column to the left of the whole entry.

The object is passed in the `icon` field of the [configurable activity structure](./layout.md), and the structure itself is passed in the `layout` parameter of the [crm.activity.configurable.add](../crm-activity-configurable-add.md) and [crm.activity.configurable.update](../crm-activity-configurable-update.md) methods. The `icon` field is required: without it the method returns the `FIELD_IS_REQUIRED` error. The calling conditions are described [on the structure page](./layout.md).

## Parameters of the `IconDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **code^*^**
[`string`](../../../../../data-types.md) | Icon code, for example `call-completed` ||
|#

The object has no other fields. An extra field is rejected by the method with the `FIELD_IS_REDUNDANT` error, and an unknown icon code with the `ENUM_FIELD` error.

## How to Choose an Icon Code

Bitrix24 has a set of system icons for typical timeline events. The full list is returned by the [crm.timeline.icon.list](../../../logmessage/icons/crm-timeline-icon-list.md) method. Each icon in the response has a `code` and an `isSystem` flag: `true` — a Bitrix24 icon, `false` — an icon uploaded by an application.

Frequently used codes:

#|
|| **Code** | **For Which Entry** ||
|| `call`, `call-incoming`, `call-outcoming`, `call-completed`, `call-incoming-missed` | Calls: general, incoming, outgoing, completed, missed ||
|| `sms`, `telegram`, `whatsapp`, `IM` | Messages in the corresponding channel ||
|| `mail-outcome` | Outgoing email ||
|| `document` | Document ||
|| `task` | Task ||
|| `info` | Information message ||
|| `check`, `circle-check`, `complete` | Confirmation or completion ||
|| `clock` | Reminder, waiting ||
|| `stage-change`, `pipeline` | Stage change, work with a pipeline ||
|#

If there is no suitable icon, upload your own with the [crm.timeline.icon.add](../../../logmessage/icons/crm-timeline-icon-add.md) method. The application sets the code for it, and after registration the same code is passed in the `code` field of the activity structure. The [crm.timeline.icon.get](../../../logmessage/icons/crm-timeline-icon-get.md) method verifies that the icon is registered.

## Example of the Object

The value of the `icon` field: the icon of a completed call.

```json
{
    "code": "call-completed"
}
```

The icon as part of a complete structure is in the [`LayoutDto` object example](./layout.md#primer).

## Continue Exploring

- [{#T}](./layout.md)
- [{#T}](./header.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./action.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)
