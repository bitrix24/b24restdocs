# Get Chat Data imbot.dialog.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.dialog.get` retrieves information about a dialog.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DIALOG_ID^*^**
[`unknown`](../../data-types.md) | `chat29`
or
`256` | Identifier of the dialog. Format:
- **chatXXX** – recipient's chat if the message is for a chat
- **XXX** – recipient's identifier if the message is for a private dialog | 24 ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'imbot.dialog.get',
    		{
    			DIALOG_ID: 'chat29'
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.dialog.get',
                [
                    'DIALOG_ID' => 'chat29',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting dialog: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'imbot.dialog.get',
        {
            DIALOG_ID: 'chat29'
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'imbot.dialog.get',
        Array(
            'DIALOG_ID' => 'chat29'
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

- cURL

    // example for cURL

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Successful Response

```json
{
    "result":
{
    "id": "21191",
    "title": "Mint Chat #3",
    "owner": "2",
    "extranet": false,
    "avatar": "",
    "color": "#4ba984",
    "type": "chat",
    "entity_type": "",
    "entity_data_1": "",
    "entity_data_2": "",
    "entity_data_3": "",
    "date_create": "2017-10-14T12:15:32+02:00",
    "message_type": "C"
}
}
```

### Key Descriptions

- **id** – chat identifier
- **title** – chat title
- **owner** – identifier of the chat owner
- **extranet** – indicator of external extranet user participation in the chat (`true/false`)
- **color** – chat color in hex format
- **avatar** – link to the avatar (if empty, the avatar is not set)
- **type** – type of chat (group chat, call chat, open channel chat, etc.)
- **entity_type** – external code for the chat – type
- **entity_id** – external code for the chat – identifier
- **entity_data_1** – external data for the chat
- **entity_data_2** – external data for the chat
- **entity_data_3** – external data for the chat
- **date_create** – chat creation date in ATOM format
- **message_type** – type of chat messages

## Error Response

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

### Key Descriptions

- **error** – code of the occurred error
- **error_description** – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **DIALOG_ID_EMPTY** | Dialog identifier not provided ||
|| **ACCESS_ERROR** | Current user does not have access permission to the dialog ||
|#