# Save User Calendar Settings calendar.user.settings.set

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

The method `calendar.user.settings.set` saves user calendar settings.

#|
|| **Parameter** | **Description** ||
|| **settings**^*^ | List of user settings.||
|#

{% include [Parameter Note](../../_includes/required.md) %}

## Example

```js
BX24.callMethod("calendar.user.settings.set",
    {
        settings: {
            tabId: 'month',
            meetSection: '23',
            blink: true,
            showDeclined: false,
            showMuted: true
        }
    }
);
```

{% include [Examples Note](../../_includes/examples.md) %}

## Response on Success

Returns **true** if the execution is successful.