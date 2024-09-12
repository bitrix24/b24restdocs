# Get a list of fields available for use in crm.duplicate.volatileType.fields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

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
crm.duplicate.volatileType.fields({?entityTypeId: number})
```

The method will return an array containing array elements with the following keys:

#|
|| **Key** | **Description** ||
|| **entityTypeId**
| Entity type identifier. Can take values `1` (lead), `3` (contact), or `4` (company). Not a required parameter. If not specified, available fields for all entities will be returned. ||
|#

## Parameters

#|
|| **Parameter** | **Description** ||
|| **entityTypeId** | Entity type identifier ||
|| **fieldCode** | Field code ||
|| **fieldTitle** | Field title ||
|#

{% include [Parameter notes](../../../../_includes/required.md) %}

## Example

```json
[
    {
        "entityTypeId": 1,
        "fieldCode": "TITLE",
        "fieldTitle": "Lead Title"
    },
    {
        "entityTypeId": 1,
        "fieldCode": "ADDRESS",
        "fieldTitle": "Address"
    },
    {
        "entityTypeId": 1,
        "fieldCode": "SOURCE_DESCRIPTION",
        "fieldTitle": "Additional Information about the Source"
    },
    ...
]
```

{% include [Example notes](../../../../_includes/examples.md) %}