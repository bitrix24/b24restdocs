# Provide the ability to select resource bookings calendar.resource.booking.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

The method `calendar.resource.booking.list` provides the ability to select resource bookings.

#| 
|| **Parameter** | **Description** ||
|| **filter**^*^ | Filter fields. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Examples

**First option:** to assess bookings (availability) of specific resources over a certain period. It can be used to create custom availability views or for use in logic.

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.resource.booking.list", {
        filter: {
            resourceTypeIdList: [10852, 10888, 10873, 10871, 10853], // a list of resource IDs that can be selected using the calendar.resource.list method
            from: '2018-06-20',
            to: '2018-08-20',
        }
    });
    ```

{% endlist %}

**Second option:** the ability to select bookings by their IDs (these are the values of the UF field associated with the CRM entity).

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.resource.booking.list", {
        filter: {
            resourceIdList: [10, 18, 17] // these IDs are taken from the UF field value of type resourcebooking for CRM entities LEAD|DEAL
        }
    });
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}

## Response in case of success

Returns data about each booking. Bookings have identical fields to events, as they are essentially events.