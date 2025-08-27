# Get available fields of crm.measure.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a description of the fields for measurement units.

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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.measure.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.measure.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.measure.fields",
    		{}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.measure.fields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching CRM measure fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.measure.fields",
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.measure.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Returned Fields

#|
|| **Field**
`type` | **Description** | **Note** ||
|| **CODE** 
[`integer`](../../data-types.md) | Unit code | Required ||
|| **ID** 
[`integer`](../../data-types.md) | Identifier | Read-only ||
|| **IS_DEFAULT** 
[`char`](../../data-types.md) | Default | ||
|| **MEASURE_TITLE** 
[`string`](../../data-types.md) | Measurement unit name | Required ||
|| **SYMBOL_INTL** 
[`string`](../../data-types.md) | International symbol | ||
|| **SYMBOL_LETTER_INTL** 
[`string`](../../data-types.md) | International letter code | ||
|| **SYMBOL_RUS** 
[`string`](../../data-types.md) | Symbol | ||
|#

## Continue Learning

- [{#T}](./crm-measure-add.md)
- [{#T}](./crm-measure-update.md)
- [{#T}](./crm-measure-get.md)
- [{#T}](./crm-measure-list.md)
- [{#T}](./crm-measure-delete.md)