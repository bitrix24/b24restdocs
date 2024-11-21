# Get a list of products by filter crm.product.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of products based on the filter.

## Method Parameters

See the description of [list methods](../../../common/index.md).

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"NAME":"ASC"},"filter":{"CATALOG_ID":"your_catalog_id"},"select":["ID","NAME","CURRENCY_ID","PRICE"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.product.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"NAME":"ASC"},"filter":{"CATALOG_ID":"your_catalog_id"},"select":["ID","NAME","CURRENCY_ID","PRICE"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.product.list
    ```

- JS

    ```js
    var catalogId = prompt("Enter the catalog ID");
    BX24.callMethod(
        "crm.product.list",
        {
            order: { "NAME": "ASC" },
            filter: { "CATALOG_ID": catalogId },
            select: [ "ID", "NAME", "CURRENCY_ID", "PRICE" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

    To get product properties, you need to specify `PROPERTY_*` in `select`.

    ```js
    $arFields['select'] = array('*', 'PROPERTY_*');
    ```

- PHP

    ```php
    require_once('crest.php');

    $catalogId = 'your_catalog_id'; // Replace 'your_catalog_id' with the actual catalog ID

    $result = CRest::call(
        'crm.product.list',
        [
            'order' => ['NAME' => 'ASC'],
            'filter' => ['CATALOG_ID' => $catalogId],
            'select' => ['ID', 'NAME', 'CURRENCY_ID', 'PRICE']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- B24-PHP-SDK

    ```php        
    try {
        $order = []; // Define your order array
        $filter = []; // Define your filter array
        $select = ['ID', 'CATALOG_ID', 'PRICE', 'CURRENCY_ID', 'NAME', 'CODE', 'DESCRIPTION', 'DESCRIPTION_TYPE', 'ACTIVE', 'SECTION_ID', 'SORT', 'VAT_ID', 'VAT_INCLUDED', 'MEASURE', 'XML_ID', 'PREVIEW_PICTURE', 'DETAIL_PICTURE', 'DATE_CREATE', 'TIMESTAMP_X', 'MODIFIED_BY', 'CREATED_BY'];
        $startItem = 0; // Define your start item
        $result = $serviceBuilder
            ->getCRMScope()
            ->product()
            ->list($order, $filter, $select, $startItem);
        foreach ($result->getProducts() as $product) {
            print("ID: {$product->ID}\n");
            print("Catalog ID: {$product->CATALOG_ID}\n");
            print("Price: {$product->PRICE}\n");
            print("Currency ID: {$product->CURRENCY_ID}\n");
            print("Name: {$product->NAME}\n");
            print("Code: {$product->CODE}\n");
            print("Description: {$product->DESCRIPTION}\n");
            print("Description Type: {$product->DESCRIPTION_TYPE}\n");
            print("Active: {$product->ACTIVE}\n");
            print("Section ID: {$product->SECTION_ID}\n");
            print("Sort: {$product->SORT}\n");
            print("VAT ID: {$product->VAT_ID}\n");
            print("VAT Included: {$product->VAT_INCLUDED}\n");
            print("Measure: {$product->MEASURE}\n");
            print("XML ID: {$product->XML_ID}\n");
            print("Preview Picture: {$product->PREVIEW_PICTURE}\n");
            print("Detail Picture: {$product->DETAIL_PICTURE}\n");
            print("Date Create: " . (new DateTime($product->DATE_CREATE))->format(DateTime::ATOM) . "\n");
            print("Timestamp X: " . (new DateTime($product->TIMESTAMP_X))->format(DateTime::ATOM) . "\n");
            print("Modified By: {$product->MODIFIED_BY}\n");
            print("Created By: {$product->CREATED_BY}\n");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

{% endlist %}