# Cancel Document Processing (unconfirm) catalog.document.unconfirm

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

{% note info "Attention!" %}

This method has been deprecated since version **22.400.0**. It is recommended to use the method [catalog.document.cancel](./catalog-document-cancel.md).

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```http
catalog.document.unconfirm(id)
```

This method is used to cancel the processing of a document. If the operation is successful, it returns `true` for the added inventory.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`integer`](../../data-types.md)| Document number. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS
  
    ```js
    BX24.callMethod(
        'catalog.document.unconfirm',
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

- PHP
  
    ```php
    $result = CRest::call(
        'catalog.document.unconfirm',
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