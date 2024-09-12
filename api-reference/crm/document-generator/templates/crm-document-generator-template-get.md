# Get Document Template Information by Id crm.documentgenerator.template.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.template.get` returns information about a template by its identifier.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Template ID. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Successful Response

```json
"template": {
    "id": 1, // template id
    "name": "Invoice (USA)", // name
    "region": "com", // country
    "code": "INVOICE_US", // code
    "download": '', // download link for the user
    "downloadMachine": '', // download link for the application
    "active": "Y", // active status
    "moduleId": "crm", // module id
    "numeratorId": 1, // numerator id
    "withStamps": "Y", // apply stamps by default
    "isDeleted": "N", // deleted or not
    "entityTypeId": [ // linked entities
        "0": "4",
        "1": "3",
        "2": "2_category_0",
        "3": "2_category_1",
        "4": "5",
        "5": "1",
        "6": "14",
        "7": "7"
    ],
    "users": [ // linked users
        "0": "UA"
    ],
    "sort": 500 // sort index
}
```