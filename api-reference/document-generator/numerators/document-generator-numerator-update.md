# Update documentgenerator.numerator.update

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

The method `documentgenerator.numerator.update` updates an existing numerator with new values.

{% note warning %}

You can only update numerators that were created using [documentgenerator.numerator.add()](./index.md).

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | ID of the numerator. ||
|| **fields** | An array similar to [documentgenerator.numerator.add()](./index.md), but all fields are optional. ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Response on success

Returns a result identical to [documentgenerator.numerator.get()](./document-generator-numerator-get.md).