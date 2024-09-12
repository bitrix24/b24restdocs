# Get Document Information by Id crm.documentgenerator.document.get

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

The method `crm.documentgenerator.document.get` returns information about a document by its identifier.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Document ID. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

### Why there is no link to **pdf** in the result of **crm.documentgenerator.document.add**

- Conversion to **pdf** is an asynchronous operation. At the time the document generation is completed, the **pdf** file is not yet available. The minimum conversion time is 8 seconds.
- If a **pdf** is urgently needed for the document, the only option right now is to make a repeated request to **crm.documentgenerator.document.get** after 20-30 seconds to retrieve the link to the **pdf**. If it hasn't appeared, try again.

## Response in case of success

```json
"document": {
    "id": 1,
    "title": "Invoice (USA) 1",
    "number": "1",
    "createTime": "2018-06-05T16:04:40+02:00",
    "updateTime": "2018-06-05T16:04:40+02:00",
    "createdBy": "1",
    "updatedBy": "1",
    "stampsEnabled": true,
    "downloadUrl": "",
    "downloadUrlMachine": "",
    "imageUrl": "",
    "imageUrlMachine": "",
    "pdfUrl": "",
    "pdfUrlMachine": "",
    "publicUrl": "",
    "isTransformationError": false,
    "templateId": "1",
    "entityTypeId": 1,
    "entityId": "1",
    "values": {}
}
```