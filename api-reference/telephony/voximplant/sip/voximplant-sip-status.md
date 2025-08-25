# Get Current SIP Registration Status (Cloud PBX Only) voximplant.sip.status

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.sip.status` returns the current SIP registration status (for Cloud PBXs only). This method is available to the holder of the [access permissions](https://helpdesk.bitrix24.com/open/18216960/) `Manage numbers - change - any`.

#|
|| **Parameter** | **Description** ||
|| **REG_ID** | SIP registration identifier. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"voximplant.sip.status",
    		{
    			"REG_ID": 5505,
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.info(result);
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
                'voximplant.sip.status',
                [
                    'REG_ID' => 5505,
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
        echo 'Error checking SIP status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "voximplant.sip.status",
        {
            "REG_ID": 5505,
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}

## Returned Data

#|
|| **Field** | **Description** ||
|| **REG_ID** | SIP registration identifier. ||
|| **LAST_UPDATED** | Date of the last change to the SIP registration. ||
|| **ERROR_MESSAGE** | Text description of the error code. ||
|| **STATUS_CODE** | Numeric error code. ||
|| **STATUS_RESULT** | SIP registration status. Possible values:
- `success` — successful registration
- `error` — registration failed
- `in_progress` — registration in progress
- `wait` — waiting to start registration
||
|#