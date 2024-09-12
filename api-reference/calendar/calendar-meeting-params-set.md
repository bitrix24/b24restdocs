# Set Event Parameters for the Current User calendar.meeting.params.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.meeting.params.set` sets the event parameters for the current user if they are a participant.

#|
|| **Parameter** | **Description** ||
|| **eventId** | Event identifier. ||
|| **accessibility** | Availability during the event: 
- busy; 
- absent; 
- quest; 
- free. ||
|| **remind** | Event reminder: 
- type - time type of reminder (min, hour, day); 
- count - numerical value of the time interval. ||
|#

## Example

```js
BX24.callMethod("calendar.meeting.params.set",
    {
        eventId: '651',
        accessibility: 'free',
        remind: [{type: 'min', count: 20}]
    }
);
```

{% include [Footnote on examples](../../_includes/examples.md) %}

## Response on Success

Returns **true** if the execution is successful.