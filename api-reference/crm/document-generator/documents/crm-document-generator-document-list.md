# Get the list of documents crm.documentgenerator.document.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.document.list` returns a list of documents based on the filter.

#|
|| **Parameter** | **Description** ||
|| **select** | Array of fields to output. ||
|| **order** | Array to specify the output order `{"id": "desc"}`. ||
|| **filter** | Array for filtering. ||
|| **start** | Offset for pagination. ||
|#

## Example of a filter

```json
"filter": {
    "entityTypeId": 2,
    "entityId": 5
}
```

As a result, a list of documents will be generated for the deal with ID=5.

## Response in case of success

```json
"documents": [
    {
        "id": "1523",
        "title": "Act (USA) 1",
        "number": "1",
        "templateId": "115",
        "entityTypeId": "2",
        "fileId": "4315",
        "imageId": "4316",
        "pdfId": "4317",
        "createTime": "2018-06-05T16:04:40+02:00",
        "updateTime": "2018-06-05T16:04:40+02:00",
        "values": {},
        "entityId": "5",
        "downloadUrl": "",
        "downloadUrlMachine": "",
        "imageUrl": "",
        "imageUrlMachine": "",
        "pdfUrl": "",
        "pdfUrlMachine": ""
    }
]
```