# View User Availability from calendar.accessibility.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.accessibility.get` returns the availability of users from the list.

#|
|| **Parameter** | **Description** ||
|| **users**^*^ | List of user IDs. ||
|| **from**^*^ | Start date of the period for determining availability. ||
|| **to**^*^ | End date of the period for determining availability. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

```js
BX24.callMethod("calendar.accessibility.get",
    {
        from: '2013-06-20',
        to: '2013-12-20',
        users: [1, 2, 34]
    }
);
```

{% include [Footnote about examples](../../_includes/examples.md) %}

## Successful Response

Returns information about the availability for each requested user.