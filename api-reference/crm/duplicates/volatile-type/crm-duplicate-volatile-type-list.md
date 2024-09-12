# Get a list of custom fields involved in duplicate search crm.duplicate.volatileType.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing response in case of success
- missing response in case of error
- required fields are not specified
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
crm.duplicate.volatileType.list()
```

The method will return an array containing array elements with the following keys:

#|
|| **Key** | **Description** ||
|| **id** | Identifier of the duplicate type ||
|| **entityTypeId** | Identifier of the entity type ||
|| **fieldCode** | Field code ||
|#

## Parameters

No parameters.

## Example

```json
[
    {
        "id": 33554432,
        "entityTypeId": 1,
        "fieldCode": "TITLE"
    },
    {
        "id": 67108864,
        "entityTypeId": 3,
        "fieldCode": "UF_CRM_1620140992555"
    }
]
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}