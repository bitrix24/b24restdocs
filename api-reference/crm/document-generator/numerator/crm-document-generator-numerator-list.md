# Get the list of numerators crm.documentgenerator.numerator.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter type is not specified
- parameter requirement is not specified
- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.numerator.list` returns a list of numerators.

#|
|| **Parameter** | **Description** ||
|| **start** | `offset` for pagination. ||
|#

## Response on success

```json
"numerators": [
    {
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
        }
    }
]
```