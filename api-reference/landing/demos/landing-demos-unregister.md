# Delete Registered Partner Template landing.demos.unregister

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.demos.unregister` deletes the registered partner template. It returns *true* or an error. If the template has already been deleted or not found, it will return *false*.

{% note warning %}

Both the site template with this code and all page templates with this code will be deleted. Created sites and pages based on these templates will remain untouched.

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** ||
|| **code**
[`unknown`](../../data-types.md) | Symbolic code of the template. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.demos.unregister',
    {
        code: 'myfirstsite'
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

{% include [Example Note](../../../_includes/examples.md) %}