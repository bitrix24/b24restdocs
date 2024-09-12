# Change Country documentgenerator.region.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The method `documentgenerator.region.update` updates an existing country.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Region ID. ||
|| **fields** | Array of fields. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## fields Parameters

#|
|| **Parameter** | **Description** ||
|| **title** | Country name. ||
|| **languageId** | Two-letter language identifier. ||
|| **formatDate** | Date format. ||
|| **formatDatetime** | Date-time format. ||
|| **formatName** | Name format. ||
|#

All parameters are optional.

## Success Response

Returns the same data as when calling [documentgenerator.region.get()](./document-generator-region-get.md).