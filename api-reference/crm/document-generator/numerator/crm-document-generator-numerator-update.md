# Update Existing Numbering crm.documentgenerator.numerator.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- it's better to add a custom response in case of success (not a link)
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.numerator.update` updates an existing numbering with new values.

Only those numberings that were created through [crm.documentgenerator.numerator.add()](./crm-document-generator-numerator-add.md) can be updated.

#|
|| **Parameter** | **Description** ||
|| **id** | ID of the numbering. ||
|| **fields** | An array similar to [crm.documentgenerator.numerator.add()](./crm-document-generator-numerator-add.md), but all fields are optional. ||
|#

## Response in case of success

Returns a result identical to [crm.documentgenerator.numerator.get()](./crm-document-generator-numerator-get.md).