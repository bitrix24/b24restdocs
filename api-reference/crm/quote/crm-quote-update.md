# Update the estimate crm.quote.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method Development Stopped" %}

The method `crm.quote.update` continues to function, but there is a more relevant alternative [crm.item.update](../universal/crm-item-update.md).

{% endnote %}

The method `crm.quote.update` updates an existing estimate.


#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the estimate. ||
|| **fields**
[`unknown`](../../data-types.md) | [Set of fields](./crm-quote-add.md) - an array of the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values returned by the method [crm.quote.fields](./crm-quote-fields.md). 
{% note info %}

To find out the required format of the fields, execute the method [crm.quote.fields](./crm-quote-fields.md) and check the format of the returned values for these fields. 

{% endnote %}
||
|| **params**
[`unknown`](../../data-types.md) | Set of parameters. `REGISTER_HISTORY_EVENT` - create a record in history, default value: "Y". Additionally, a notification will be sent to the person responsible for the estimate. ||
|#

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.quote.update",
    		{
    			id: id,
    			fields: { "STATUS_ID": "SENT" }    
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.info(result);
    }
    catch(error)
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
                'crm.quote.update',
                [
                    'id' => $id,
                    'fields' => ['STATUS_ID' => 'SENT'],
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
        echo 'Error updating estimate: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.quote.update",
        {
            id: id,
            fields: { "STATUS_ID": "SENT" }    
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}