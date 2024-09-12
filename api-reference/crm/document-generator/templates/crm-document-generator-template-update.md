# Update Existing Document Template crm.documentgenerator.template.update

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

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.template.update` updates an existing template. It returns the same data as when calling [crm.documentgenerator.template.get()](./crm-document-generator-template-get.md).

#|
|| **Parameter** | **Description** ||
|| **id** | Template identifier. ||
|| **fields** | Array of fields. Similar to the method [crm.documentgenerator.template.add](./crm-document-generator-template-add.md), but here all fields are optional. ||
|#