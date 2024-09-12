# Add Badge crm.activity.badge.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Success response is absent
- Error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.badge.add` adds a badge. Access to execute this method is only available to the CRM administrator.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **code**
[`string`](../../../../data-types.md) | Badge code ||
|| **title**
[`string`\|`array`](../../../../data-types.md)
| Badge title. It can be either a string or an array of strings for different languages. ||
|| **value**
[`string`\|`array`](../../../../data-types.md)
| Badge value. It can be either a string or an array of strings for different languages. ||
|| **type**
[`string`](../../../../data-types.md)
| Badge type ||
|#