# Get Access Permission Names access.name

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- no response in case of success
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `access.name` method retrieves the names of access permissions.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ACCESS^*^**
[`unknown`](../../data-types.md) | Required. A list of permission identifiers for which names need to be retrieved. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```http
https://my.bitrix24.com/rest/access.name.xml?ACCESS[]=AU&auth=d161f25928c3184678924ec127edd29a
```

{% include [Note on examples](../../../_includes/examples.md) %}