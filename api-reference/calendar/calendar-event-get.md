# Get Calendar Events List calendar.event.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `calendar.event.get` method returns a list of calendar events.

#|
|| **Parameter** | **Description** ||
|| **type**^*^ | Calendar type: 
- user; 
- group. ||
|| **ownerId** | Identifier of the calendar owner. ||
|| **from** | Start date of the selection. Default value is one month before the current date. ||
|| **to** | End date of the selection. Default value is three months after the current date. ||
|| **section** | Array of sections. ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Example

```js
BX24.callMethod("calendar.event.get",
    {
        type: 'user',
        ownerId: '1',
        from: '2013-06-20',
        to: '2013-08-20',
        section: [21, 44]
    }
);
```

Get company calendar events:

```js
'type'=> 'company_calendar',
'ownerId' => '' // ownerId is not specified when retrieving company calendar events. It is empty for all events of this type.
```

{% include [Example Notes](../../_includes/examples.md) %}