# View the list of upcoming events for the current user calendar.event.get.nearest

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.event.get.nearest` returns a list of upcoming events for the current user.

#|
|| **Parameter** | **Description** ||
|| **type** | Calendar type: 
- user; 
- group. ||
|| **ownerId** | Identifier of the calendar owner. ||
|| **days** | Number of days for selection (default - 60). ||
|| **forCurrentUser** | Output the list of events for the current user. ||
|| **maxEventsCount** | Maximum number of events to display. ||
|| **detailUrl** | URL for the calendar. ||
|#

## Example

```js
BX24.callMethod("calendar.event.get.nearest",
    {
        type: 'user',
        ownerId: '2',
        days: 10,
        forCurrentUser: true,
        detailUrl: '/company/personal/user/#user_id#/calendar/'
    }
);
```

{% include [Footnote on examples](../../_includes/examples.md) %}