# How to Create a Comment in a Task and Attach a File

> Scope: [`task`](../../api-reference/scopes/permissions.md), [`im`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: a user with access to the task and its chat
>
> - [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) — any user with access to the task
> - [im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md) — a user with access to the task chat
> - [im.v2.File.download](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-download.md) — a user with access to the task chat

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Task comments are stored in the task chat. To add a comment with a file, first get the identifier of the task chat, then upload the file to that chat using the [im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method.

The [im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method uploads the file, attaches it to the chat, and sends the message in a single call. You do not need to upload the file to Drive using a separate method.

The scenario consists of two steps.

1. Get the task chat `chatId` using the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method
2. Send a message with a file using the [im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method

As a result, a comment with an attached file will appear in the task chat. The operation is confirmed by the `messageId`, `chatId`, `dialogId`, and `file.id` fields in the response of the [im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method.

## Before You Start

You need the following to run the example:

- an inbound webhook with the `task` and `im` scopes
- the task `taskId`. You can get it using the [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method
- the file to attach to the comment
- the file name with an extension, for example `file.pdf`
- the file content in Base64 format without the `data:*/*;base64,` prefix

The webhook executes requests with the permissions of the user who created it. Do not publish the webhook secret in client-side code or repositories. Store it in environment variables.

For server-side JS examples with `B24Hook`, use Node.js 18, 20, 22, or later. For new projects, use 22 or later. B24JsSDK is an ES module: save the code in a `.mjs` file or add `"type": "module"` to `package.json`.

For b24pysdk examples, use Python 3.9 or later.

## 1. Get the Task Chat chatId

To send a message with a file to the task chat, you need a dialog identifier in the `chat{chatId}` format. Get `chatId` using the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method.

Use the following parameters:

- `taskId` — the task identifier
- `select` — the array of fields to return. Specify `ID` and `CHAT_ID`

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const taskResponse = await $b24.actions.v2.call.make({
        method: 'tasks.task.get',
        params: {
            taskId: 3711,
            select: ['ID', 'CHAT_ID']
        },
        requestId: 'task-get-chat'
    })

    if (!taskResponse.isSuccess) {
        throw new Error(taskResponse.getErrorMessages().join('; '))
    }

    const chatId = taskResponse.getData().result.task.chatId
    const dialogId = `chat${chatId}`
    ```

- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Bitrix24\SDK\Core\Exceptions\BaseException;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook(getenv('B24_HOOK'));

    try {
        $task = $serviceBuilder->core->call(
            'tasks.task.get',
            [
                'taskId' => 3711,
                'select' => ['ID', 'CHAT_ID']
            ]
        )->getResponseData()->getResult()['task'];
    } catch (BaseException $e) {
        echo 'Error retrieving the task: ' . $e->getMessage();
        return;
    }

    $chatId = $task['chatId'];
    $dialogId = 'chat' . $chatId;
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    try:
        task = client.tasks.task.get(
            bitrix_id=3711,
            select=["ID", "CHAT_ID"],
        ).response.result["task"]
    except BitrixAPIError as error:
        print(f"Error retrieving the task: {error}")
        raise

    chat_id = task["chatId"]
    dialog_id = f"chat{chat_id}"
    ```

{% endlist %}

As a result, you get the task chat `chatId`. Convert the value `861` into `dialogId`: `chat861`.

```json
{
    "result": {
        "task": {
            "id": "3711",
            "chatId": 861
        }
    }
}
```

## 2. Send a Comment with a File

To send a file to the task chat, use the [im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method.

Use the following parameters:

- `dialogId` — the dialog identifier in the `chat{chatId}` format. For the example from the previous step, this is `chat861`
- `fields.name` — the file name with an extension
- `fields.content` — the file content in Base64 format
- `fields.message` — the comment text

{% list tabs %}

- JS

    ```javascript
    const uploadResponse = await $b24.actions.v2.call.make({
        method: 'im.v2.File.upload',
        params: {
            dialogId,
            fields: {
                name: 'file.pdf',
                content: 'SGVsbG8gV29ybGQh',
                message: 'Comment with a file'
            }
        },
        requestId: 'file-upload-to-task-chat'
    })

    if (!uploadResponse.isSuccess) {
        throw new Error(uploadResponse.getErrorMessages().join('; '))
    }

    const result = uploadResponse.getData().result
    console.log(result.messageId, result.file.id)

    $b24.destroy()
    ```

- PHP

    ```php
    try {
        $response = $serviceBuilder->core->call(
            'im.v2.File.upload',
            [
                'dialogId' => $dialogId,
                'fields' => [
                    'name' => 'file.pdf',
                    'content' => base64_encode(file_get_contents('/path/to/file.pdf')),
                    'message' => 'Comment with a file',
                ],
            ]
        );
    } catch (BaseException $e) {
        echo 'Error sending the comment with a file: ' . $e->getMessage();
        return;
    }

    $result = $response->getResponseData()->getResult();
    echo 'Comment created, MESSAGE_ID: ' . $result['messageId'];
    ```

- Python

    ```python
    import base64
    from pathlib import Path

    file_content = base64.b64encode(Path("file.pdf").read_bytes()).decode()

    try {
        result = token.call_method(
            "im.v2.File.upload",
            {
                "dialogId": dialog_id,
                "fields": {
                    "name": "file.pdf",
                    "content": file_content,
                    "message": "Comment with a file",
                },
            },
        )["result"]
    except BitrixAPIError as error:
        print(f"Error sending the comment with a file: {error}")
        raise

    print(result["messageId"], result["file"]["id"])
    ```

{% endlist %}

The method returns the message identifier `messageId`, the chat identifier `chatId`, the dialog identifier `dialogId`, and file data in the `file` object.

```json
{
    "result": {
        "file": {
            "id": 9817,
            "chatId": 861,
            "type": "file",
            "name": "file.pdf",
            "extension": "pdf",
            "size": 35341,
            "status": "done",
            "progress": 100,
            "authorId": 1
        },
        "messageId": 38655,
        "chatId": 861,
        "dialogId": "chat861"
    }
}
```

## Check the Result

Open the task with `id` `3711` and go to the comments. The task chat must contain a message `Comment with a file` with the attached file `file.pdf`.

Through REST, verify that the task is linked to the same chat where the file was sent and that the file is available for download.

{% list tabs %}

- JS

    ```javascript
    const checkResponse = await $b24.actions.v2.call.make({
        method: 'tasks.task.get',
        params: {
            taskId: 3711,
            select: ['ID', 'CHAT_ID']
        },
        requestId: 'task-get-check'
    })

    if (!checkResponse.isSuccess) {
        throw new Error(checkResponse.getErrorMessages().join('; '))
    }

    const task = checkResponse.getData().result.task
    console.log(task.chatId)

    const fileResponse = await $b24.actions.v2.call.make({
        method: 'im.v2.File.download',
        params: {
            dialogId: result.dialogId,
            fileId: result.file.id
        },
        requestId: 'file-download-check'
    })

    if (!fileResponse.isSuccess) {
        throw new Error(fileResponse.getErrorMessages().join('; '))
    }

    console.log(fileResponse.getData().result)
    ```

- PHP

    ```php
    $task = $serviceBuilder->core->call(
        'tasks.task.get',
        [
            'taskId' => 3711,
            'select' => ['ID', 'CHAT_ID']
        ]
    )->getResponseData()->getResult()['task'];

    echo 'CHAT_ID: ' . $task['chatId'];

    $file = $serviceBuilder->core->call(
        'im.v2.File.download',
        [
            'dialogId' => $result['dialogId'],
            'fileId' => $result['file']['id'],
        ]
    )->getResponseData()->getResult();

    print_r($file);
    ```

- Python

    ```python
    task = client.tasks.task.get(
        bitrix_id=3711,
        select=["ID", "CHAT_ID"],
    ).response.result["task"]

    print(task["chatId"])

    file = token.call_method(
        "im.v2.File.download",
        {
            "dialogId": result["dialogId"],
            "fileId": result["file"]["id"],
        },
    )["result"]

    print(file)
    ```

{% endlist %}

The scenario is successful if the task `chatId` matches the `result.chatId` value returned by [im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md), and the upload response contains the following fields:

- `result.messageId` — the identifier of the message in the task chat
- `result.dialogId` — the identifier of the task dialog
- `result.file.id` — the identifier of the file in Drive
- `result.file.status` — the file upload status. The `done` value means the file was uploaded successfully
- the response of [im.v2.File.download](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-download.md) contains a file download link in the `result.downloadUrl` field

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Error** | **Cause and solution** ||
|| `FILE_EMPTY` | The file name or file content was not passed. Check `fields.name` and `fields.content` ||
|| `FILE_INVALID_CONTENT` | `fields.content` contains an invalid Base64 string or a string with the `data:*/*;base64,` prefix ||
|| `FILE_TOO_LARGE` | The file is larger than 100 MB. Reduce the file size or use a different data transfer approach ||
|| `CHAT_NOT_FOUND` | The chat from `dialogId` was not found. Check that `chatId` was taken from the correct task and passed with the `chat` prefix ||
|| `ACCESS_DENIED` | The webhook user does not have access to the task or the task chat ||
|#

Repeat the scenario from the step that returned the error. If [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) returned the error, verify `taskId` and the user's permissions. If [im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md) returned the error, repeat only the second step.

## What to Consider

- [im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md) replaces the outdated chain `im.disk.folder.get` + upload through Drive + `im.disk.file.commit`
- The file is passed in `fields.content` as a Base64 string without the `data:*/*;base64,` prefix
- `dialogId` for a task chat is built from `chatId`: if `chatId` is `861`, pass `chat861`
- Re-running the example creates a new message with a file in the task chat

## Continue Learning

- [Upload a File to a Chat im.v2.File.upload](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-upload.md)
- [Download a File from a Chat im.v2.File.download](../../api-reference/chat-bots/chat-bots-v2/im.v2/files/file-download.md)
- [Get a Task by ID tasks.task.get](../../api-reference/tasks/tasks-task-get.md)
- [How to Create a Task with an Attached File](./how-to-create-task-with-file.md)
