# Add a new numerator crm.documentgenerator.numerator.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- it's better to add a custom response in case of success (not as a link)
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.numerator.add` adds a new numerator.

#|
|| **Parameter** | **Description** ||
|| **name** | Name. ||
|| **template** | Template. ||
|| **settings** | Generator settings. ||
|#

## Response in case of success

Returns a result identical to [crm.documentgenerator.numerator.get()](./crm-document-generator-numerator-get.md).

## Continue exploring

- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)