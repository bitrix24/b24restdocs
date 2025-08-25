# Update Existing SIP Line voximplant.sip.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.sip.update` updates an existing SIP line (created by the application). This method is available to the holder of the [access permissions](https://helpdesk.bitrix24.com/open/18216960/) `Manage numbers - change - any`.

#| 
|| **Parameter** | **Description** ||
|| **CONFIG_ID**^*^ | Identifier of the SIP line configuration. ||
|| **TYPE** | Type of PBX, a list of PBX types, optional parameter, defaults to `Cloud PBX`. ||
|| **TITLE** | Name of the connection (optional field). ||
|| **SERVER** | Address of the SIP registration server (optional field). ||
|| **LOGIN** | Login for the server (optional field). ||
|| **PASSWORD** | Password for the server (optional field). ||
|#

{% include [Footnote on parameters](../../../../_includes/required.md) %}

To successfully call, at least one of the fields must be present: `TITLE`, `SERVER`, `LOGIN`, `PASSWORD`.

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"voximplant.sip.update",
    		{
    			"CONFIG_ID": 69,
    			"TITLE": "line name",
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
                'voximplant.sip.update',
                [
                    'CONFIG_ID' => 69,
                    'TITLE'     => 'line name',
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
        echo 'Error updating SIP line: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "voximplant.sip.update",
        {
            "CONFIG_ID": 69,
            "TITLE": "line name",
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

## Response on Success

Returns 1 on successful execution or an exception.

## Specific Error Codes

#| 
|| **Code** | **Description** ||
|| TITLE_EXISTS | A line with this name already exists. ||
|#