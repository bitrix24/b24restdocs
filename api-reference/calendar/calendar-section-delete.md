# Delete Calendar calendar.section.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `calendar.section.delete` removes a calendar. Here and further, section will be referred to as "calendar".

#|
|| **Parameter** | **Description** ||
|| **type**^*^ | Calendar type: 
- user; 
- group. ||
|| **ownerId**^*^ | Identifier of the calendar owner. ||
|| **id**^*^ | Identifier of the calendar. ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Example

```js
BX24.callMethod("calendar.section.delete",
    {
        type: 'user',
        ownerId: '2',
        id: 521
    }
);
```

{% include [Example Notes](../../_includes/examples.md) %}

## Response on Success

Returns **true** if the deletion is successful.