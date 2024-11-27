# View Calendar Event by ID calendar.event.getbyid

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

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

The method `calendar.event.getbyid` returns a calendar event by its ID.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Number. Returns an array with the fields of the event entity or `null`. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod("calendar.event.getbyid", {id: 324});
    /*
        * Returns event by its id
        *
        * @param array $params - incoming params:
        * $params['id'] - int, (required) calendar event id
        * @return event or null
        * @throws \Bitrix\Rest\RestException
        *
        * @example (Javascript)
        * BX24.callMethod("calendar.event.getbyid",
        * {
        *     id: 324
        * });
        *
        */
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}