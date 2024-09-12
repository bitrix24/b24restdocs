# Get the list of roles landing.role.getList

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% note info "" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Execution rights**: `administrator`

{% endnote %}

The method `landing.role.getList` allows you to retrieve a list of roles. It will return an array of identifiers and names of all roles.

## Parameters

The method has no parameters.

## Examples

```js
BX24.callMethod(
    'landing.role.getList',
    {
    },
    function(result)
    {
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            console.info(result.data());
        }
    }
);
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}