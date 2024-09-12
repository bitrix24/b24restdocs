# Get the list of documents documentgenerator.document.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- complete example is missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.document.list` returns a list of documents based on the filter.

#|
|| **Parameter** | **Description** ||
|| **select** | Array of fields to output. ||
|| **order** | Array to specify the output order `{"id": "desc"}`. ||
|| **filter** | Array for filtering. ||
|| **start** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Example of a filter

```json
"filter": {
    "value": 5
}
```

As a result, a list of documents will be generated for the external identifier = 5.

## Response in case of success

> 200 OK

```json
"documents": [
    {
        "id": 1929, // document id
        "title": "Rest Template 1", // title
        "number": "1", // number
        "createTime": "2018-06-05T16:04:40+02:00", // creation date
        "updateTime": "2018-06-05T16:04:40+02:00", // modification date
        "stampsEnabled": true, // whether stamps and signatures are inserted
        "downloadUrl": "", // link for the user to download the docx file
        "downloadUrlMachine": "", // link for the application to download the docx file
        "imageUrl": "", // link for the user to download the image
        "imageUrlMachine": "", // link for the application to download the image
        "pdfUrl": "", // link for the user to download the pdf
        "pdfUrlMachine": "", // link for the application to download the pdf
        "publicUrl": "", // public link (if available)
        "isTransformationError": false, // whether there was a conversion error
        "templateId": "202", // template id
        "provider": "Bitrix\\DocumentGenerator\\DataProvider\\Rest", // provider code
        "value": "1", // external identifier
        "values": { // field values
            "SomeDate": "2018-02-21T16:33:00+03:00"
        }
    }
]
```