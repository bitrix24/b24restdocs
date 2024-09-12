# Update Existing Document documentgenerator.document.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.document.update` updates an existing document with new values. Please note that updating a document on a remote template is not possible. It works similarly to [documentgenerator.document.add()](./document-generator-document-add.md).

#|
|| **Parameter** | **Description** ||
|| **id** | Document identifier. ||
|| **values** | Array of new field values for the document. ||
|| **stampsEnabled** | 1 (enable), 0 (disable) stamps and signatures. ||
|| **fields** | Field settings. ||
|#