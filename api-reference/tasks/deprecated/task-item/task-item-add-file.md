# Upload a file to the task task.item.addfile

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method uploads a file to a task. Currently, file upload is implemented via `post` with the file content being passed in the `CONTENT` parameter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **TASK_ID** | Task identifier ||
|| **NAME**
[`string`](../../../data-types.md) | File name ||
|| **CONTENT** | File content in `base64` format ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASK_ID":"140","FILE":{"NAME":"desc.txt","CONTENT":"BASE64_ENCODED_CONTENT_OF_DESC.TXT"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.addfile
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASK_ID":"140","FILE":{"NAME":"desc.txt","CONTENT":"BASE64_ENCODED_CONTENT_OF_DESC.TXT"},"auth":"z3eamwwkpgl7u18kx14q1s4c0ffckqsn"}' \
    https://**put_your_bitrix24_address**/rest/task.item.addfile
    ```

- JS

    ```js
    BX24.callMethod(
        "task.item.addfile",
        {
            TASK_ID: "140",
            FILE: {
                NAME: "desc.txt",
                CONTENT: "BASE64_ENCODED_CONTENT_OF_DESC.TXT"
            }
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.item.addfile',
        [
            'TASK_ID' => "140",
            'FILE' => [
                'NAME' => 'desc.txt',
                'CONTENT' => base64_encode(file_get_contents($_SERVER['DOCUMENT_ROOT'] .'/desc.txt'))
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}