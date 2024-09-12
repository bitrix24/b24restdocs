# Enable and Disable Public Link for Document crm.documentgenerator.document.enablepublicurl

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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

The method `crm.documentgenerator.document.enablepublicurl` enables/disables the public link for the document.

#|
|| **Parameter** | **Description** ||
|| **id** | Document identifier. ||
|| **status** | 1 (enable), 0 (disable) the public link for the document. ||
|#