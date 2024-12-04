# Get information on whether inventory management is enabled catalog.document.mode.status

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter specifications are missing
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.mode.status()
```

Method to retrieve information on whether inventory management is enabled.
Returns the status of inventory management:

- `Y` - inventory management is enabled;
- `N` - inventory management is disabled.
  
## Parameters

No parameters

## Examples

{% list tabs %}

- JS
  
    ```js
    BX24.callMethod(
        'catalog.document.mode.status',
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP
  
    ```php
    $result = CRest::call(
        'catalog.document.mode.status'
    );
    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}