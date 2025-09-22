# Custom Field Types in CRM

In CRM, you can create fields of two types:
- standard: number, string, date, address, link, file, and so on,
- custom: application integrations within the CRM detail form.

With custom field types, you can:

- display data in the CRM detail form that does not fit standard field types. Essentially, the data will be stored in the application database in the required format, and the integration will show it within the field.
- create interface elements in CRM detail forms. For example, display buttons within a field to control the application.
- integrate external services into the CRM detail form. For instance, display dynamic information in a field. Each time the detail form is opened, the field will make a request to the application handler and automatically load fresh data.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Working with custom fields](https://helpdesk.bitrix24.com/open/22067852/)

## Connection with CRM Objects

Custom field types can be added to:
- [deals](../../deals/index.md) — use the methods [crm.deal.userfield.add](../../deals/user-defined-fields/crm-deal-userfield-add.md) or [userfieldconfig.add](../userfieldconfig/userfieldconfig/userfieldconfig-add.md),
- [leads](../../leads/index.md) — [crm.lead.userfield.add](../../leads/userfield/crm-lead-userfield-add.md) or [userfieldconfig.add](../userfieldconfig/userfieldconfig/userfieldconfig-add.md),
- [contacts](../../contacts/index.md) — [crm.contact.userfield.add](../../contacts/userfield/crm-contact-userfield-add.md) or [userfieldconfig.add](../userfieldconfig/userfieldconfig/userfieldconfig-add.md),
- [companies](../../companies/index.md) — [crm.company.userfield.add](../../companies/userfields/crm-company-userfield-add.md) or [userfieldconfig.add](../userfieldconfig/userfieldconfig/userfieldconfig-add.md),
- [new invoices](../invoice.md) — [userfieldconfig.add](../userfieldconfig/userfieldconfig/userfieldconfig-add.md),
- [estimates](../../quote/index.md) — [crm.quote.userfield.add](../../quote/user-field/crm-quote-user-field-add.md) or [userfieldconfig.add](../userfieldconfig/userfieldconfig/userfieldconfig-add.md),
- [SPAs](../index.md) — [userfieldconfig.add](../userfieldconfig/userfieldconfig/userfieldconfig-add.md).

In the `USER_TYPE_ID` field, pass the value in the form `rest_#ID_application#_#USER_TYPE_ID#`. For example, for an application with `ID: 123` and `USER_TYPE_ID: userfield1`, the value will be `rest_123_test_userfield1`.

To get the application `ID`, use the method [app.info](../../../common/system/app-info.md).

## Errors When Working with Custom Field Types

### Error 400 When Creating a Field

When creating a field with a custom type, you may encounter the error `Error! 400: ERROR_CORE: Invalid user type specified. (400)`.

1. Execute the method [userfieldtype.list](../../../widgets/user-field/userfieldtype-list.md).

   - If the method returned the field type `USER_TYPE_ID`, proceed to step 2.

   - If the required field type is not found, register a new type using the method [userfieldtype.add](../../../widgets/user-field/userfieldtype-add.md).

2. Execute the method [app.info](../../../common/system/app-info.md). This method will check the correctness of the application installation.

   - If the method returned `INSTALLED = true`, the application is installed correctly.

   - If the method returned `INSTALLED = false`, execute the method [BX24.installFinish](../../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) on the application page. This method will complete the installation of the application with the interface.

    ```javascript
    BX24.init(function(){
        BX24.installFinish();
    });
    ```

### Error 50x When Loading a Field

If the field was created without errors, but the content is not loading:

1. Check the URL of the handler `HANDLER` specified during field registration using the method [userfieldtype.list](../../../widgets/user-field/userfieldtype-list.md). To change the `HANDLER`, use the method [userfieldtype.update](../../../widgets/user-field/userfieldtype-update.md).

2. Ensure that the handler is accessible from the internet:

   - do not use local addresses: localhost, 192.168.* and other addresses accessible only from the local network,

   - check the handler's accessibility using public "site availability" services.

3. Check:

   - the correctness of the SSL certificate if HTTPS is used,

   - the absence of blocks in .htaccess or firewall on the handler server,

   - the returned HTTP codes, which should be 200 OK.

## General Recommendations

- Use the HTTPS protocol for handlers; otherwise, browsers may block the loading of application content.

- Give clear names to field types: consider the purpose of the field and its connection with the application. The field type cannot be renamed after creation — it can only be deleted and registered again.

{% note tip "Typical use-cases and scenarios" %}

-  [Embed a widget in a lead as a custom property](../../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page)

-  [Widget embedding mechanism](../../../widgets/index)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`placement, crm`](../../../scopes/permissions.md)
> 
> Who can execute the method: administrator

#|
|| **Method** | **Description** ||
|| [userfieldtype.add](../../../widgets/user-field/userfieldtype-add.md) | Registers a new type of custom field ||
|| [userfieldtype.update](../../../widgets/user-field/userfieldtype-update.md) | Modifies the parameters of an existing field type ||
|| [userfieldtype.list](../../../widgets/user-field/userfieldtype-list.md) | Returns a list of registered field types ||
|| [userfieldtype.delete](../../../widgets/user-field/userfieldtype-delete.md) | Deletes a registered field type ||
|#