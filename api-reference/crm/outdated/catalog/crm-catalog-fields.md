# Get Product Catalog Fields crm.catalog.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the description of the product catalog fields.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.catalog.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/crm.catalog.fields?auth=**put_access_token_here**
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.catalog.fields",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.catalog.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **ID** 
[`integer`](../../../data-types.md) | Identifier. Read-only ||
|| **NAME** 
[`string`](../../../data-types.md) | Name ||
|| **ORIGINATOR_ID** 
[`string`](../../../data-types.md) | Identifier of the data source. Used only for linking to an external source ||
|| **ORIGIN_ID** 
[`string`](../../../data-types.md) | Identifier of the element in the data source. Used only for linking to an external source ||
|| **XML_ID** 
[`string`](../../../data-types.md) | Symbolic code. Read-only ||
|#