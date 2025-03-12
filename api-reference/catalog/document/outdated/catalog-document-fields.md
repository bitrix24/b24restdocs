# Get a list of document fields catalog.document.fields

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

{% note warning "Method development has been halted" %}

The method `catalog.document.fields` is still operational, but there is a more relevant alternative [catalog.document.getFields](../catalog-document-get-fields.md).

{% endnote %}

The method `catalog.document.fields` returns a list of document fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.fields
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.document.fields',
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
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning 

- [{#T}](./catalog-document-confirm.md)
- [{#T}](./catalog-document-unconfirm.md)
- [{#T}](./catalog-document-element-fields.md)