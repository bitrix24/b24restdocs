# Custom Fields for Deals: Overview of Methods

Custom fields store information about a deal in various data formats: string, number, link, address, and others.

> Quick navigation: [all methods](#all-methods) 
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
                    [title] => Mandatory
                )
````

## Settings for Custom Fields

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
* In the cloud Bitrix24, submit a request to [technical support](../../../../bitrix-support.md) to get details about the error.
* In the on-premise Bitrix24, request the server error log from the server administrator or hosting administrator. Then, contact [technical support](../../../../bitrix-support.md) and attach the log for analysis.

### Common Causes of Server Errors

1. You can create 1016 custom fields for deals — this is a limitation of the database architecture. If there are already 1016 fields for deals in Bitrix24, attempting to create a new field will result in the method [crm.deal.userfield.add](./crm-deal-userfield-add.md) returning an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    You can check the number of custom fields for deals using the method [crm.deal.userfield.list](./crm-deal-userfield-list.md).

2. There is a limitation on servers for the execution time of a single request — `max_execution_time`. The standard time is 60 seconds. If the request takes longer, it is interrupted with an error [INTERNAL_SERVER_ERROR](../../../../error-codes.md).

    The time for [creating](./crm-deal-userfield-add.md) or [deleting](./crm-deal-userfield-delete.md) a custom field for deals depends on the number of deals. When a field is created, it is added to all deal detail forms. When a field is deleted, it is removed from all detail forms. The fewer deals in your Bitrix24, the faster fields are created and deleted.

    To check the number of deals in Bitrix24, use the method [crm.deal.list](../crm-deal-list.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the methods: depending on the method

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [crm.deal.userfield.add](./crm-deal-userfield-add.md) | Creates a new custom field for deals ||
    || [crm.deal.userfield.update](./crm-deal-userfield-update.md) | Modifies an existing custom field for deals ||
    || [crm.deal.userfield.get](./crm-deal-userfield-get.md) | Retrieves a custom field for deals by Id ||
    || [crm.deal.userfield.list](./crm-deal-userfield-list.md) | Gets a list of custom fields for deals ||
    || [crm.deal.userfield.delete](./crm-deal-userfield-delete.md) | Deletes a custom field for deals ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [onCrmDealUserFieldAdd](./events/on-crm-deal-user-field-add.md) | When a custom field is added ||
    || [onCrmDealUserFieldUpdate](./events/on-crm-deal-user-field-update.md) | When a custom field is modified ||
    || [onCrmDealUserFieldDelete](./events/on-crm-deal-user-field-delete.md) | When a custom field is deleted ||
    || [onCrmDealUserFieldSetEnumValues](./events/on-crm-deal-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed ||
    |#

{% endlist %}