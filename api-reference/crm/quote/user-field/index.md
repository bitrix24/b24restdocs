# Custom Fields for Estimates: Overview of Methods

Custom fields store information about an estimate in various data formats: string, number, link, address, and others.

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

When creating or deleting custom fields, the request may be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). This is an internal server error. The cause of the error can be found in the server logs at the time the request was executed: 
* In cloud Bitrix24, submit a request to technical support to obtain error details. 
* In on-premise Bitrix24, request the server error log from the server administrator or hosting administrator. Then, contact technical support and attach the log for analysis.

### Common Causes of Server Errors

1. Up to 1016 custom fields can be created for estimates — this is a limitation of the database architecture. If there are already 1016 fields for estimates in Bitrix24, attempting to create a new field will result in the method [crm.quote.userfield.add](./crm-quote-user-field-add.md) returning an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). 

    You can check the number of custom fields for estimates using the method [crm.quote.userfield.list](./crm-quote-user-field-list.md). 

2. There is a limitation on servers for the execution time of a single request — `max_execution_time`. The standard time is 60 seconds. If the request takes longer, it will be interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md). 

    The time for [creating](./crm-quote-user-field-add.md) or [deleting](./crm-quote-user-field-delete.md) a custom field for an estimate depends on the number of estimates. When a field is created, it is added to all estimate detail forms. When a field is deleted, it is removed from all detail forms. The fewer estimates in your Bitrix24, the faster fields are created and deleted.
   
    To check the number of estimates in Bitrix24, use the method [crm.quote.list](../crm-quote-list.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the methods: depending on the method

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [crm.quote.userfield.add](./crm-quote-user-field-add.md) | Creates a new custom field for estimates ||
    || [crm.quote.userfield.update](./crm-quote-user-field-update.md) | Updates an existing custom field for estimates ||
    || [crm.quote.userfield.get](./crm-quote-user-field-get.md) | Returns a custom field for estimates by ID ||
    || [crm.quote.userfield.list](./crm-quote-user-field-list.md) | Returns a list of custom fields for estimates by filter ||
    || [crm.quote.userfield.delete](./crm-quote-user-field-delete.md) | Deletes a custom field for estimates ||
    |#

- Events 

    #| 
    || **Event** | **Triggered by** ||
    || [onCrmQuoteUserFieldAdd](./events/on-crm-quote-user-field-add.md) | When a custom field is added ||
    || [onCrmQuoteUserFieldUpdate](./events/on-crm-quote-user-field-update.md) | When a custom field is updated ||
    || [onCrmQuoteUserFieldDelete](./events/on-crm-quote-user-field-delete.md) | When a custom field is deleted ||
    || [onCrmQuoteUserFieldSetEnumValues](./events/on-crm-quote-user-field-set-enum-values.md) | When the set of values for a custom list-type field is changed ||
    |#

 {% endlist %}