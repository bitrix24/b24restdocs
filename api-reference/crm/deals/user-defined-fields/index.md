# Custom Deal Fields: Overview of Methods

Custom fields store information about a deal in various data formats: string, number, link, address, and others.

> Quick Navigation: [All Methods](#all-methods) 
> 
> User Documentation: [Custom Fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

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
                [title] => Default Value
            )
    [PRECISION] => Array
            (
                [type] => int
                [title] => Precision
            )
````

## Errors When Working with Custom Fields

When creating or deleting custom fields, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error. The cause of the error can be found in the server logs at the time of the request:
* In the cloud version of Bitrix24, contact technical support to obtain error details.
* In the on-premise version of Bitrix24, request the server error log from the server administrator or hosting administrator. Then, contact technical support and attach the log for analysis.

### Common Causes of Server Errors

1. You can create 1016 custom fields for deals — this is a database architecture limitation. If there are already 1016 fields for deals in Bitrix24, attempting to create a new field will result in the method [crm.deal.userfield.add](./crm-deal-userfield-add.md) returning an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    You can check the number of custom fields for deals using the method [crm.deal.userfield.list](./crm-deal-userfield-list.md).

2. There is a limitation on servers for the execution time of a single request — `max_execution_time`. The standard time is 60 seconds. If the request takes longer, it will be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    The time for [creating](./crm-deal-userfield-add.md) or [deleting](./crm-deal-userfield-delete.md) a custom field for deals depends on the number of deals. When a field is created, it is added to all deal detail forms. When a field is deleted, it is removed from all detail forms. The fewer deals you have in your Bitrix24, the faster fields are created and deleted.

    To check the number of deals in Bitrix24, use the method [crm.deal.list](../crm-deal-list.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.deal.userfield.add](./crm-deal-userfield-add.md) | Creates a new custom field for deals ||
|| [crm.deal.userfield.update](./crm-deal-userfield-update.md) | Modifies an existing custom field for deals ||
|| [crm.deal.userfield.get](./crm-deal-userfield-get.md) | Returns a custom field for deals by ID ||
|| [crm.deal.userfield.list](./crm-deal-userfield-list.md) | Returns a list of custom fields for deals based on a filter ||
|| [crm.deal.userfield.delete](./crm-deal-userfield-delete.md) | Deletes a custom field for deals ||
|#