# Update Template documentgenerator.template.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.template.update` updates an existing template.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Template identifier. ||
|| **fields** | Array of fields. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Fields Parameters

#|
|| **Parameter** | **Description** ||
|| **name** | Template name. ||
|| **file** | File content, encoded in `base64` (required). Alternatively, the file content can be sent in `multipart/form-data`. In this case, it should not be encoded in `base64`. ||
|| **code** | Symbolic code of the template. ||
|| **numeratorId** | Identifier of the numerator. ||
|| **region** | Country. ||
|| **users** | Visibility array. Defaults to empty. ||
|| **active** | Y/N active flag. Defaults to Y. ||
|| **withStamps** | Y/N apply stamps and signatures. Defaults to N. ||
|| **sort** | Sorting index. ||
|#

All parameters are optional.

## Success Response

Returns the same data as when calling [documentgenerator.template.get()](./document-generator-template-get.md).