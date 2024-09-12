# Delete Document Template crm.documentgenerator.template.delete

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

The method `crm.documentgenerator.template.delete` deletes a template.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | template identifier. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Response on Success

The response is empty.