# Event onCrmProductSectionDelete

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmProductSectionDelete` is triggered after a section is deleted.

## Response Handling

HTTP status: **200**

Returns an array:

```php
array('FIELDS' => array('ID' => $id))
```

## Error Handling

HTTP status: **400**

In case of an error or if the section is deleted **not in the CRM information block**, it throws the exception `\Bitrix\Rest\RestException`.