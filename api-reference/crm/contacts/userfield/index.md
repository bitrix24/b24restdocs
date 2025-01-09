# Custom Contact Fields: Overview of Methods

Custom fields store information about a contact in various data formats: string, number, link, address, and others.

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

## Types of Custom Fields

Use the method [crm.userfield.types](../../universal/user-defined-fields/crm-userfield-types.md) to retrieve the available types of custom fields. The method will return the ID and name of the field types.

```` 
    (
        [ID] => double    
        [title] => Number
    )
````

Use the method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md) to get a list of characteristics of custom fields. The method will return the codes of the characteristics along with their type and name.

```` 
    [MANDATORY] => Array
                (
                    [type] => char
                    [title] => Required
                )
````

## Custom Field Settings

Use the method [crm.userfield.settings.fields](../../universal/user-defined-fields/crm-userfield-settings-fields.md) to obtain a list of available settings. The method will return the supported settings for the requested field type.

```` 
    [DEFAULT_VALUE] => Array
            (
                [type] => double
                [title] => Default value
            )
    [PRECISION] => Array
            (
                [type] => int
                [title] => Precision
            )
````

## Errors When Working with Custom Fields

When creating or deleting custom fields, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error. The cause of the error can be found in the server logs at the time of the request:
* In cloud Bitrix24, submit a request to technical support to obtain error details.
* In on-premise Bitrix24, request the server error log from the server administrator or hosting administrator. Then, contact technical support and attach the log for analysis.

### Common Causes of Server Errors

1. You can create 1016 custom fields for contacts — this is a database architecture limitation. If there are already 1016 fields for contacts in Bitrix24, attempting to create a new field will result in the method [crm.contact.userfield.add](./crm-contact-userfield-add.md) returning an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    You can check the number of custom fields for contacts using the method [crm.contact.userfield.list](./crm-contact-userfield-list.md).

2. There is a limitation on servers for the execution time of a single request — `max_execution_time`. The standard time is 60 seconds. If the request takes longer, it is interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    The time for [creating](./crm-contact-userfield-add.md) or [deleting](./crm-contact-userfield-delete.md) a custom contact field depends on the number of contacts. When a field is created, it is added to all contact detail forms. When a field is deleted, it is removed from all detail forms. The fewer contacts you have in your Bitrix24, the faster fields are created and deleted.

    To check the number of contacts in Bitrix24, use the method [crm.contact.list](../crm-contact-list.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform methods: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.contact.userfield.add](./crm-contact-userfield-add.md) | Creates a new custom field for contacts ||
|| [crm.contact.userfield.update](./crm-contact-userfield-update.md) | Updates an existing custom field for contacts ||
|| [crm.contact.userfield.get](./crm-contact-userfield-get.md) | Returns a custom field for contacts by ID ||
|| [crm.contact.userfield.list](./crm-contact-userfield-list.md) | Returns a list of custom fields for contacts by filter ||
|| [crm.contact.userfield.delete](./crm-contact-userfield-delete.md) | Deletes a custom field for contacts || 
|#