# Get the participation status of the current user in the event calendar.meeting.status.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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

The method `calendar.meeting.status.get` returns the participation status of the current user in the event.

#|
|| **Parameter** | **Description** ||
|| **eventId**^*^ | Event identifier. ||
|#

{% include [Parameter Note](../../_includes/required.md) %}

## Example

```js
BX24.callMethod("calendar.meeting.status.get",
    {
        eventId: '651'
    }
);
```

{% include [Examples Note](../../_includes/examples.md) %}

## Successful Response

Returns the status ("Y", "N", "Q").