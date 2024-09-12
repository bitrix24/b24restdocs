# Upload the generated document and attach it to the specified entity crm.documentgenerator.document.upload

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- the mandatory nature of all parameters is not indicated
- examples are missing
- it’s better to add a custom response in case of success (not a link)
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.document.upload` uploads a generated document and attaches it to the specified entity. During the upload process, a hidden template is created, linked to the current REST application (one template per application). An empty file is attached to this template (since a template cannot exist without a file).

#|
|| **Parameter** | **Description** ||
|| **fileContent** | Content of the docx file in *base64*. ||
|| **region** | Country. ||
|| **entityTypeId** | ID of the CRM entity type. ||
|| **entityId** | ID of the CRM entity. ||
|| **title** | Document title. ||
|| **number** | Document number. ||
|| **pdfContent** | Content of the pdf file in *base64*, optional. ||
|| **imageContent** | Content of the image in *base64*, optional. ||
|#

## Response in case of success

The method returns values similar to [crm.documentgenerator.document.add](./crm-document-generator-document-add.md).