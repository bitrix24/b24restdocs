# Get a list of groups and projects sonet_group.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sonet`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `sonet_group.get` returns a list of workgroups and projects considering the permissions of the current user.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ORDER**
[`object`](../data-types.md) | Sorting direction.

Possible values:
- `ASC` — ascending order
- `DESC` — descending order

Default — `ID:'DESC'` ||
|| **FILTER**
[`object`](../data-types.md) | Object for filtering in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

See below [list of available fields for filtering](#filterable).

Supported operators in the filter key:
- `!` — not equal
- `>=` — greater than or equal
- `>` — greater than
- `<=` — less than or equal
- `<` — less than
- `><` — between (inclusive range)
- `!><` — not between (outside the range)
- `?` — string search
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `%` — LIKE, substring search
- `!%` — NOT LIKE, substring search

Default — no filtering ||
|| **GROUP_ID**
[`integer`](../data-types.md) | Return group or project by identifier.

If the parameter is provided, the method adds the filter condition `ID = GROUP_ID` ||
|| **IS_ADMIN**
[`string`](../data-types.md) | Disable permission check.

Possible values:
- `Y` — disable permission check if the current user is an administrator

If `Y` is provided by a non-administrator, the value is ignored.

Default — permission check is enabled ||
|| **start**
[`integer`](../data-types.md) | Pagination parameter.

The page size of results is 50 records.

To get the second page, pass `50`; the third — `100`, and so on.

Formula:

`start = (N - 1) * 50`, where `N` — page number ||
|#

### Available fields for filtering {#filterable}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the group or project ||
|| **NAME**
[`string`](../data-types.md) | Name of the group or project ||
|| **OWNER_ID**
[`integer`](../data-types.md) | Identifier of the owner ||
|| **ACTIVE**
[`string`](../data-types.md) | Activity status of the group.

Possible values:
- `Y` — group is active
- `N` — group is deactivated ||
|| **VISIBLE**
[`string`](../data-types.md) | Visibility of the group in the list.

Possible values:
- `Y` — group is visible in the general list
- `N` — group is hidden from the general list ||
|| **OPENED**
[`string`](../data-types.md) | Is the group open for free membership.

Possible values:
- `Y` — user can join the group without confirmation
- `N` — membership by invitation or request ||
|| **CLOSED**
[`string`](../data-types.md) | Is the group archived.

Possible values:
- `Y` — group is archived
- `N` — active group ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Creation date of the group in ISO-8601 format ||
|| **DATE_UPDATE**
[`datetime`](../data-types.md) | Modification date of the group in ISO-8601 format ||
|| **DATE_ACTIVITY**
[`datetime`](../data-types.md) | Date of last activity in the group in ISO-8601 format ||
|| **IS_EXTRANET**
[`string`](../data-types.md) | Filter by the type of the group's site.

Possible values:
- `Y` — extranet groups
- `N` — non-extranet groups ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ORDER":{"NAME":"ASC"},"FILTER":{"%NAME":"Pro"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sonet_group.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ORDER":{"NAME":"ASC"},"FILTER":{"%NAME":"Pro"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sonet_group.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'sonet_group.get',
            {
                ORDER: { NAME: 'ASC' },
                FILTER: { '%NAME': 'Pro' }
            }
        );
        
        const result = response.getData().result;
        console.log('Retrieved groups:', result);
        
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
                'sonet_group.get',
                [
                    'ORDER' => ['NAME' => 'ASC'],
                    'FILTER' => ['%NAME' => 'Pro']
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving groups: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sonet_group.get', {
        ORDER: { NAME: 'ASC' },
        FILTER: { '%NAME': 'Pro' }
    }, function(result) {
        if (result.error())
        {
            console.error(result.error(), result.error_description());
        }
        else
        {
            console.log(result.data());
        }
    });
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sonet_group.get',
        [
            'ORDER' => ['NAME' => 'ASC'],
            'FILTER' => ['%NAME' => 'Pro']
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
    "result": [
        {
        "ID": "77",
        "SITE_ID": "s1",
        "NAME": "New Project Title",
        "DESCRIPTION": null,
        "DATE_CREATE": "2026-03-19T15:01:27+02:00",
        "DATE_UPDATE": "2026-03-19T15:01:27+02:00",
        "ACTIVE": "Y",
        "VISIBLE": "Y",
        "OPENED": "N",
        "CLOSED": "N",
        "SUBJECT_ID": "1",
        "OWNER_ID": "1271",
        "KEYWORDS": null,
        "NUMBER_OF_MEMBERS": "12",
        "DATE_ACTIVITY": "2026-03-19T15:01:27+02:00",
        "SUBJECT_NAME": "Workgroups",
        "PROJECT": "Y",
        "IS_EXTRANET": "N"
        },
        {
        "ID": "79",
        "SITE_ID": "s1",
        "NAME": "Scrum Project",
        "DESCRIPTION": null,
        "DATE_CREATE": "2026-03-19T15:15:06+02:00",
        "DATE_UPDATE": "2026-03-19T15:15:06+02:00",
        "ACTIVE": "Y",
        "VISIBLE": "Y",
        "OPENED": "N",
        "CLOSED": "N",
        "SUBJECT_ID": "1",
        "OWNER_ID": "1269",
        "KEYWORDS": null,
        "NUMBER_OF_MEMBERS": "8",
        "DATE_ACTIVITY": "2026-03-19T15:15:06+02:00",
        "SUBJECT_NAME": "Workgroups",
        "PROJECT": "Y",
        "IS_EXTRANET": "N"
        }
    ],
    "total": 2,
    "time": {
        "start": 1773925430,
        "finish": 1773925430.419962,
        "duration": 0.41996192932128906,
        "processing": 0,
        "date_start": "2026-03-19T16:03:50+02:00",
        "date_finish": "2026-03-19T16:03:50+02:00",
        "operating_reset_at": 1773926030,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Array of groups and projects that match the `FILTER` conditions.

An empty array means that there are no suitable records considering the access permissions of the current user ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the group ||
|| **SITE_ID**
[`string`](../data-types.md) | Identifier of the group's site ||
|| **NAME**
[`string`](../data-types.md) | Name of the group ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the group ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Creation date of the group in ISO-8601 format ||
|| **DATE_UPDATE**
[`datetime`](../data-types.md) | Modification date of the group in ISO-8601 format ||
|| **DATE_ACTIVITY**
[`datetime`](../data-types.md) | Date of last activity in ISO-8601 format ||
|| **ACTIVE**
[`string`](../data-types.md) | Activity status ||
|| **VISIBLE**
[`string`](../data-types.md) | Visibility of the group ||
|| **OPENED**
[`string`](../data-types.md) | Is the group open ||
|| **CLOSED**
[`string`](../data-types.md) | Is the group archived ||
|| **SUBJECT_ID**
[`integer`](../data-types.md) | Identifier of the group's subject ||
|| **OWNER_ID**
[`integer`](../data-types.md) | Identifier of the owner ||
|| **KEYWORDS**
[`string`](../data-types.md) | Keywords of the group ||
|| **NUMBER_OF_MEMBERS**
[`integer`](../data-types.md) | Number of members ||
|| **SUBJECT_NAME**
[`string`](../data-types.md) | Name of the group's subject ||
|| **IMAGE**
[`string`](../data-types.md) | URL of the group's avatar ||
|| **IS_EXTRANET**
[`string`](../data-types.md) | Indicator of the extranet group ||
|| **total**
[`integer`](../data-types.md) | Total number of items in the selection ||
|| **next**
[`integer`](../data-types.md) | Offset for the next page (if any) ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./socialnetwork-api-workgroup-get.md)
- [{#T}](./socialnetwork-api-workgroup-list.md)
- [{#T}](./sonet-group-user-groups.md)
- [{#T}](./sonet-group-feature-access.md)
- [{#T}](./sonet-group-delete.md)