# Send a message to a user on behalf of the open channel imopenlines.network.message.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

> Scope: [`imopenlines, imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imopenlines.network.message.add` sends a message to a user on behalf of the open channel.

## Method Limitations

- A message can be sent no more than once for each user within one week. 
  There are no restrictions for accounts with a Partner (NFR) license.

- The keyboard can only be used for formatting the button link to an external site.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
||**Name**
`type`|**Description**||
|| **CODE*** 
[`unknown`](../../data-types.md) | Open Channel code, for example `ab515f5d85a8b844d484f6ea75a2e494` ||
|| **USER_ID*** 
[`int`](../../data-types.md) | ID of the message recipient, for example `2` ||
|| **MESSAGE*** 
[`string`](../../data-types.md) | Message text, formatting is available [here](../../chats/messages/index.md) ||
|| **ATTACH**
[`unknown`](../../data-types.md) | Attachment, format described in the article [How to use attachments](../../chats/messages/attachments/index.md) ||
|| **KEYBOARD**
[`unknown`](../../data-types.md) | Keyboard, information on usage in the article [Working with keyboards](../../chats/messages/keyboards.md) ||
|| **URL_PREVIEW**
[`unknown`](../../data-types.md) | Transforming links into expanded links, defaults to `Y` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"ab515f5d85a8b844d484f6ea75a2e494","USER_ID":2,"MESSAGE":"message text","ATTACH":"","KEYBOARD":"","URL_PREVIEW":"Y","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imopenlines.network.message.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'imopenlines.network.message.add',
    		{
    			'CODE': 'ab515f5d85a8b844d484f6ea75a2e494',
    			'USER_ID': 2,
    			'MESSAGE': 'message text',
    			'ATTACH': '',
    			'KEYBOARD': '',
    			'URL_PREVIEW': 'Y'
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log('Success:', result);
    }
    catch( error )
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
                'imopenlines.network.message.add',
                [
                    'CODE'        => 'ab515f5d85a8b844d484f6ea75a2e494',
                    'USER_ID'     => 2,
                    'MESSAGE'     => 'message text',
                    'ATTACH'      => '',
                    'KEYBOARD'    => '',
                    'URL_PREVIEW' => 'Y',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding network message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'imopenlines.network.message.add',
        {
            'CODE': 'ab515f5d85a8b844d484f6ea75a2e494',
            'USER_ID': 2,
            'MESSAGE': 'message text',
            'ATTACH': '',
            'KEYBOARD': '',
            'URL_PREVIEW': 'Y'
        },
        function(result) {
            if(result.error()) {
                console.error("Error: ", result.error());
            } else {
                console.log("Success: ", result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imopenlines.network.message.add',
        [
            'CODE' => 'ab515f5d85a8b844d484f6ea75a2e494',
            'USER_ID' => 2,
            'MESSAGE' => 'message text',
            'ATTACH' => '',
            'KEYBOARD' => '',
            'URL_PREVIEW' => 'Y'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

```json
{
	"result": true
}
```

**Execution result**: `true` in case of success or an error.

## Error Handling

```json
{
    "error": "CODE_ERROR",
    "error_description": "Open Channel code is incorrect"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `CODE_ERROR` | Invalid open channel code ||
|| `USER_ID_EMPTY` | User ID is missing ||
|| `MESSAGE_EMPTY` | Message text is missing ||
|| `ATTACH_ERROR ` | Attachment object failed validation ||
|| `ATTACH_OVERSIZE` | Exceeded maximum allowed attachment size (30 KB) ||
|| `KEYBOARD_ERROR` | The provided keyboard object failed validation ||
|| `KEYBOARD_OVERSIZE` | Exceeded maximum allowed keyboard size (30 KB) ||
|| `USER_MESSAGE_LIMIT` | Message limit exceeded for the specific user ||
|| `WRONG_REQUEST` | Something went wrong ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](../../chats/messages/keyboards.md)
- [{#T}](../../chats/messages/attachments/index.md)
- [{#T}](../../chats/messages/index.md)