# Get Template by ID documentgenerator.template.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `documentgenerator.template.get` returns information about a template by its ID.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Template ID. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Successful Response

> 200 OK

```json
"template": {
    "id": "202", // template id
    "name": "Rest Template", // name
    "region": "us", // country
    "code": "", // code
    "download": '', // download link for the user
    "downloadMachine": '', // download link for the application
    "active": "Y", // active status
    "moduleId": "rest", // module id
    "numeratorId": "20", // numerator id
    "withStamps": "N", // apply stamps by default
    "isDeleted": "N", // deleted or not
    "sort": "100", // sorting
    "createTime": "2018-06-05T13:07:12+02:00",
    "updateTime": "2018-09-06T14:26:24+02:00",
    "providers": [ // providers
        "bitrix\\documentgenerator\\dataprovider\\rest": "bitrix\\documentgenerator\\dataprovider\\rest"
    ],
    "users": [ // associated users
        "0": "UA"
    ]
}
```