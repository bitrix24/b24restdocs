# Get a list of document templates crm.documentgenerator.template.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.template.list` returns a list of templates based on the filter.

#|
|| **Parameter** | **Description** ||
|| **select** | Array of fields to output. By default, it outputs all template fields except for `users` and `entityTypeId`. To include them, you need to add them additionally. For example, `['*', 'entityTypeId', 'users']`. ||
|| **order** | Array to specify the output order `{"id": "desc"}`. ||
|| **filter** | Array for filtering. ||
|| **start** | `offset` for pagination. ||
|#

## Filter Examples

```json
filter: {
    "code": "%_US",
    "numeratorId": "2",
    "region": "by",
    "active": "Y"
}
```

You can pass the entity ID in the filter using the key `entityTypeId`. You should pass the deal code considering the filtering by directions. However, if you need to return templates linked to any deal direction, you can pass it in the filter.

```json
filter: {
    "entityTypeId": "2%"
}
```

The method will return a list of templates with their fields.

## Response on Success

```json
templates: {
    115: {
        "id": "115",
        "active": "Y",
        "name": "Act (USA)",
        "code": "ACT_US",
        "region": "com",
        "sort": "100",
        "createTime": "2018-06-05T13:07:12+02:00",
        "updateTime": "2018-09-06T14:26:24+02:00",
        "moduleId": "crm",
        "numeratorId": "29",
        "withStamps": "N",
        "isDeleted": "N",
        "entityTypeId": [
            "0": "4",
            "1": "3",
            "2": "2_category_0",
            "3": "2_category_1",
            "4": "5",
            "5": "1",
            "6": "14",
            "7": "7"
        ],
        "users": [
            "0": "UA"
        ]
    }
}
```