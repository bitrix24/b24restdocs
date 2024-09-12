# Delete File from Chat Folder im.disk.file.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed to meet writing standards
- parameter types not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.disk.file.delete` removes files from within the chat folder.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **CHAT_ID^*^**
[`unknown`](../../data-types.md) | `17` | Chat identifier | 18 ||
|| **DISK_ID^*^**
[`unknown`](../../data-types.md) | `112` | File identifier | 18 ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```javascript
    BX24.callMethod(
        'im.disk.file.delete',
        {
            'CHAT_ID': 17,
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

- PHP

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.disk.file.delete',
        Array(
            'CHAT_ID' => 17,
            'DISK_ID' => 112,
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}

## Successful Response

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
|| **FILE_ID_EMPTY** | File identifier not provided ||
|#