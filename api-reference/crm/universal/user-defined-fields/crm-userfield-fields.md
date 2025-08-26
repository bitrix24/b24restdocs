# Get Description for Custom Fields crm.userfield.fields

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameters and their types are not specified
- response is missing in case of error and success

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.userfield.fields` returns the description of fields for custom fields.


## Example

{% list tabs %}

- CURL (webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.userfield.fields
    ```

- CURL (oauth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/crm.userfield.fields?auth=**put_access_token_here**
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.userfield.fields",
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
                'crm.userfield.fields',
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
        echo 'Error fetching user fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.userfield.fields",
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

    ```
    require_once('crest.php');

    $result = CRest::call(
        'crm.userfield.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}