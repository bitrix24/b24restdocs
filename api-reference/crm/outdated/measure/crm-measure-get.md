# Get the values of the fields of the unit of measurement crm.measure.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the values of all fields of the unit of measurement by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name** | **Description** ||
|| **id*** | Identifier of the unit of measurement ||
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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.measure.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"**put_id_here**","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.measure.get
    ```

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.measure.get",
    		{id: id}
    	);
    
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    $id = $_POST['id'];
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.measure.get',
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
        echo 'Error getting measure: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.measure.get",
        {id: id},
        function (result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $id = ''; // Set the ID here

    $result = CRest::call(
        'crm.measure.get',
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
- [{#T}](./crm-measure-list.md)
- [{#T}](./crm-measure-delete.md)
- [{#T}](./crm-measure-fields.md)