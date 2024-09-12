# Get Document Template Fields crm.documentgenerator.template.getfields

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- return values are absent
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.template.getfields` returns a list of template fields along with their descriptions.

#|
|| **Parameter** | **Description** ||
|| **id** | Template ID. ||
|| **entityTypeId** | CRM entity type ID. ||
|| **entityId** | ID of the entity being used. ||
|| **values** | Array of additional values. ||
|#

## Response on Success

```json
"templateFields": {
    "DocumentNumber": {
        "title": "Number",
        "value": "22",
        "group": [
            "Document"
        ],
        "default": "22"
    },
    "MyCompanyUfLogo": {
        "title": "Logo",
        "value": "",
        "type": "IMAGE",
        "group": [
            "Document",
            "My Company"
        ],
        "default": ""
    },
    "MY_COMPANY": {
        "title": "My Company",
        "value": [
            {
                "value": "6",
                "title": "1C-Bitrix",
                "selected": "1"
            },
            {
                "value": "11",
                "title": "Kopytov LLC",
                "selected": " "
            }
        ],
        "group": [
            "Document",
            "My Company"
        ]
    }
}
```