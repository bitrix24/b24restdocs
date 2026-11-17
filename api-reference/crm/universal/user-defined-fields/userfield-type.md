# Custom Field Types in CRM

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

This article explains how an application registers its own field type, how to create a field of this type in a CRM detail form, and how to troubleshoot typical errors. How the handler works inside the field is described in [Custom Field Types: Overview of Methods](../../../widgets/user-field/index.md).

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Standard and Custom Types

A CRM field can have a standard or a custom type:

- standard types are string, number, date, list, address, link, file, and others. The complete list is returned by the [crm.userfield.types](./crm-userfield-types.md) method
- custom types are types that an application registers with the [userfieldtype.add](../../../widgets/user-field/userfieldtype-add.md) method. A field of such a type shows the application interface in the CRM detail form, which makes it a widget

If the value is plain text, a number, a date, or a choice from a list, a standard type is enough. A custom type is needed if the field must:

- display data that standard types do not fit. The value is stored in Bitrix24, and the application shows it and, if necessary, supplements it with data from its own service
- display controls, for example application buttons
- load data from an external service: each time the detail form is opened, the field sends a request to the application handler

## How a Custom Field Type Works

1. The application registers the type with the [userfieldtype.add](../../../widgets/user-field/userfieldtype-add.md) method: it passes the type code `USER_TYPE_ID` and the handler address `HANDLER`. The type code is stored in lowercase: `MyField` becomes `myfield`.
2. A field of this type is created with the full code `rest_<APP_ID>_<USER_TYPE_ID>`, where `APP_ID` is the `ID` value from the [app.info](../../../common/system/app-info.md) method response.
3. When a user opens the detail form, Bitrix24 loads `HANDLER` in the field frame and passes the field and item data in `PLACEMENT_OPTIONS`.
4. The handler passes the value to the detail form with the `setValue` command via [BX24.placement.call](../../../widgets/ui-interaction/bx24-placement-call.md). The value is written to Bitrix24 when the user saves the detail form.

The `userfieldtype.*` methods work only in the application context: when called via a webhook, they return the `WRONG_AUTH_TYPE` error. Only an administrator can call them. These conditions do not apply to creating a field. A field with the full type code can be created via a webhook or from another application — only the permissions and scopes from [How to Create a Custom Type Field](#create-field) are required.

## How to Create a Custom Type Field {#create-field}

A field is created with the universal [userfieldconfig.add](../userfieldconfig/userfieldconfig-add.md) method or with the `crm.*.userfield.add` method of the required object.

The `crm.*.userfield.add` methods are deprecated, but they create fields with application types. The REST 3.0 methods `crm.<type>.field.custom.add` that replace them do not create such fields.

#|
|| **Object** | **Field Creation Method** ||
|| [Deal](../../deals/index.md) | [crm.deal.userfield.add](../../deals/user-defined-fields/crm-deal-userfield-add.md) ||
|| [Lead](../../leads/index.md) | [crm.lead.userfield.add](../../leads/userfield/crm-lead-userfield-add.md) ||
|| [Contact](../../contacts/index.md) | [crm.contact.userfield.add](../../contacts/userfield/crm-contact-userfield-add.md) ||
|| [Company](../../companies/index.md) | [crm.company.userfield.add](../../companies/userfields/crm-company-userfield-add.md) ||
|| [Estimate](../../quote/index.md) | [crm.quote.userfield.add](../../quote/user-field/crm-quote-user-field-add.md) ||
|| [New invoice](../invoice.md) | Only [userfieldconfig.add](../userfieldconfig/userfieldconfig-add.md) ||
|| [SPA](../user-defined-object-types/index.md) | Only [userfieldconfig.add](../userfieldconfig/userfieldconfig-add.md) ||
|#

In the `crm.*.userfield.add` methods, the full code is passed in `fields.USER_TYPE_ID`; in `userfieldconfig.add`, in `field.userTypeId`. For example, for an application with `ID: 123` and the `userfield1` type, the code is `rest_123_userfield1`.

The `crm.*.userfield.add` methods require the `crm` scope, and the `userfieldconfig.add` method requires the `userfieldconfig` and `crm` scopes. All of these methods require the "Allow to modify settings" access permission in CRM.

## Errors When Working with Custom Field Types

### Error 400 When Creating a Field

When creating a field with a custom type, you may get the error `Error! 400: ERROR_CORE: The custom type is invalid. (400)`.

1. Call the [userfieldtype.list](../../../widgets/user-field/userfieldtype-list.md) method from the application that registered the type: the method returns only the types of the calling application.

   - If the required type is in the list, check the code in the field creation request. The full code `rest_<APP_ID>_<USER_TYPE_ID>` is required, where `USER_TYPE_ID` is the value from the `userfieldtype.list` response. If the code is correct, proceed to step 2.

   - If the required field type is not found, register a new type with the [userfieldtype.add](../../../widgets/user-field/userfieldtype-add.md) method.

2. Call the [app.info](../../../common/system/app-info.md) method. The method returns the `INSTALLED` flag, which indicates that the installation is complete.

   - If the method returns `INSTALLED = true`, the application is installed correctly.

   - If the method returns `INSTALLED = false`, call the [BX24.installFinish](../../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) function on the application page. It completes the installation of the application with an interface.

     ```javascript
     BX24.init(function(){
         BX24.installFinish();
     });
     ```

### The Handler Does Not Open in the Field

If the field has been created, but the handler does not open in it or shows an error:

1. Check the `HANDLER` URL specified when registering the field type with the [userfieldtype.list](../../../widgets/user-field/userfieldtype-list.md) method. To change `HANDLER`, use the [userfieldtype.update](../../../widgets/user-field/userfieldtype-update.md) method.

2. Make sure that the handler is accessible from the internet:

   - do not use local addresses: localhost, 192.168.*, and other addresses accessible only from the local network

   - check that the `HANDLER` address opens from a device outside your network

3. Check:

   - that the handler SSL certificate is valid

   - that `.htaccess` and the firewall on the handler server do not block requests

## Requirements and Recommendations

- The handler must be available over HTTPS: the Bitrix24 page is opened over HTTPS, and the browser blocks a frame loaded over HTTP.

- Give clear names to field types: consider the purpose of the field and its connection with the application.

- The `USER_TYPE_ID` type code does not change after registration: `userfieldtype.update` changes only the title, description, and handler. For a new code, delete the type and register it again.

## Overview of Methods {#all-methods}

> Scope: [`placement`](../../../scopes/permissions.md)
>
> Who can execute the methods: administrator, only in the application context

#|
|| **Method** | **Description** ||
|| [userfieldtype.add](../../../widgets/user-field/userfieldtype-add.md) | Registers a new type of custom field ||
|| [userfieldtype.update](../../../widgets/user-field/userfieldtype-update.md) | Modifies the parameters of an existing field type ||
|| [userfieldtype.list](../../../widgets/user-field/userfieldtype-list.md) | Returns the field types registered by the calling application ||
|| [userfieldtype.delete](../../../widgets/user-field/userfieldtype-delete.md) | Deletes a registered field type ||
|#

## Continue Learning

- [{#T}](../../../widgets/user-field/index.md)
- [{#T}](../../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md)
- [{#T}](../../../widgets/index.md)
