# Group Import of Records

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples in other languages are missing
- response in case of error is missing
- ask the developer (Zylyov) about the syntax

{% endnote %}

{% endif %}

{% note info "crm.item.batchImport" %}

**Scope**: [`crm`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

```http
crm.item.batchImport({entityTypeId: number, $data: ?{})
```

Group import of records.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **entityTypeId** | Identifier of the entity type ([What entities are supported?](index.md)) ||
|| **data** | Array of field values for the entities. It can be viewed as an array where each element contains a set of fields `fields`, as described in the method [crm.item.import](crm-item-import.md). ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

The method will return an array `data`, containing the same keys that were in the `data` request array. Each element of this array will contain the import result for a specific entity: an array `item` with the identifier of the created entity in case of success, or an error message.

The logic for adding entities works similarly to the method [crm.item.import](crm-item-import.md).

{% note warning "Attention!" %}

A maximum of 20 entities can be imported in a single request.

{% endnote %}

## Example

Importing a deal:

```json
{
    "entityTypeId": 2,
    "data": [
        {
            "title": "My Deal",
            "categoryId": 0
        },
        {
            "title": ""
        }
    ]
}
```

**Example result:**

```json
{
    "result": {
        "items": [
            {
                "item": {
                    "id": 15
                }
            },
            {
                "error": "CRM_FIELD_ERROR_REQUIRED",
                "error_description": "The field \"Title\" is required."
            }
        ]
    }
}
```

{% include [Note on examples](../../../../_includes/examples.md) %}