# Check the access permissions of the current user sonet_group.feature.access

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `sonet_group.feature.access` checks whether the current user has access to operations within the group or project functionality.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **GROUP_ID***
[`integer`](../data-types.md) | Identifier of the group or project.

The identifier can be obtained using the method [sonet_group.get](./sonet-group-get.md) ||
|| **FEATURE***
[`string`](../data-types.md) | Symbolic code of the group functionality.

Basic values:
- `photo` — photo gallery
- `calendar` — calendar
- `tasks` — tasks
- `files` — Drive
- `blog` — messages

Additional values may be available depending on the installed modules and event handlers ||
|| **OPERATION***
[`string`](../data-types.md) | Symbolic code of the operation within the functionality.

Allowed values depend on `FEATURE`:

- for `photo`:
  - `view` — view the photo gallery
  - `write` — modify the photo gallery
- for `calendar`:
  - `view` — view the calendar
  - `write` — modify the calendar
- for `tasks`:
  - `view` — view own tasks
  - `view_all` — view all tasks
  - `sort` — sort and move tasks
  - `create_tasks` — create tasks
  - `edit_tasks` — modify all tasks
  - `delete_tasks` — delete all tasks
- for `files`:
  - `view` — view files
  - `write` — modify files
- for `blog`:
  - `view_post` — view messages
  - `premoderate_post` — write messages with pre-moderation
  - `write_post` — write messages
  - `moderate_post` — moderate messages
  - `full_post` — manage messages
  - `view_comment` — view comments
  - `premoderate_comment` — write comments with pre-moderation
  - `write_comment` — write comments
  - `moderate_comment` — moderate comments
  - `full_comment` — manage comments ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":77,"FEATURE":"blog","OPERATION":"write_post"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.feature.access
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":77,"FEATURE":"blog","OPERATION":"write_post","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.feature.access
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.feature.access',
            {
                GROUP_ID: 77,
                FEATURE: 'blog',
                OPERATION: 'write_post'
            }
        );
        
        const result = response.getData().result;
        console.log('Feature access result:', result);
        
        processResult(result);
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
                'sonet_group.feature.access',
                [
                    'GROUP_ID' => 77,
                    'FEATURE' => 'blog',
                    'OPERATION' => 'write_post'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error checking feature access: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sonet_group.feature.access',
        {
            GROUP_ID: 77,
            FEATURE: 'blog',
            OPERATION: 'write_post'
        }, 
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sonet_group.feature.access',
        [
            'GROUP_ID' => 77,
            'FEATURE' => 'blog',
            'OPERATION' => 'write_post'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773930920,
        "finish": 1773930920.159131,
        "duration": 0.15913105010986328,
        "processing": 0,
        "date_start": "2026-03-19T17:35:20+02:00",
        "date_finish": "2026-03-19T17:35:20+02:00",
        "operating_reset_at": 1773931520,
        "operating": 0.10687804222106934
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if the operation is allowed, otherwise `false` ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Wrong operation"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong socialnetwork group ID` | An incorrect `GROUP_ID` was provided ||
|| — | `Wrong feature` | An unsupported value for `FEATURE` was provided ||
|| — | `Wrong operation` | An unsupported value for `OPERATION` was provided ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-get.md)
- [{#T}](./sonet-group-user-groups.md)
- [{#T}](./socialnetwork-api-workgroup-get.md)
- [{#T}](./socialnetwork-api-workgroup-list.md)