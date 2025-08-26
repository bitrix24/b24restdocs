# Get the field settings description for the custom field type crm.userfield.settings.fields

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- no response in case of error and success

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.settings.fields` returns the field settings description for the custom field type.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **type** | The type of the custom field. Value from the list returned by the method [crm.userfield.types](crm-userfield-types.md).  ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Example

{% list tabs %}

- CURL (webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"string"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.userfield.settings.fields
    ```

- CURL (oauth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"string"}' \
    https://**put_your_bitrix24_address**/rest/crm.userfield.settings.fields?auth=**put_access_token_here**
    ```

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.userfield.settings.fields",
    		{
    			type: "string"
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch(error)
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
                'crm.userfield.settings.fields',
                [
                    'type' => 'string'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.userfield.settings.fields",
        {
            type: "string"
        },
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

    $type = 'string'; // Replace with the required type

    $result = CRest::call(
        'crm.userfield.settings.fields',
        ['type' => $type]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

{% include [Note on examples](../../../../_includes/examples.md) %}

## Continue exploring

- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-precision-to-user-field.md)