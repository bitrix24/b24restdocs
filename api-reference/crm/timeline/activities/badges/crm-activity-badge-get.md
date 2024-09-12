# Get Badge by Code crm.activity.badge.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.badge.get` will return an array containing [badge fields](./index.md), which means information about the badge.

## Parameters

#|
|| **Parameter** | **Description** | **Type** ||
|| **code**
[`string`](../../../../data-types.md)
| Badge code | ||
|#

## Example Response on Success

> 200 OK
```json
{
    "code": "missedCall",
    "title": {
        "ru": "Call status",
        "en": "Call status"
    },
    "value": "Missed",
    "type": "failure"
}
```