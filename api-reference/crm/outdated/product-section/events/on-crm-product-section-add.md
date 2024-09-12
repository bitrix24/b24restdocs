# Event for Adding a New Product Section onCrmProductSectionAdd

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmProductSectionAdd` is triggered after a section is added.

## Handling the Response

HTTP status: **200**

Returns an array:

```php
array('FIELDS' => array('ID' => $id))
```

## Error Handling

HTTP status: **400**

In case of an error or if the section is created **not in the CRM information block**, it throws the exception `\Bitrix\Rest\RestException`.