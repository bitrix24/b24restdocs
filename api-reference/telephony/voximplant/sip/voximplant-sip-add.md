# Create a new SIP line linked to the application voximplant.sip.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

{% include notitle [Scope telephony admin](../../_includes/scope-telephony-admin.md) %}

The method `voximplant.sip.add` creates a new SIP line linked to the application. After creation, this line becomes the default outgoing line. The method is available to the holder of the [access permissions](https://helpdesk.bitrix24.com/open/18216960/) `Manage numbers - change - any`.

#|
|| **Parameter** | **Description** ||
|| **TYPE** | Type of PBX. Possible values:
- `cloud` — cloud PBX
- `office` — office PBX

Default is `cloud` ||
|| **TITLE**^*^ | Connection name. ||
|| **SERVER**^*^ | SIP registration server address. ||
|| **LOGIN**^*^ | Server login. ||
|| **PASSWORD**^*^ | Server password. ||
|#

{% include [Footnote on parameters](../../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'voximplant.sip.add',
    		{
    			"TYPE": "cloud",
    			"TITLE": "sipnet",
    			"SERVER": "sipnet.com",
    			"LOGIN": "YYYYY",
    			"PASSWORD": "ZZZZZ"
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
                'voximplant.sip.add',
                [
                    'TYPE'     => 'cloud',
                    'TITLE'    => 'sipnet',
                    'SERVER'   => 'sipnet.com',
                    'LOGIN'    => 'YYYYY',
                    'PASSWORD' => 'ZZZZZ',
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
        echo 'Error adding SIP: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'voximplant.sip.add',
        {
            "TYPE": "cloud",
            "TITLE": "sipnet",
            "SERVER": "sipnet.com",
            "LOGIN": "YYYYY",
            "PASSWORD": "ZZZZZ"
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
|| **CONFIG_ID** | Identifier of the SIP line configuration. ||
|| **TYPE** | Type of PBX. Possible values:
- `cloud` — cloud PBX
- `office` — office PBX ||
|| **TITLE** | Connection name. ||
|| **SERVER** | SIP registration server address for Cloud PBX or server address for Office PBX. ||
|| **LOGIN** | Server login. ||
|| **PASSWORD** | Server password. ||
|| **REG_ID** | SIP registration identifier (only for Cloud PBX). ||
|| **INCOMING_SERVER** | Server address for connection during outgoing calls (only for Cloud PBX). ||
|| **INCOMING_LOGIN** | Login for connection (only for Cloud PBX). ||
|| **INCOMING_PASSWORD** | Password for connection (only for Cloud PBX). ||
|#

## Specific Error Codes

#|
|| **Code** | **Description** ||
|| **MAX_CLOUD_PBX** | You cannot connect more than 5 cloud PBXs. ||
|| **TITLE_EXISTS** | A line with this name already exists. ||
|#