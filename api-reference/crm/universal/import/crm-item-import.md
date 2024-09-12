# Importing a Single Record

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requiredness of parameters is not specified
- examples in other languages are missing
- response in case of error is absent

{% endnote %}

{% endif %}

{% note info "crm.item.import" %}

**Scope**: [`crm`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

```http
crm.item.import({entityTypeId: number, fields: ?{}})
```

Importing a single record.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **entityTypeId** | Identifier of the entity type ([What entities are supported?](index.md)) ||
|| **fields** | Values of the entity fields. You can find out the set of these fields using the REST method [crm.item.fields](../crm-item-fields.md), or use methods for specific types: [crm.lead.fields](../../leads/crm-lead-fields.md), [crm.deal.fields](../../deals/crm-deal-fields.md), [crm.contact.fields](../../contacts/crm-contact-fields.md), [crm.company.fields](../../companies/crm-company-fields.md), [crm.quote.fields](.). ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

The method will return an array `item` with the identifier of the created entity in case of success, or an error message.

To upload a file, you need to pass an array as the value of the custom field, where the first element is the file name, and the second is the base64 encoded content of the file.

More details about the differences between import logic and regular entity addition logic can be found [here](.).

## Example

Importing a deal:

```json
{
    "entityTypeId": 2,
    "fields": {
        "title": "My Deal",
        "categoryId": 0
    }
}
```

**Successful result:**

```json
{
    "result": {
        "item": {
            "id": 15
        }
    }
}
```

{% include [Examples Note](../../../../_includes/examples.md) %}