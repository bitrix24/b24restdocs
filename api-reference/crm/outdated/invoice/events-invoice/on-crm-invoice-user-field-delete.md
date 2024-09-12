# Event for Deleting a Custom Field onCrmInvoiceUserFieldDelete

The event is triggered when a custom field is deleted.

## Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../../data-types.md)| Identifier of the custom field ||
|| **entityId** 
[`string`](../../../../data-types.md)| Symbolic identifier of the entity for which the field was created ||
|| **fieldName** 
[`string`](../../../../data-types.md)| Name of the created custom field ||
|#