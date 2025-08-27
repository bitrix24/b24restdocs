# Save file to your Drive im.disk.file.save

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.disk.file.save` saves a file to your Bitrix24.Drive.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **DISK_ID^*^**
[`unknown`](../../data-types.md) | `112` | File identifier | 21 ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'im.disk.file.save',
    		{
    			'DISK_ID': 112,
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
                'im.disk.file.save',
                [
                    'DISK_ID' => 112,
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
        echo 'Error saving disk file: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'im.disk.file.save',
        {
            'DISK_ID': 112,
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
        'im.disk.file.save',
        Array(
            'DISK_ID' => 112,
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

- cURL

    // example for cURL

{% endlist %}

{% include [Examples notes](../../../_includes/examples.md) %}

## Response on success

```json
{
"result": {
    "folder": {
        "id": 130,
        "name": "Saved files"
        },
    "file": {
        "id": 578,
        "name": "image.png"
        }
    }
}
```

## Response on error

```json
{
    "error": "FILE_ID_EMPTY",
    "error_description": "File ID can't be empty"
}
```

### Description of keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **FILE_SAVE_ERROR** | Failed to save the file ||
|| **FILE_ID_EMPTY** | File identifier not provided ||
|#