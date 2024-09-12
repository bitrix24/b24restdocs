# Delete crm.documentgenerator.numerator.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `crm.documentgenerator.numerator.delete` removes a numerator.

You can only delete numerators that were created using [crm.documentgenerator.numerator.add()](./crm-document-generator-numerator-add.md). 

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | ID of the numerator. ||
|#

{% include [Parameter notes](../../../../_includes/required.md) %}

## Response on success

The response is empty.