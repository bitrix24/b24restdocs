# Get a List of Workgroups socialnetwork.api.workgroup.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`socialnetwork`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `socialnetwork.api.workgroup.list` returns a list of workgroups, projects, scrums, and collaborations based on the current user's permissions.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`object`](../data-types.md) | An object for filtering in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

See below for the [list of available fields for filtering](#filterable).

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

If `IS_ADMIN = Y` is not passed in `params`, the method automatically adds a check for the current user's permissions `CHECK_PERMISSIONS`.

The method also always adds a filter by site:

- for extranet users, the extranet site is used
- for others — the site from `params[siteId]` or the current account site ||
|| **select**
[`array`](../data-types.md) | An array containing the list of fields to select.

See below for the [list of available fields for selection](#selectable).

If the parameter is not passed or is empty, only `ID` is selected. ||
|| **order**
[`object`](../data-types.md) | A sorting object in the format `{"field_1": "order_1", ..., "field_N": "order_N"}`.

Possible values for `field` correspond to the fields from the [list of available fields for filtering](#filterable).

Possible values for `order`:

- `ASC` — ascending order
- `DESC` — descending order ||
|| **params**
[`object`](../data-types.md) | Additional [request parameters](#params) ||
|| **start**
[`integer`](../data-types.md) | Pagination parameter.

The page size for results is 50 records.

To get the second page, pass `50`; for the third — `100`, and so on.

Formula: `start = (N - 1) * 50`, where `N` is the page number.

If `-1` is passed, the response will not include the `total` field. ||
|#

### Available Fields for Filtering {#filterable}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Group identifier ||
|| **NAME**
[`string`](../data-types.md) | Group name ||
|| **OWNER_ID**
[`integer`](../data-types.md) | Owner identifier ||
|| **ACTIVE**
[`string`](../data-types.md) | Group activity status: `Y` or `N` ||
|| **VISIBLE**
[`string`](../data-types.md) | Group visibility in the general list: `Y` or `N` ||
|| **OPENED**
[`string`](../data-types.md) | Is the group open for free membership: `Y` or `N` ||
|| **CLOSED**
[`string`](../data-types.md) | Is the group archived: `Y` or `N` ||
|| **PROJECT**
[`string`](../data-types.md) | Object type: `Y` — project, `N` — group ||
|| **SUBJECT_ID**
[`integer`](../data-types.md) | Group subject identifier ||
|| **SITE_ID**
[`string`](../data-types.md) | Group site identifier ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Group creation date ||
|| **DATE_UPDATE**
[`datetime`](../data-types.md) | Group modification date ||
|| **DATE_ACTIVITY**
[`datetime`](../data-types.md) | Last activity date ||
|#

### Available Fields for Selection {#selectable}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Group identifier ||
|| **ACTIVE**
[`string`](../data-types.md) | Group activity status ||
|| **SUBJECT_ID**
[`integer`](../data-types.md) | Group subject identifier ||
|| **NAME**
[`string`](../data-types.md) | Group name ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Group description ||
|| **KEYWORDS**
[`string`](../data-types.md) | Group keywords ||
|| **CLOSED**
[`string`](../data-types.md) | Archive group status ||
|| **VISIBLE**
[`string`](../data-types.md) | Group visibility status ||
|| **OPENED**
[`string`](../data-types.md) | Open group status ||
|| **PROJECT**
[`string`](../data-types.md) | Project status ||
|| **LANDING**
[`string`](../data-types.md) | Group for publication status ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Creation date ||
|| **DATE_UPDATE**
[`datetime`](../data-types.md) | Modification date ||
|| **DATE_ACTIVITY**
[`datetime`](../data-types.md) | Last activity date ||
|| **IMAGE_ID**
[`integer`](../data-types.md) | User avatar identifier ||
|| **AVATAR_TYPE**
[`string`](../data-types.md) | System avatar type ||
|| **OWNER_ID**
[`integer`](../data-types.md) | Owner identifier ||
|| **NUMBER_OF_MEMBERS**
[`integer`](../data-types.md) | Number of participants ||
|| **NUMBER_OF_MODERATORS**
[`integer`](../data-types.md) | Number of moderators ||
|| **INITIATE_PERMS**
[`string`](../data-types.md) | Permissions to invite participants ||
|| **PROJECT_DATE_START**
[`datetime`](../data-types.md) | Project start date ||
|| **PROJECT_DATE_FINISH**
[`datetime`](../data-types.md) | Project end date ||
|| **SCRUM_OWNER_ID**
[`integer`](../data-types.md) | Scrum owner identifier ||
|| **SCRUM_MASTER_ID**
[`integer`](../data-types.md) | Scrum master identifier ||
|| **SCRUM_SPRINT_DURATION**
[`integer`](../data-types.md) | Sprint duration in seconds ||
|| **SCRUM_TASK_RESPONSIBLE**
[`string`](../data-types.md) | Default executor in scrum ||
|| **TYPE**
[`string`](../data-types.md) | Group type: `group`, `project`, `scrum`, `collab` ||
|| **AVATAR**
[`string`](../data-types.md) | Avatar URL ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **IS_ADMIN**
[`string`](../data-types.md) | Disable permission check.

Possible values:
- `Y` — disable permission check if the current user is an administrator

If `Y` is passed by a non-administrator, the value is ignored. ||
|| **siteId**
[`string`](../data-types.md) | Identifier of the site to be used in the automatic filter `SITE_ID` for regular users.

For extranet users, this value is ignored: the method always uses the extranet site. ||
|| **mode**
[`string`](../data-types.md) | Response mode.

Supported value:
- `mobile` — adds the `additionalData` field to each list item

The `additionalData` field has the structure:
  - `role` — the current user's role in the group
  - `initiatedByType` — who initiated the user's connection to the group:
    - `U` — the user themselves (e.g., sent a request to join)
    - `G` — the group (e.g., the user was invited)
  - `features` — list of available group tools (returned if `features`/`mandatoryFeatures` are passed) ||
|| **features**
[`array`](../data-types.md) | List of group tool codes to consider when forming `additionalData` in `mobile` mode ||
|| **mandatoryFeatures**
[`array`](../data-types.md) | List of tool codes that should always be included in `additionalData` in `mobile` mode ||
|| **shouldSelectDialogId**
[`string`](../data-types.md) | Whether to add a field with the chat identifier `dialogId` to the list item.

Possible values:
- `Y` — add `dialogId`
- `N` — do not add `dialogId`
  
Default — `N` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ACTIVE":"Y","CLOSED":"N","%NAME":"group"},"select":["ID","NAME","TYPE","AVATAR"],"order":{"ID":"DESC"},"params":{"mode":"mobile","shouldSelectDialogId":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/socialnetwork.api.workgroup.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ACTIVE":"Y","CLOSED":"N","%NAME":"group"},"select":["ID","NAME","TYPE","AVATAR"],"order":{"ID":"DESC"},"params":{"mode":"mobile","shouldSelectDialogId":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/socialnetwork.api.workgroup.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.
    try {
        const response = await $b24.callListMethod(
            'socialnetwork.api.workgroup.list',
            {
                filter: { ACTIVE: 'Y', CLOSED: 'N', '%NAME': 'group' },
                select: ['ID', 'NAME', 'TYPE', 'AVATAR'],
                order: { ID: 'DESC' },
                params: { mode: 'mobile', shouldSelectDialogId: 'Y' }
            },
            (progress) => { console.log('Progress:', progress) }
        );

        const items = response.getData() || [];
        for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
        console.error('Request failed', error);
    }

    // fetchListMethod: Retrieves data in parts using an iterator. Use for large volumes of data for efficient memory consumption.
    try {
        const generator = $b24.fetchListMethod(
            'socialnetwork.api.workgroup.list',
            {
                filter: { ACTIVE: 'Y', CLOSED: 'N', '%NAME': 'group' },
                select: ['ID', 'NAME', 'TYPE'],
                order: { ID: 'DESC' }
            },
            'ID'
        );
        for await (const page of generator) {
            for (const entity of page) { console.log('Entity:', entity) }
        }
    } catch (error) {
        console.error('Request failed', error);
    }

    // callMethod: Manual control of pagination through the start parameter.
    try {
        const response = await $b24.callMethod(
            'socialnetwork.api.workgroup.list',
            {
                filter: { ACTIVE: 'Y', CLOSED: 'N', '%NAME': 'group' },
                select: ['ID', 'NAME', 'TYPE'],
                order: { ID: 'DESC' }
            },
            0
        );
        const result = response.getData().result.workgroups || [];
        for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
        console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'socialnetwork.api.workgroup.list',
                [
                    'filter' => ['ACTIVE' => 'Y', 'CLOSED' => 'N', '%NAME' => 'group'],
                    'select' => ['ID', 'NAME', 'TYPE', 'AVATAR'],
                    'order' => ['ID' => 'DESC'],
                    'params' => [
                        'mode' => 'mobile',
                        'shouldSelectDialogId' => 'Y',
                    ],
                ]
            );

        print_r($response->getResponseData()->getResult());
    } catch (\Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'socialnetwork.api.workgroup.list',
        {
            filter: { ACTIVE: 'Y', CLOSED: 'N', '%NAME': 'group' },
            select: ['ID', 'NAME', 'TYPE', 'AVATAR'],
            order: { ID: 'DESC' },
            params: { mode: 'mobile', shouldSelectDialogId: 'Y' }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'socialnetwork.api.workgroup.list',
        [
            'filter' => ['ACTIVE' => 'Y', 'CLOSED' => 'N', '%NAME' => 'group'],
            'select' => ['ID', 'NAME', 'TYPE', 'AVATAR'],
            'order' => ['ID' => 'DESC'],
            'params' => [
                'mode' => 'mobile',
                'shouldSelectDialogId' => 'Y',
            ],
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "workgroups": [
                {
            "id": "5",
            "name": "Open group for everyone",
            "type": "group",
            "imageId": "5",
            "avatarType": null,
            "avatar": "https://test.bitrix24.com/b13743910/resize_cache/5/7acf4caaf5d8/socialnetwork/8d6/8d2c04ece929572/3.png",
            "additionalData": {
            "role": "",
            "initiatedByType": ""
            },
            "dialogId": ""
        },
        {
            "id": "1",
            "name": "Closed visible group",
            "type": "group",
            "imageId": "1",
            "avatarType": null,
            "avatar": "",
            "additionalData": {
            "role": "",
            "initiatedByType": ""
            },
            "dialogId": "chat177"
        }
        ]
    },
    "total": 2,
    "time": {
        "start": 1774357689,
        "finish": 1774357689.398272,
        "duration": 0.3982720375061035,
        "processing": 0,
        "date_start": "2026-03-24T16:08:09+02:00",
        "date_finish": "2026-03-24T16:08:09+02:00",
        "operating_reset_at": 1774358289,
        "operating": 0.12220001220703125
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root object of the response ||
|| **workgroups**
[`object[]`](../data-types.md) | List of workgroups.

The structure of the object depends on the fields passed in `select` and the parameters in `params`.

If no groups are found by the filter, `workgroups` will return an empty array. ||
|| **next**
[`integer`](../data-types.md) | Offset for the next page. The field is returned if there are more records. ||
|| **total**
[`integer`](../data-types.md) | Total number of records. The field is not returned if the request is executed with `start = -1`. ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-create.md)
- [{#T}](./sonet-group-update.md)
- [{#T}](./socialnetwork-api-workgroup-get.md)
- [{#T}](./sonet-group-get.md)
- [{#T}](./sonet-group-delete.md)