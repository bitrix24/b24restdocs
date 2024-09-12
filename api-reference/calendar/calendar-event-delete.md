# Delete Event calendar.event.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.event.delete` removes an event.

#|
|| **Parameter** | **Description** ||
|| **type**^*^ | Calendar type: 
- user; 
- group. ||
|| **ownerId**^*^ | Identifier of the calendar owner. ||
|| **id**^*^ | Identifier of the event. ||
|#

{% include [Note on parameters](../../_includes/required.md) %}

## Example

```js
BX24.callMethod("calendar.event.delete",
    {
        id: 698,
        type: 'user',
        ownerId: '2'
    }
);
```

{% include [Note on examples](../../_includes/examples.md) %}

## Response on Success

Returns **true** if the deletion is successful.