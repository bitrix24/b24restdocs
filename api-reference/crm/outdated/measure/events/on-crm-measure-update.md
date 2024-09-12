# Event onCrmMeasureUpdate

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

The event `onCrmMeasureUpdate` is triggered after the unit of measurement is changed on the account.

## Response Handling

HTTP status: **200**

Returns an array:

```php
array('FIELDS' => array('ID' => $id))
```

where `$id` is the identifier of the modified unit of measurement.

## Error Handling

HTTP status: **40x**, **50x**

In case of errors, the exception `\Bitrix\Rest\RestException` is thrown.