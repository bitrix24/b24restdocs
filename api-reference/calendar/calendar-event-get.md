# Get the list of calendar events calendar.event.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.event.get` returns a list of calendar events.

#|
|| **Parameter** | **Description** ||
|| **type**^*^ | Calendar type: 
- user; 
- group. ||
|| **ownerId** | Identifier of the calendar owner. ||
|| **from** | Start date of the selection. Default value - one month before the current date. ||
|| **to** | End date of the selection. Default value - three months after the current date. ||
|| **section** | Array of sections. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

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

{% endlist %}

Get events from the company calendar:

{% list tabs %}

- JS

    ```js
    'type'=> 'company_calendar',
    'ownerId' => '' // ownerId is not specified when retrieving events from the company calendar. It is empty for all events of this type.
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}