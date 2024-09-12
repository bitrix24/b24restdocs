# Add Country documentgenerator.region.add

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

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.region.add` adds a new region. It returns the same data as calling [documentgenerator.region.get()](./document-generator-region-get.md) on the new region.

#|
|| **Parameter** | **Description** ||
|| **fields** | Array of country fields. ||
|#

## Parameters fields

#|
|| **Parameter** | **Description** ||
|| **title**^*^ | Name of the country. ||
|| **languageId** | Two-letter language identifier. ||
|| **formatDate**^*^ | Date format. ||
|| **formatDatetime**^*^ | Date-time format. ||
|| **formatName**^*^ | Name format. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Response on success

> 200 OK

```json
"region": {
    "id": "1",
    "title": "Turkey",
    "languageId": "",
    "formatDate": "YYYY-MM-DD",
    "formatDatetime": "YYYY-MM-DD HH:MI:SS",
    "formatName": "#LAST_NAME# #NAME# #SECOND_NAME#",
    "phrases": {
        "TAX_INCLUDED": "VAT included in the price",
        "TAX_NOT_INCLUDED": "VAT not included in the price",
        "TAX_INCLUDED_NOT_VAT": "Tax (not VAT) included in the price",
        "TAX_NOT_INCLUDED_NOT_VAT": "Tax (not VAT) not included in the price",
        "UF_TYPE_BOOLEAN_YES": "Yes",
        "UF_TYPE_BOOLEAN_NO": "No"
    }
}
```