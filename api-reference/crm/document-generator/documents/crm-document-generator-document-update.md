# Update Document crm.documentgenerator.document.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.document.update` updates an existing document with new values. It is important to remember that updating a document based on a remote template is not possible. The method works similarly to [crm.documentgenerator.document.add()](./crm-document-generator-document-add.md).

#|
|| **Parameter** | **Description** ||
|| **id** | Document identifier. ||
|| **values** | Array of new field values for the document. ||
|| **stampsEnabled** | 1 (enable), 0 (disable) stamps and signatures. ||
|#