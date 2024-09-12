# Get the list of templates documentgenerator.template.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.template.list` returns a list of templates based on the filter.

#|
|| **Parameter** | **Description** ||
|| **select** | Array of fields to output. By default, it outputs all template fields except *users* and *providers*. To include them, you need to add them additionally. For example, `['*', 'providers', 'users']`. ||
|| **order** | Array to specify the output order `{"id": "desc"}`. ||
|| **filter** | Array for filtering. ||
|| **start** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Example

```json
templates: {
    202: {
        "id": "202",
        "active": "Y",
        "name": "Rest Template",
        "code": "",
        "region": "com",
        "sort": "100",
        "createTime": "2018-06-05T13:07:12+02:00",
        "updateTime": "2018-09-06T14:26:24+02:00",
        "moduleId": "rest",
        "numeratorId": "20",
        "withStamps": "N",
        "isDeleted": "N",
        "download": "",
        "downloadMachine": "",
        "providers": [
            "bitrix\\documentgenerator\\dataprovider\\rest": "bitrix\\documentgenerator\\dataprovider\\rest"
        ],
        "users": [
            "0": "UA"
        ]
    }
}
```

## Filter Examples

```json
filter: {
"numeratorId": "20",
"region": "com",
"active": "Y"
}
```

By default, the filter has the following values:

```json
filter: {
"moduleId": "rest", // cannot be changed, the method will always return templates only for rest
"isDeleted": "N" // if you need a list of templates regardless of isDeleted, you must pass "@isDeleted": ["Y", "N"]
}
```
## Successful Response

The method will return a list of templates with their fields.