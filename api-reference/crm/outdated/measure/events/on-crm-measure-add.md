# Event for Adding a New Unit of Measurement onCrmMeasureAdd

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event `onCrmMeasureAdd` is triggered after a new unit of measurement is added to the account.

## Response Handling

HTTP status: **200**

Returns an array:

```php
array('FIELDS' => array('ID' => $id))
```

where `$id` is the identifier of the created unit of measurement.

## Error Handling

HTTP status: **40x**, **50x**

In case of errors, the exception `\Bitrix\Rest\RestException` is thrown.