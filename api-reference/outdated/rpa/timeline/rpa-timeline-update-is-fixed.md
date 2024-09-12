# Update the attachment flag rpa.timeline.updateIsFixed

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- the requiredness of parameters is not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.timeline.updateIsFixed` updates the attachment flag of the record.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id** 
[`number](../../../data-types.md) | Identifier of the record. ||
|| **isFixed** 
[`string`](../../../data-types.md) | Attachment flag of the record. If y - the record will be attached, otherwise - it will not. ||
|#

## Response on success

> 200 OK

```json
{
    "timeline": {
        "id": 322,
        ...
    }
}
```