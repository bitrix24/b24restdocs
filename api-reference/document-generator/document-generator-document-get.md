# Get Document by ID documentgenerator.document.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.document.get` returns information about a document by its ID.

#|
|| **Parameter** | **Description** ||
|| **id** | Document ID. ||
|#

## Successful Response

> 200 OK

```json
"document": {
    "id": 1929, // document id
    "title": "Rest Template 1", // title
    "number": "1", // number
    "createTime": "2018-06-05T16:04:40+02:00", // creation date
    "updateTime": "2018-06-05T16:04:40+02:00", // modification date
    "createdBy": "1", // user id of the document creator
    "updatedBy": "1", // user id of the document updater
    "stampsEnabled": true, // whether stamps and signatures are included
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
```