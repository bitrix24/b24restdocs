# Get Application-Bound Data app.option.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Success response is missing
- Error response is missing

{% endnote %}

{% endif %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `app.option.get` retrieves data bound to the application. If no input is provided, it will return all properties recorded via [app.option.set](./app-option-set.md).

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **option**
[`string`](../../data-types.md) | A string, one of the keys from the **app.option.set** property. | ||
|#

## Examples

```js
CRest::call('app.option.get',[//will return the property with the key 'data'
    'option' => 'data'
])
```

```js
CRest::call('app.option.get',[]) //will return all properties
```

{% include [Example Notes](../../../_includes/examples.md) %}