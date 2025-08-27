# Add File to Chat im.disk.file.commit

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

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.disk.file.commit` publishes the uploaded file to the chat.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`unknown`](../../data-types.md) | `17` | Identifier of the chat | 18 ||
|| **UPLOAD_ID^*^**
[`unknown`](../../data-types.md) | `213` | Identifier of the uploaded file via DISK module methods | 18 ||
|| **DISK_ID^*^**
[`unknown`](../../data-types.md) | `112` | Identifier of the file available from the local disk | 18 ||
|| **MESSAGE**
[`unknown`](../../data-types.md) | `Important document` | Description of the file that will be published in the chat | 18 ||
|| **SILENT_MODE**
[`unknown`](../../data-types.md) | `N` | Parameter for Open Channel chats, indicates whether the file information will be sent to the client or not | 18 ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

To successfully call the API, you need to specify `CHAT_ID` and **one of two** fields – `UPLOAD_ID` or `DISK_ID`.

{% note warning %}

The file must be uploaded in advance using the method [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md).

{% endnote %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'im.disk.file.commit',
    		{
    			'CHAT_ID': 17,
    			'UPLOAD_ID': 112,
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
                'im.disk.file.commit',
                [
                    'CHAT_ID'  => 17,
                    'UPLOAD_ID' => 112,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error committing file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.disk.file.commit',
        {
            'CHAT_ID': 17,
            'UPLOAD_ID': 112,
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
        'im.disk.file.commit',
        Array(
            'CHAT_ID' => 17,
            'UPLOAD_ID' => 112,
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

- cURL

    // example for cURL

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": true
}
```

## Response on Error

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|| **ACCESS_ERROR** | The current user does not have access permission to the dialog ||
|| **FILES_ERROR** | File identifiers not provided ||
|#