# Register SMS Provider or Message Provider messageservice.sender.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- no success response is provided
- no error response is provided
- No examples in other languages

{% endnote %}

{% endif %}

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

This method registers a new message provider.

#|
|| **Parameter** | **Description** ||
|| **CODE** | Internal identifier of the provider. Allowed characters are a-z, A-Z, 0-9, dot, dash, and underscore. ||
|| **TYPE** | Type of the provider. ||
|| **HANDLER** | URL of the application to which data will be sent. ||
|| **NAME** | Name of the provider. It can be a string or an associative array of localized strings. ||
|| **DESCRIPTION** | Description of the provider. It can be a string or an associative array of localized strings. ||
|#

{% include [Footnote on parameters](../../_includes/required.md) %}

Data is sent to HANDLER:

- **module_id** - initiating module. `crm` means the message was sent from a detail form (other options may be available in the future), `bizproc` means sent from Business Processes or an Automation rule.
- **bindings** - parameter relevant only for module_id = crm. It contains an array of message bindings to CRM entities (what the activity will be linked to).
- **workflow_id**, **document_id**, **document_type** - parameters relevant only for module_id = bizproc. These parameters are not always present: if sent from a detail form, they will not be included.
- **message_id** - unique identifier of the message. It can be used to refer to [messageservice.message.status.update](messageservice-message-status-update.md).
- **message_to** - recipient's phone number
- **message_body** - text of the message

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'messageservice.sender.add',
    		{
    			CODE: 'provider1',
    			TYPE: 'SMS',
    			HANDLER: 'http:///',
    			NAME: 'Provider ***.com',
    			DESCRIPTION: 'Provider ***.com'
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		alert("Error: " + result.error());
    	else
    		alert("Success: " + result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $params = [
            'CODE'        => 'provider1',
            'TYPE'        => 'SMS',
            'HANDLER'     => 'http:///',
            'NAME'        => 'Provider ***.com',
            'DESCRIPTION' => 'Provider ***.com',
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'messageservice.sender.add',
                $params
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding sender: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        CODE: 'provider1',
        TYPE: 'SMS',
        HANDLER: 'http:///',
        NAME: 'Provider ***.com',
        DESCRIPTION: 'Provider ***.com'
    };
    BX24.callMethod(
        'messageservice.sender.add',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

{% endlist %}

**Sending from CRM detail form**

```plaintext
Array
(
    [module_id] => crm
    [bindings] => Array
        (
            [0] => Array
                (
                    [OWNER_TYPE_ID] => 1
                    [OWNER_ID] => 98
                )

        )

    [properties] => Array
        (
            [phone_number] => +1*********
            [message_text] => test message
    )

    [type] => SMS
    [code] => example
    [message_id] => 72dd742c8270db0ddbbab92f98877537
    [message_to] => +1**********
    [message_body] => test message
    [ts] => 1506687055
    [auth] => /*auth*/

)
```

**Sending from Business Process or Automation rule.**

```plaintext
Array
(
    [module_id] => bizproc
    [workflow_id] => 59ce38567ff2a5.26351167
    [document_id] => Array
        (
            [0] => crm
            [1] => CCrmDocumentLead
            [2] => LEAD_98
        )

    [document_type] => Array
        (
            [0] => crm
            [1] => CCrmDocumentLead
            [2] => LEAD
        )

    [properties] => Array
        (
            [phone_number] => +1*********
            [message_text] => test message
        )

    [type] => SMS
    [code] => example
    [message_id] => 8b3fc6cd0cb4a7b91f6632889cdf46e0
    [message_to] => +1*********
    [message_body] => test message
    [ts] => 1506687103
    [auth] => /*auth*/

)
```
{% include [Footnote on examples](../../_includes/examples.md) %}