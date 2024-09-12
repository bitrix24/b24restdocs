# View Calendar Event by ID calendar.event.getbyid

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

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

{% include [Parameter Note](../../_includes/required.md) %}

## Example

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

{% include [Example Note](../../_includes/examples.md) %}