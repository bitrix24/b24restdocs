# Add a New Product Section crm.productsection.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.productsection.add` creates a new product section.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`array`](../../data-types.md) | A set of fields — an array of the form `array("field"=>"value"[, ...])`, containing the values of the product section fields. 

{% note info %}

To find out the required format of the fields, execute the method [crm.productsection.fields](./crm-product-section-fields.md) and check the format of the returned values for these fields. 

{% endnote %}
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    catalogId=$(prompt "Enter catalog ID")
    sectionId=$(prompt "Enter parent section ID (0 if at root)")
    sectionName=$(prompt "Enter section name")

    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "fields": {
            "CATALOG_ID": '"$catalogId"',
            "NAME": "'"$sectionName"'",
            "SECTION_ID": '"$sectionId"'
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.productsection.add
    ```

- cURL (OAuth)

    ```curl
    catalogId=$(prompt "Enter catalog ID")
    sectionId=$(prompt "Enter parent section ID (0 if at root)")
    sectionName=$(prompt "Enter section name")

    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "fields": {
            "CATALOG_ID": '"$catalogId"',
            "NAME": "'"$sectionName"'",
            "SECTION_ID": '"$sectionId"'
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/crm.productsection.add
    ```

- JS

    ```js
    try
    {
    	const catalogId = prompt("Enter catalog ID");
    	const sectionId = prompt("Enter parent section ID (0 if at root)");
    	const sectionName = prompt("Enter section name");
    
    	const response = await $b24.callMethod(
    		"crm.productsection.add",
    		{
    			fields:
    			{
    				CATALOG_ID: catalogId,
    				NAME: sectionName,
    				SECTION_ID: sectionId
    			}
    		}
    	);
    
    	const result = response.getData().result;
    	console.info("A new section has been created with ID " + result);
    }
    catch(error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $catalogId = readline("Enter catalog ID");
        $sectionId = readline("Enter parent section ID (0 if at root)");
        $sectionName = readline("Enter section name");
    
        $response = $b24Service
            ->core
            ->call(
                "crm.productsection.add",
                [
                    'fields' => [
                        'CATALOG_ID' => $catalogId,
                        'NAME' => $sectionName,
                        'SECTION_ID' => $sectionId
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo "A new section has been created with ID " . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating product section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var catalogId = prompt("Enter catalog ID");
    var sectionId = prompt("Enter parent section ID (0 if at root)");
    var sectionName = prompt("Enter section name");
    BX24.callMethod(
        "crm.productsection.add",
        {
            fields:
            {
                CATALOG_ID: catalogId,
                NAME: sectionName,
                SECTION_ID: sectionId
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("A new section has been created with ID " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $catalogId = readline("Enter catalog ID: ");
    $sectionId = readline("Enter parent section ID (0 if at root): ");
    $sectionName = readline("Enter section name: ");

    $result = CRest::call(
        'crm.productsection.add',
        [
            'fields' => [
                'CATALOG_ID' => $catalogId,
                'NAME' => $sectionName,
                'SECTION_ID' => $sectionId
            ]
        ]
    );

    if (isset($result['error'])) {
        echo "Error: " . $result['error_description'] . "\n";
    } else {
        echo "A new section has been created with ID " . $result['result'] . "\n";
    }
    ```

{% endlist %}