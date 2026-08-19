# Custom Field Types: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The `userfieldtype.*` methods manage an application's own custom field types. The application registers not a field but its type and the handler address. When a user opens a card with a field of this type, Bitrix24 opens the handler in a frame inside the field, and further work is no different from that with a regular widget.

In the Bitrix24 cloud, such fields work in the CRM card. An application creates fields of standard types and of the types it has registered itself. An administrator creates fields of any registered types, including types of other applications.

> Quick navigation: [all methods](#all-methods)

## Getting Started

1. Register the field type using the [userfieldtype.add](./userfieldtype-add.md) method. Pass the short type code `USER_TYPE_ID` and the public handler address `HANDLER`

2. Retrieve the application `ID` using the [app.info](../../common/system/app-info.md) method and assemble the full type code in the `rest_<APP_ID>_<USER_TYPE_ID>` format. For example, for an application with `ID: 123` and `USER_TYPE_ID: phone_data`, the full code will be `rest_123_phone_data`

3. Create a field with this type — using the universal [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig-add.md) method or an object method of the CRM, for example [crm.lead.userfield.add](../../crm/leads/userfield/crm-lead-userfield-add.md). In the `USER_TYPE_ID` parameter, pass the full type code, not the short one

4. Add the field to the card form and open the card — the handler will load inside the field

5. Parse `PLACEMENT_OPTIONS` in the handler and write the field value using the `setValue` command

For the complete scenario with the handler code, see the tutorial [{#T}](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md).

## What the Handler Receives

The handler opens in the `USERFIELD_TYPE` placement. There is no need to register it using the [placement.bind](../placement-bind.md) method: the placement is created by `userfieldtype.add`.

Along with the request, Bitrix24 passes `PLACEMENT_OPTIONS` to the handler — data about the field and the item in whose card the field is open. In the HTTP request, the data is passed as a JSON string; in the b24jssdk library, the same data is available as the `$b24.placement.options` object.

#|
|| **Key** | **Contents** ||
|| `MODE` | Field display mode: `view` or `edit` ||
|| `ENTITY_ID` | Object code for the card where the field is open. For example, `CRM_LEAD` ||
|| `FIELD_NAME` | Name of the custom field with the prefix `UF_CRM_` ||
|| `ENTITY_VALUE_ID` | Identifier of the item whose card is open. For a new card, `0` may be received ||
|| `VALUE` | Current field value. For a multiple field, an array of values ||
|| `MULTIPLE`, `MANDATORY` | Multiple and required field flags: `Y` or `N` ||
|| `XML_ID` | External code of the field ||
|#

Two placement commands are available from the handler — `setValue` and `getValue`. They are called using the [BX24.placement.call](../ui-interaction/bx24-placement-call.md) method:

```js
BX24.placement.call('setValue', value, () => {});
```

The `setValue` command writes the value into the card form. The value is retained in the database after the user saves the card.

## Relationship with Other Objects

A field type connects three topics: the CRM card where the field is displayed, the application that registered the type, and the widget mechanism through which the handler works.

**CRM.** Fields of a custom type are displayed in the cards of deals, leads, contacts, companies, new invoices, estimates, and SPAs. For the methods that create a field in each of these cards, see the article [{#T}](../../crm/universal/user-defined-fields/userfield-type.md).

**Application.** A field type belongs to the application that registered it, so a type cannot be created through an inbound webhook — the context of an [installed application](../../../settings/app-installation/index.md) is required. The [app.info](../../common/system/app-info.md) method returns the application `ID` for the full type code and shows in the `INSTALLED` field whether the installation is complete.

**Widgets.** `USERFIELD_TYPE` is one of the application placements. The other placements are listed in the article [{#T}](../placements.md), and the general embedding mechanism is described in the article [{#T}](../index.md).

## Limitations

- The `USER_TYPE_ID` code of a registered type cannot be changed: delete the type using the [userfieldtype.delete](./userfieldtype-delete.md) method and register it again. The handler address, name, description, and field height are modified by [userfieldtype.update](./userfieldtype-update.md)
- Use HTTPS for the handler, otherwise the browser will block the loading of the field content

The requirements for the type code and the handler address are listed in the parameters of the [userfieldtype.add](./userfieldtype-add.md) method. For what to do with the `Invalid user type specified` error when creating a field and with a field in which the handler does not load, see the article [{#T}](../../crm/universal/user-defined-fields/userfield-type.md).

## Overview of Methods {#all-methods}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [userfieldtype.add](./userfieldtype-add.md) | Registers a new custom field type ||
|| [userfieldtype.update](./userfieldtype-update.md) | Modifies the settings of a type registered by the application ||
|| [userfieldtype.list](./userfieldtype-list.md) | Returns a list of types registered by the application ||
|| [userfieldtype.delete](./userfieldtype-delete.md) | Deletes a type registered by the application ||
|#

## Continue Learning

- [{#T}](../../crm/universal/user-defined-fields/userfield-type.md)
- [{#T}](../../crm/universal/userfieldconfig/userfieldconfig-add.md)
- [{#T}](../ui-interaction/bx24-placement-call.md)
- [{#T}](../placements.md)
- [{#T}](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md)
