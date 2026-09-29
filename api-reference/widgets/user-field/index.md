# Custom Field Types: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An application can register its own custom field type — not the field itself, but its type and the handler address. When a user opens a card with a field of this type, Bitrix24 opens the handler in a frame inside the field, and subsequent interaction works the same way as with a regular widget.

In the Bitrix24 cloud, such fields work in the CRM card. The [crm.userfield.types](../../crm/universal/user-defined-fields/crm-userfield-types.md) method shows an application the standard types and only its own custom types. However, a field with the full type code `rest_<APP_ID>_<USER_TYPE_ID>` can also be created from another application or through an administrator's webhook.

> Quick navigation: [all methods](#all-methods)

## Getting Started

1. Register the field type using the [userfieldtype.add](./userfieldtype-add.md) method. Pass the short type code `USER_TYPE_ID` and the public handler address `HANDLER`. The method converts the code to lowercase, so pass it in lowercase right away

2. Retrieve the application `ID` using the [app.info](../../common/system/app-info.md) method and assemble the full type code in the `rest_<APP_ID>_<USER_TYPE_ID>` format. For example, for an application with `ID: 123` and `USER_TYPE_ID: phone_data`, the full code will be `rest_123_phone_data`

3. Create a field with this type. In the universal [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig-add.md) method, pass the full code in `field.userTypeId`; in an object-specific CRM method, for example [crm.lead.userfield.add](../../crm/leads/userfield/crm-lead-userfield-add.md), pass it in `fields.USER_TYPE_ID`. For this step, the application needs the `crm` scope, and for the universal method, the `userfieldconfig` scope as well

4. Add the field to the card form and open the card — the handler will load inside the field

5. Parse `PLACEMENT_OPTIONS` in the handler and write the field value using the `setValue` command

For the complete scenario with the handler code, see the tutorial [{#T}](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md).

## What the Handler Receives {#handler-data}

The handler opens in the `USERFIELD_TYPE` placement. There is no need to register it using the [placement.bind](../placement-bind.md) method: the [userfieldtype.add](./userfieldtype-add.md) method binds the handler to this placement itself.

Along with the request, Bitrix24 passes `PLACEMENT_OPTIONS` to the handler — data about the field and the item in whose card the field is open. In the HTTP request, the data is passed as a JSON string; in the b24jssdk library, the same data is available as the `$b24.placement.options` object.

#|
|| **Key** | **Contents** ||
|| `MODE` | Field display mode: `view` or `edit` ||
|| `ENTITY_ID` | Object code for the card where the field is open. For example, `CRM_LEAD` ||
|| `FIELD_NAME` | Name of the custom field with the prefix `UF_CRM_` ||
|| `ENTITY_VALUE_ID` | Identifier of the item whose card is open. For a new card, `0` is received ||
|| `VALUE` | Current field value. For a multiple field, an array of values ||
|| `MULTIPLE`, `MANDATORY` | Multiple and required field flags: `Y` or `N` ||
|| `XML_ID` | External code of the field ||
|| `ENTITY_DATA` | CRM card object: `entityTypeId` — [Object Type Identifier](../../crm/data-types.md#object_type), `entityId` — item identifier, `module` — `crm` ||
|| `URI` | Address of the open card ||
|#

The rest of the request data — user authorization and application parameters — is the same as for other placements. For what the full request looks like, see the article [{#T}](../index.md); for `PLACEMENT_OPTIONS` of a field in a deal card, see the [userfieldtype.add](./userfieldtype-add.md) method page.

Two placement commands are available from the handler — `setValue` and `getValue`. They are called using the [BX24.placement.call](../ui-interaction/bx24-placement-call.md) method:

```js
BX24.placement.call('setValue', value, () => {});
```

In edit mode, when `MODE` is `edit`, the `setValue` command writes the value into the card form. The value is retained in the database after the user saves the card.

## Relationship with Other Objects

A field type connects three topics: the CRM card where the field is displayed, the application that registered the type, and the widget mechanism through which the handler works.

**CRM.** Fields of a custom type are displayed in the cards of deals, leads, contacts, companies, new invoices, estimates, and SPAs. For the methods that create a field in each of these cards, see the article [{#T}](../../crm/universal/user-defined-fields/userfield-type.md).

**Application.** A field type belongs to the application that registered it, so a type cannot be created through an inbound webhook — the context of an [installed application](../../../settings/app-installation/index.md) is required. The [app.info](../../common/system/app-info.md) method returns the application `ID` for the full type code and shows in the `INSTALLED` field whether the installation is complete. Until the installation is complete, the handler does not load in the field.

**Widgets.** `USERFIELD_TYPE` is one of the application placements. The other placements are listed in the article [{#T}](../placements.md), and the general embedding mechanism is described in the article [{#T}](../index.md).

## Limitations

- The `USER_TYPE_ID` code of a registered type cannot be changed: a new code means a new type, and fields for it will have to be created again. The handler address, name, description, and field height are modified by [userfieldtype.update](./userfieldtype-update.md)
- If Bitrix24 runs over HTTPS, use HTTPS for the handler as well, otherwise the browser will block the loading of the field content

The requirements for the type code and the handler address are listed in the parameters of the [userfieldtype.add](./userfieldtype-add.md) method.

## Common Errors

#|
|| **What Happens** | **Cause** | **What to Do** ||
|| [`userfieldtype.*` Methods](#all-methods) return `WRONG_AUTH_TYPE` | The method is called through a webhook, but field types are registered only by an application | Call the methods with the token of an installed application ||
|| [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig-add.md) returns `The custom type is invalid` | The short type code is passed in `userTypeId`, or the type has been deleted | Pass the full code `rest_<APP_ID>_<USER_TYPE_ID>` and check the type using the [userfieldtype.list](./userfieldtype-list.md) method from the application that registered it ||
|| [userfieldtype.add](./userfieldtype-add.md) returns `ERROR_CORE` with the text `Unable to set placement handler: Handler already binded` | The type code is already registered in Bitrix24, or another type of this application has the same handler address | Choose a different code or address ||
|| The field has disappeared from the card, and CRM methods do not return its value | The field type was deleted using the [userfieldtype.delete](./userfieldtype-delete.md) method | Register the type with the same code from the same application — the fields and values will be restored ||
|#

For how to create a field in different CRM cards and what to do if the handler does not load in the field, see the article [{#T}](../../crm/universal/user-defined-fields/userfield-type.md).

## Overview of Methods {#all-methods}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [userfieldtype.add](./userfieldtype-add.md) | Registers a new custom field type and returns `true` ||
|| [userfieldtype.update](./userfieldtype-update.md) | Modifies the settings of a type registered by the application and returns `true` ||
|| [userfieldtype.list](./userfieldtype-list.md) | Retrieves the types registered by the current application, no more than 50 per call ||
|| [userfieldtype.delete](./userfieldtype-delete.md) | Deletes a type registered by the application and returns `true` ||
|#

## Continue Learning

- [{#T}](../../crm/universal/user-defined-fields/userfield-type.md)
- [{#T}](../../crm/universal/userfieldconfig/userfieldconfig-add.md)
- [{#T}](../ui-interaction/bx24-placement-call.md)
- [{#T}](../placements.md)
- [{#T}](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md)
