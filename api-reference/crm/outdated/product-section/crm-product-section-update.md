# Update Product Section `crm.productsection.update`

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.productsection.update` updates an existing product section.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the product section ||
|| **fields**
[`array`](../../data-types.md) | [Set of fields](./crm-product-section-add.md) — an array in the format `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values returned by the method [crm.productsection.fields](./crm-product-section-fields.md). 

{% note info %}

To find out the required format of the fields, execute the method [crm.productsection.fields](./crm-product-section-fields.md) and check the format of the returned values for those fields. 

{% endnote %}
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    id=$(prompt "Enter ID")
    sectionName=$(prompt "Enter section name")

    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": '"$id"',
        "fields": {
            "NAME": "'"$sectionName"'"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.productsection.update
    ```

- cURL (OAuth)

    ```curl
    id=$(prompt "Enter ID")
    sectionName=$(prompt "Enter section name")

    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": '"$id"',
        "fields": {
            "NAME": "'"$sectionName"'"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/crm.productsection.update
    ```

- JS

    ```js
    var id = prompt("Enter ID");
    var sectionName = prompt("Enter section name");
    BX24.callMethod(
        "crm.productsection.update",
        {
            id: id,
            fields:
            {
                "NAME": sectionName
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $id = readline("Enter ID: ");
    $sectionName = readline("Enter section name: ");

    $result = CRest::call(
        'crm.productsection.update',
        [
            'id' => $id,
            'fields' => [
                'NAME' => $sectionName
            ]
        ]
    );

    if (isset($result['error'])) {
        echo "Error: " . $result['error_description'] . "\n";
    } else {
        echo "Updated section with ID " . $id . "\n";
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

{% endlist %}