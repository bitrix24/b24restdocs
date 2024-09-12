# Enable/Disable Public Link for Document documentgenerator.document.enablepublicurl

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `documentgenerator.document.enablepublicurl` enables/disables the public link for the document.

#|
|| **Parameter** | **Description** ||
|| **id** | Document identifier. ||
|| **status** | 1 (enable), 0 (disable) the public link for the document. ||
|#

## Success Response

> 200 OK

```json
"publicUrl": "" // public link
```