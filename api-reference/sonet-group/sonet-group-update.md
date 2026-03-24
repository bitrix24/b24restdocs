# Change group or project sonet_group.update

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: administrator or owner of the group or project

The method `sonet_group.update` modifies the parameters of a workgroup or project.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **GROUP_ID***
[`integer`](../data-types.md) | Identifier of the group or project.

The identifier can be obtained using the [sonet_group.get](./sonet-group-get.md) method. ||
|| **NAME**
[`string`](../data-types.md) | New name. ||
|| **DESCRIPTION**
[`string`](../data-types.md) | New description. ||
|| **VISIBLE**
[`string`](../data-types.md) | Visibility of the group in the list.

Possible values:
- `Y` — group is visible in the general list
- `N` — group is hidden from the general list

For extranet users, the value is forcibly set to `N`. ||
|| **OPENED**
[`string`](../data-types.md) | Is the group open for free joining?

Possible values:
- `Y` — user can join the group without confirmation
- `N` — joining by invitation or request

For extranet users, the value is forcibly set to `N`. ||
|| **CLOSED**
[`string`](../data-types.md) | Is the group archived?

Possible values:
- `Y` — group is archived
- `N` — active group. ||
|| **KEYWORDS**
[`string`](../data-types.md) | Keywords separated by commas. ||
|| **INITIATE_PERMS**
[`string`](../data-types.md) | Who can invite participants.

Possible values:
- `A` — only the owner of the group
- `E` — owner and moderators
- `K` — all participants. ||
|| **PROJECT_DATE_START**
[`datetime`](../data-types.md) | Project start date in ISO-8601 format. ||
|| **PROJECT_DATE_FINISH**
[`datetime`](../data-types.md) | Project end date in ISO-8601 format. ||
|| **OWNER_ID**
[`integer`](../data-types.md) | New owner of the group.

The user identifier can be obtained using the [user.get](../user/user-get.md) method. ||
|| **IMAGE**
[`file`](../data-types.md) | New group avatar in [Base64](../files/how-to-upload-files.md#how-to-encode-file-to-base64) format. ||
|| **IMAGE_FILE_ID**
[`integer`](../data-types.md) | File identifier from Drive for setting the avatar.

The file identifier can be obtained using the [disk.storage.getchildren](../disk/storage/disk-storage-get-children.md) and [disk.folder.getchildren](../disk/folder/disk-folder-get-children.md) methods.

To delete the avatar, pass `IMAGE_FILE_ID: 0`. The avatar will be removed even if `IMAGE` is passed simultaneously; it will not be used. ||
|| **SITE_ID**
[`array`](../data-types.md) | List of sites to which the group is linked. ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":77,"NAME":"New project title"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"GROUP_ID":77,"NAME":"New project title","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.update',
            {
                GROUP_ID: 77,
                NAME: 'New project title'
            }
        );
        
        const result = response.getData().result;
        console.log('Updated group with ID:', result);
        
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
                'sonet_group.update',
                [
                    'GROUP_ID' => 77,
                    'NAME' => 'New project title'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sonet_group.update',
        {
            GROUP_ID: 77,
            NAME: 'New project title'
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
        'sonet_group.update',
        [
            'GROUP_ID' => 77,
            'NAME' => 'New project title'
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
    "result": 77,
    "time": {
        "start": 1773923568,
        "finish": 1773923568.505599,
        "duration": 0.5055990219116211,
        "processing": 0,
        "date_start": "2026-03-19T15:32:48+01:00",
        "date_finish": "2026-03-19T15:32:48+01:00",
        "operating_reset_at": 1773924168,
        "operating": 0.3419780731201172
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the updated group or project. ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Wrong group ID"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| — | `Wrong group ID` | Incorrect `GROUP_ID` provided. ||
|| — | `User has no permissions to update group` | Insufficient rights to modify the group or project. ||
|| — | `Cannot update group` | Failed to update the group or project. ||
|| `ERROR_IMAGE_ID` | `Image is incorrect: Invalid file type` | Incorrect `IMAGE` provided. ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-create.md)
- [{#T}](./socialnetwork-api-workgroup-get.md)
- [{#T}](./socialnetwork-api-workgroup-list.md)
- [{#T}](./sonet-group-get.md)
- [{#T}](./sonet-group-delete.md)