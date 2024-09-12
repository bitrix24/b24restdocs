# Conduct Warehouse Accounting Document (confirm) catalog.document.confirm

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

{% note info "Attention!" %}

The method has been deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.conduct](./catalog-document-conduct.md).

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.confirm(id)
```

Method for conducting a warehouse accounting document. If the operation is successful, `true` is returned in the response body.


## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md)| Document identifier. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- js
  
    ```
    BX24.callMethod(
    'catalog.document.confirm',
    {
        'id': 42,
    },
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
        'catalog.document.confirm',
        [
            'id' => 42,
        ]
    );
    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}