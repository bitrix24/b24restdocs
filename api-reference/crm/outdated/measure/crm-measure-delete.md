# Delete Measurement Unit crm.measure.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method deletes a measurement unit.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name** | **Description** ||
|| **id*** | Identifier of the measurement unit ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"**put_id_here**"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.measure.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"**put_id_here**","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.measure.delete
    ```

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.measure.delete",
    		{id: id}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.info(result);
    	}
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    $id = readline("Enter ID");
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.measure.delete',
                [
                    'id' => $id,
                ]
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
        echo 'Error deleting measure: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.measure.delete",
        {id: id},
        function (result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $id = ''; // Set the ID here

    $result = CRest::call(
        'crm.measure.delete',
        ['id' => $id]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./crm-measure-add.md)
- [{#T}](./crm-measure-update.md)
- [{#T}](./crm-measure-get.md)
- [{#T}](./crm-measure-list.md)
- [{#T}](./crm-measure-fields.md)