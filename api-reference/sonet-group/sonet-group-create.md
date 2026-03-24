# Create a group or project sonet_group.create

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: a user with the right to create a group or project

The method `sonet_group.create` creates a workgroup or project.

## Method parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../data-types.md) | The name of the group or project ||
|| **DESCRIPTION**
[`string`](../data-types.md) | The description of the group or project ||
|| **VISIBLE**
[`string`](../data-types.md) | The visibility of the group in the list.

Possible values:
- `Y` — the group is visible in the general list
- `N` — the group is hidden from the general list

Default — `Y`. For extranet users, the value is forcibly set to `N` ||
|| **OPENED**
[`string`](../data-types.md) | Is the group open for free joining.

Possible values:
- `Y` — the user can join the group without confirmation
- `N` — joining by invitation or request

Default — `N`. For extranet users, the value is forcibly set to `N` ||
|| **CLOSED**
[`string`](../data-types.md) | Is the group archived.

Possible values:
- `Y` — the group is archived
- `N` — the group is active 

Default — `N` ||
|| **KEYWORDS**
[`string`](../data-types.md) | Keywords separated by commas ||
|| **INITIATE_PERMS**
[`string`](../data-types.md) | Who can invite participants.

Possible values:
- `A` — only the group owner
- `E` — owner and moderators
- `K` — all participants

If the parameter is not provided, the default value is used:
- for intranet users — `K`
- for extranet users — `E` ||
|| **PROJECT**
[`string`](../data-types.md) | Create a project instead of a group.

Possible values:
- `Y` — create a project
- `N` — create a group 

Default — `N` ||
|| **PROJECT_DATE_START**
[`datetime`](../data-types.md) | The project start date in ISO-8601 format ||
|| **PROJECT_DATE_FINISH**
[`datetime`](../data-types.md) | The project end date in ISO-8601 format ||
|| **SCRUM_MASTER_ID**
[`integer`](../data-types.md) | The identifier of the scrum master if the project is created as scrum.

The user ID can be obtained using the [user.get](../user/user-get.md) method ||
|| **OWNER_ID**
[`integer`](../data-types.md) | The identifier of the owner.

The user ID can be obtained using the [user.get](../user/user-get.md) method.

Specifying the owner is available only to administrators. For all others, the current user automatically becomes the owner ||
|| **IMAGE**
[`file`](../data-types.md) | The group's avatar in [Base64](../files/how-to-upload-files.md#how-to-encode-file-to-base64) format ||
|| **IMAGE_FILE_ID**
[`integer`](../data-types.md) | The file ID from Drive for setting the avatar.

The file ID can be obtained using the [disk.storage.getchildren](../disk/storage/disk-storage-get-children.md) and [disk.folder.getchildren](../disk/folder/disk-folder-get-children.md) methods.

If both `IMAGE_FILE_ID` and `IMAGE` are provided, `IMAGE_FILE_ID` takes precedence ||
|| **SITE_ID**
[`array`](../data-types.md) | A list of site IDs to which the group is linked

Default — the current site ||
|| **SUBJECT_ID**
[`integer`](../data-types.md) | The identifier of the group's subject ||
|#

## Code examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"New Project","PROJECT":"Y","VISIBLE":"Y","OPENED":"N","INITIATE_PERMS":"K","IMAGE":["avatar.png","iVBORw0KGgoAAAANSUhEUgAA..."]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.create
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"New Project","PROJECT":"Y","VISIBLE":"Y","OPENED":"N","INITIATE_PERMS":"K","IMAGE":["avatar.png","iVBORw0KGgoAAAANSUhEUgAA..."],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.create
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.create',
            {
                NAME: 'New Project',
                PROJECT: 'Y',
                VISIBLE: 'Y',
                OPENED: 'N',
                INITIATE_PERMS: 'K',
                IMAGE: [
                    'avatar.png',
                    'iVBORw0KGgoAAAANSUhEUgAA...'
                ]
            }
        );
        
        const result = response.getData().result;
        console.log('Created group with ID:', result);
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
                'sonet_group.create',
                [
                    'NAME' => 'New Project',
                    'PROJECT' => 'Y',
                    'VISIBLE' => 'Y',
                    'OPENED' => 'N',
                    'INITIATE_PERMS' => 'K',
                    'IMAGE' => [
                        'avatar.png',
                        'iVBORw0KGgoAAAANSUhEUgAA...'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sonet_group.create',
        {
            NAME: 'New Project',
            PROJECT: 'Y',
            VISIBLE: 'Y',
            OPENED: 'N',
            INITIATE_PERMS: 'K',
            IMAGE: [
                'avatar.png',
                'iVBORw0KGgoAAAANSUhEUgAA...'
            ]
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
        'sonet_group.create',
        [
            'NAME' => 'New Project',
            'PROJECT' => 'Y',
            'VISIBLE' => 'Y',
            'OPENED' => 'N',
            'INITIATE_PERMS' => 'K',
            'IMAGE' => [
                'avatar.png',
                'iVBORw0KGgoAAAANSUhEUgAA...'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response handling

HTTP status: **200**

```json
{
    "result": 77,
    "time": {
        "start": 1773921687,
        "finish": 1773921688.071989,
        "duration": 1.0719890594482422,
        "processing": 1,
        "date_start": "2026-03-19T15:01:27+02:00",
        "date_finish": "2026-03-19T15:01:28+02:00",
        "operating_reset_at": 1773922287,
        "operating": 0.9421911239624023
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | The identifier of the created group or project ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Cannot create group"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Incorrect input data` | Incorrect format of parameters provided ||
|| — | `You have no permissions to create a group` | Insufficient rights to create a group or project ||
|| — | `Cannot create group` | Failed to create a group or project ||
|| `ERROR_IMAGE_ID` | `Image is incorrect: Invalid file type` | An incorrect `IMAGE` was provided ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue learning

- [{#T}](./sonet-group-update.md)
- [{#T}](./socialnetwork-api-workgroup-get.md)
- [{#T}](./socialnetwork-api-workgroup-list.md)
- [{#T}](./sonet-group-get.md)
- [{#T}](./sonet-group-delete.md)