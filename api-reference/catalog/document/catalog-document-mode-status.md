# Get Information on Whether Inventory Management is Enabled catalog.document.mode.status

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
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
The status of inventory management is returned:

- `Y` - inventory management is enabled;
- `N` - inventory management is disabled.
  
## Parameters

No parameters

## Examples

{% list tabs %}

- js
  
    ```
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

- php
  
    ```
    $result = CRest::call(
        'catalog.document.mode.status'
    );
    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}