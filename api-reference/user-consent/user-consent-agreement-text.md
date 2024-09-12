# Get the text of the agreement userconsent.agreement.text

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`userconsent`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `userconsent.agreement.text` allows you to retrieve the text of the agreement.

#|
|| **Parameter** | **Description** ||
|| **id** | Agreement code. ||
|| **replace** | Array:
- **button_caption** - button name;
- **fields** - array of form field names. || 
|#