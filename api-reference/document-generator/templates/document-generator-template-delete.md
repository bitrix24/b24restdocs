# Delete Template documentgenerator.template.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.template.delete` removes a document template.

If there is at least one document created from this template, it is not actually deleted. The record remains to maintain the link between the document and the module. When retrieving the list of templates, they can be filtered by the parameter `isDeleted=Y`.

If there are no documents, the template record is completely removed.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Template identifier. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}