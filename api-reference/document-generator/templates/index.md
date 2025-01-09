# Templates in the Document Generator

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Write an article, as it is completely unclear how roles work in the document generator.

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [documentgenerator.template.add](./document-generator-template-add.md) | Uploads a new document template ||
|| [documentgenerator.template.update](./document-generator-template-update.md) | Updates an existing document template ||
|| [documentgenerator.template.get](./document-generator-template-get.md) | Returns a document template by its identifier ||
|| [documentgenerator.template.list](./document-generator-template-list.md) | Returns a list of document templates by filter ||
|| [documentgenerator.template.delete](./document-generator-template-delete.md) | Deletes a document template ||
|| [documentgenerator.template.getfields](./document-generator-template-get-fields.md) | Returns a list of document template fields || 
|#