# Get Information About the Numerator by Id crm.documentgenerator.numerator.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.numerator.get` returns information about the numerator by its identifier.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | ID of the numerator. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Successful Response

```json
"numerator": {
    "id": "202", // template id
    "name": "Rest Template", // name
    "template": "{NUMBER}", // template
    "settings": { // generator settings
        "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
            "start": 20,
            "step": 5,
            "periodicBy": '',
            "timezone": '',
            "isDirectNumeration": ''
        }    
    },
}
```