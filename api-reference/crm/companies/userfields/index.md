# Custom Company Fields: Overview of Methods

Custom fields store information about a company in various data formats: string, number, link, address, and others.

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
                    [title] => Mandatory
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

When creating or deleting custom fields, the request may be interrupted with an [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error. The cause of the error can be found in the server logs at the time of the request:
* In cloud Bitrix24, submit a request to technical support to obtain error details.
* In on-premise Bitrix24, request the server error log from the server administrator or hosting administrator. Then contact technical support and attach the log for analysis.

### Common Causes of Server Errors

1. Up to 1016 custom fields can be created for companies—this is a limitation of the database architecture. If there are already 1016 fields for companies in Bitrix24, attempting to create a new field will result in an [INTERNAL_SERVER_ERROR](../../../../error-codes.md) from the method [crm.company.userfield.add](./crm-company-userfield-add.md).

    You can check the number of custom fields for companies using the method [crm.company.userfield.list](./crm-company-userfield-list.md).

2. There is a limitation on servers for the execution time of a single request—`max_execution_time`. The standard time is 60 seconds. If the request takes longer, it will be interrupted with an [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

   The time to create or delete a custom field for a company depends on the number of companies. When a field is created, it is added to all company detail forms. When a field is deleted, it is removed from all detail forms. The fewer companies in your Bitrix24, the faster fields are created and deleted.
   
   To check the number of companies in Bitrix24, use the method [crm.company.list](../crm-company-list.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform methods: depending on the method

#| 
|| **Method** | **Description** ||
|| [crm.company.userfield.add](./crm-company-userfield-add.md) | Creates a new custom field for companies ||
|| [crm.company.userfield.get](./crm-company-userfield-get.md) | Returns a custom field for companies by ID ||
|| [crm.company.userfield.list](./crm-company-userfield-list.md) | Returns a list of custom fields for companies by filter ||
|| [crm.company.userfield.update](./crm-company-userfield-update.md) | Updates an existing custom field for companies ||
|| [crm.company.userfield.delete](./crm-company-userfield-delete.md) | Deletes a custom field for companies ||
|#