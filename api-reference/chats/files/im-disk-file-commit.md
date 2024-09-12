# Add File to Chat im.disk.file.commit

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
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
[`unknown`](../../data-types.md) | `213` | Identifier of the uploaded file through the DISK module methods | 18 ||
|| **DISK_ID^*^**
[`unknown`](../../data-types.md) | `112` | Identifier of the file available from the local disk | 18 ||
|| **MESSAGE**
[`unknown`](../../data-types.md) | `Important document` | Description of the file that will be published in the chat | 18 ||
|| **SILENT_MODE**
[`unknown`](../../data-types.md) | `N` | Parameter for Open Lines chat, indicates whether information about the file will be sent to the client or not | 18 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

To successfully call the API, you need to specify `CHAT_ID` and **one of the two** fields – `UPLOAD_ID` or `DISK_ID`.

{% note warning %}

The file must be uploaded in advance using the [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md) method.

{% endnote %}

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

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

- PHP

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
{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

## Success Response

```json
{
    "result": true
}
```

## Error Response

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

### Key Descriptions

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **CHAT_ID_EMPTY** | Chat identifier not provided ||
|| **ACCESS_ERROR** | Current user does not have access permission to the dialog ||
|| **FILES_ERROR** | File identifiers not provided ||
|#