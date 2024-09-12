# Delete documentgenerator.numerator.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.numerator.delete` removes a numerator.

{% note warning %}

You can only delete numerators that were created using [documentgenerator.numerator.add()](./index.md).

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | ID of the numerator. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %} 

## Response on success

The response is empty.