# Document Templates

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- An introduction corresponding to the title is needed.

{% endnote %}

{% endif %}

{% note info "Permissions" %}

**Scope**: [`crm.documentgenerator`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

#|
|| **Method** | **Description** ||
|| [crm.documentgenerator.template.getfields](./crm-document-generator-template-get-fields.md) | Document template fields. ||
|| [crm.documentgenerator.template.add](./crm-document-generator-template-add.md) | Adding a new template. ||
|| [crm.documentgenerator.template.update](./crm-document-generator-template-update.md) | Modifying an existing document template. ||
|| [crm.documentgenerator.template.get](./crm-document-generator-template-get.md) | Retrieving information about a document template by Id. ||
|| [crm.documentgenerator.template.list](./crm-document-generator-template-list.md) | Getting a list of document templates. ||
|| [crm.documentgenerator.template.delete](./crm-document-generator-template-delete.md) | Deleting a document template. ||
|#