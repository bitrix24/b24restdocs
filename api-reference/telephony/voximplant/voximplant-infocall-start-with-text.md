# Make a call to the specified number with automatic pronunciation of the given text voximplant.infocall.startwithtext

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony all](../_includes/scope-telephony-all.md) %}

The method `voximplant.infocall.startwithtext` makes a call to the specified number with automatic pronunciation of the given text. This method is available to the holder of the [access permission](https://helpdesk.bitrix24.com/open/18216960/) `Outgoing call - Execution - any`.

To access the method, the application must request the call permission. The permission is specified during [application registration](../../../settings/app-installation/index.md).

#|
|| **Parameter** | **Description** ||
|| **FROM_LINE** | ID of the line from which the call will be made. A list of available lines can be obtained using the [voximplant.line.get](lines/voximplant-line-get.md) method. ||
|| **TO_NUMBER** | The number to call. ||
|| **TEXT_TO_PRONOUNCE** | The text to pronounce. ||
|| **VOICE** | The voice to pronounce this text (optional). A list of voices can be obtained using the [voximplant.tts.voices.get](voximplant-tts-voices-get.md) method. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'voximplant.infocall.startwithtext',
    		{
    			"FROM_LINE": "reg1332",
    			"TO_NUMBER": "+17911xxxxxxx",
    			"PRONOUNCE": "Good afternoon. Your request has been completed",
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
                'voximplant.infocall.startwithtext',
                [
                    'FROM_LINE' => 'reg1332',
                    'TO_NUMBER' => '+17911xxxxxxx',
                    'PRONOUNCE' => 'Good afternoon. Your request has been completed',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error starting info call: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'voximplant.infocall.startwithtext',
        {
            "FROM_LINE": "reg1332",
            "TO_NUMBER": "+17911xxxxxxx",
            "PRONOUNCE": "Good afternoon. Your request has been completed",
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