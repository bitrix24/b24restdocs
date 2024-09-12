# Delete Document crm.documentgenerator.document.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter type is not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.document.delete` removes a document.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Document identifier. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Response on Success

The response is empty.