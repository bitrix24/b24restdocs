# Set Participation Status in Event for Current User calendar.meeting.status.set

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

The method `calendar.meeting.status.set` sets the participation status in an event for the current user.

#|
|| **Parameter** | **Description** ||
|| **eventId** | Event identifier. ||
|| **status** | Status: 
- Y - confirmed participation; 
- N - declined; 
- Q - tentative. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.meeting.status.set",
        {
            eventId: '651',
            status: 'Y'
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../_includes/examples.md) %}

## Response on Success

Returns **true** if the status was set successfully.