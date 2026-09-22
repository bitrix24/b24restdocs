# Get a List of Workgroups socialnetwork.api.workgroup.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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

If the parameter is not passed or is empty, only `ID` is selected. The `ID` field is always returned even if it is not included in `select`. Unknown fields are ignored ||
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
[`boolean`](../data-types.md) | Group activity status: `Y` or `N` ||
|| **VISIBLE**
[`boolean`](../data-types.md) | Group visibility in the general list: `Y` or `N` ||
|| **OPENED**
[`boolean`](../data-types.md) | Is the group open for free membership: `Y` or `N` ||
|| **CLOSED**
[`boolean`](../data-types.md) | Is the group archived: `Y` or `N` ||
|| **PROJECT**
[`boolean`](../data-types.md) | Object type: `Y` — project, `N` — group ||
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
[`boolean`](../data-types.md) | Group activity status: `Y` or `N` ||
|| **SUBJECT_ID**
[`integer`](../data-types.md) | Group subject identifier ||
|| **NAME**
[`string`](../data-types.md) | Group name ||
|| **DESCRIPTION**
[`text`](../data-types.md) | Group description ||
|| **KEYWORDS**
[`string`](../data-types.md) | Group keywords ||
|| **CLOSED**
[`boolean`](../data-types.md) | Archive group status: `Y` or `N` ||
|| **VISIBLE**
[`boolean`](../data-types.md) | Group visibility status: `Y` or `N` ||
|| **OPENED**
[`boolean`](../data-types.md) | Open group status: `Y` or `N` ||
|| **PROJECT**
[`boolean`](../data-types.md) | Project status: `Y` or `N` ||
|| **LANDING**
[`boolean`](../data-types.md) | Group for publication status: `Y` or `N` ||
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
[`enum`](../data-types.md) | Who can invite participants:

- `A` — group owner only
- `E` — owner and moderators
- `K` — all members ||
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
[`enum`](../data-types.md) | Default assignee in Scrum:

- `A` — task creator
- `M` — Scrum master ||
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
[`string[]`](../data-types.md) | List of group tool codes to consider when forming `additionalData` in `mobile` mode ||
|| **mandatoryFeatures**
[`string[]`](../data-types.md) | Tool codes from `features` to include in `additionalData.features` regardless of the current user's permissions ||
|| **shouldSelectHasCollabers**
[`boolean`](../data-types.md) | Whether to add the `hasCollabers` external member indicator to `additionalData`.

Possible values:
- `true` or `Y` — add the indicator
- `false` or `N` — do not add the indicator

Default — `false` ||
|| **shouldEnsureHasCollabers**
[`boolean`](../data-types.md) | Whether to recalculate `hasCollabers` before returning the response.

The parameter is used only if `shouldSelectHasCollabers` is `true` or `Y`.

Possible values:
- `true` or `Y` — recalculate the indicator
- `false` or `N` — return the saved value

Default — `false` ||
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type WorkgroupListResult = {
      workgroups: Workgroup[]
    }

    type Workgroup = {
      id: number
      name: string
      type: 'group' | 'project' | 'scrum' | 'collab' | null
      imageId: number
      avatarType: string | null
      avatar: string
      additionalData: {
        role: string
        initiatedByType: string
        features?: string[]
        hasCollabers?: boolean
      }
      dialogId: string
    }

    try {
      // socialnetwork.api.workgroup.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<WorkgroupListResult>({
        method: 'socialnetwork.api.workgroup.list',
        params: {
          filter: { ACTIVE: 'Y', CLOSED: 'N', '%NAME': 'group' },
          select: ['ID', 'NAME', 'TYPE', 'AVATAR'],
          order: { ID: 'DESC' },
          params: { mode: 'mobile', shouldSelectDialogId: 'Y' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Workgroups:', result.workgroups.length, result.workgroups)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function fetchWorkgroupList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // socialnetwork.api.workgroup.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'socialnetwork.api.workgroup.list',
            params: {
              filter: { ACTIVE: 'Y', CLOSED: 'N', '%NAME': 'group' },
              select: ['ID', 'NAME', 'TYPE', 'AVATAR'],
              order: { ID: 'DESC' },
              params: { mode: 'mobile', shouldSelectDialogId: 'Y' },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Workgroups:', result.workgroups.length, result.workgroups)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchWorkgroupList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.socialnetwork.api.workgroup.list(
            filter={
                "ACTIVE": "Y",
                "CLOSED": "N",
                "%NAME": "group",
            },
            select=[
                "ID",
                "NAME",
                "TYPE",
                "AVATAR",
            ],
            order={
                "ID": "DESC",
            },
            params={
                "mode": "mobile",
                "shouldSelectDialogId": "Y",
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    Example `as_list`

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.socialnetwork.api.workgroup.list(
            filter={
                "ACTIVE": "Y",
                "CLOSED": "N",
                "%NAME": "group",
            },
            select=[
                "ID",
                "NAME",
                "TYPE",
                "AVATAR",
            ],
            order={
                "ID": "DESC",
            },
            params={
                "mode": "mobile",
                "shouldSelectDialogId": "Y",
            },
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    Example `as_list_fast`

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.socialnetwork.api.workgroup.list(
            filter={
                "ACTIVE": "Y",
                "CLOSED": "N",
                "%NAME": "group",
            },
            select=[
                "ID",
                "NAME",
                "TYPE",
                "AVATAR",
            ],
            order={
                "ID": "DESC",
            },
            params={
                "mode": "mobile",
                "shouldSelectDialogId": "Y",
            },
        ).as_list_fast(descending=True).response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "socialnetwork.api.workgroup.list", b24.Params{
    	"filter": b24.Params{
    		"ACTIVE": "Y",
    		"CLOSED": "N",
    		"%NAME":  "group",
    	},
    	"select": []string{"ID", "NAME", "TYPE", "AVATAR"},
    	"order": b24.Params{
    		"ID": "DESC",
    	},
    	"params": b24.Params{
    		"mode":                 "mobile",
    		"shouldSelectDialogId": "Y",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("socialnetwork.api.workgroup.list: %w", err)
    }

    // The method wraps the response in an object with the "workgroups" key.
    raw, ok := b24.Unwrap(res.Result, "workgroups")
    if !ok {
    	return fmt.Errorf("no workgroups key in the response")
    }

    var items []struct {
    	ID       b24.ID `json:"id"`
    	Name     string `json:"name"`
    	Type     string `json:"type"`
    	ImageID  b24.ID `json:"imageId"`
    	Avatar   string `json:"avatar"`
    	DialogID string `json:"dialogId"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "workgroups": [
            {
                "id": 5,
                "name": "Open group for everyone",
                "type": "group",
                "imageId": 5,
                "avatarType": null,
                "avatar": "https://test.bitrix24.com/b13743910/resize_cache/5/7acf4caaf5d8/socialnetwork/8d6/8d2c04ece929572/3.png",
                "additionalData": {
                    "role": "",
                    "initiatedByType": ""
                },
                "dialogId": ""
            },
            {
                "id": 1,
                "name": "Closed visible group",
                "type": "group",
                "imageId": 1,
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
[`object[]`](../data-types.md) | List of workgroups with [field descriptions](#workgroup-fields).

The structure of the object depends on the fields passed in `select` and the parameters in `params`.

If no groups are found by the filter, `workgroups` will return an empty array. ||
|| **next**
[`integer`](../data-types.md) | Offset for the next page. The field is returned if there are more records. ||
|| **total**
[`integer`](../data-types.md) | Total number of records. The field is not returned if the request is executed with `start = -1`. ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request. ||
|#

### workgroup Object Fields {#workgroup-fields}

Fields are passed in `select` in `UPPER_SNAKE_CASE` and returned in the response in `camelCase`. For example, `DATE_CREATE` corresponds to `dateCreate`, and `NUMBER_OF_MEMBERS` corresponds to `numberOfMembers`.

#|
|| **Response Field**
`type` | **select Field or Return Condition** | **Description** ||
|| **id**
[`integer`](../data-types.md) | `ID` | Group identifier. Always returned ||
|| **active**
[`boolean`](../data-types.md) | `ACTIVE` | Group activity status: `Y` or `N` ||
|| **subjectId**
[`integer`](../data-types.md) | `SUBJECT_ID` | Group subject identifier ||
|| **name**
[`string`](../data-types.md) | `NAME` | Group name ||
|| **description**
[`text`](../data-types.md) | `DESCRIPTION` | Group description ||
|| **keywords**
[`string`](../data-types.md) | `KEYWORDS` | Group keywords ||
|| **closed**
[`boolean`](../data-types.md) | `CLOSED` | Archive group status: `Y` or `N` ||
|| **visible**
[`boolean`](../data-types.md) | `VISIBLE` | Group visibility status: `Y` or `N` ||
|| **opened**
[`boolean`](../data-types.md) | `OPENED` | Open group status: `Y` or `N` ||
|| **project**
[`boolean`](../data-types.md) | `PROJECT` | Project status: `Y` or `N` ||
|| **landing**
[`boolean`](../data-types.md) | `LANDING` | Group for publication status: `Y` or `N` ||
|| **dateCreate**
[`datetime`](../data-types.md) | `DATE_CREATE` | Group creation date ||
|| **dateUpdate**
[`datetime`](../data-types.md) | `DATE_UPDATE` | Group modification date ||
|| **dateActivity**
[`datetime`](../data-types.md) | `DATE_ACTIVITY` | Last activity date ||
|| **imageId**
[`integer`](../data-types.md) | `IMAGE_ID` or `AVATAR` | Custom avatar identifier ||
|| **avatarType**
[`string`](../data-types.md) \| `null` | `AVATAR_TYPE` or `AVATAR` | System avatar type ||
|| **avatar**
[`string`](../data-types.md) | `AVATAR` | Avatar URL. Returns an empty string if no avatar is set ||
|| **ownerId**
[`integer`](../data-types.md) | `OWNER_ID` | Owner identifier ||
|| **numberOfMembers**
[`integer`](../data-types.md) | `NUMBER_OF_MEMBERS` | Number of members ||
|| **numberOfModerators**
[`integer`](../data-types.md) | `NUMBER_OF_MODERATORS` | Number of moderators ||
|| **initiatePerms**
[`enum`](../data-types.md) | `INITIATE_PERMS` | Who can invite members: `A` — owner, `E` — owner and moderators, `K` — all members ||
|| **projectDateStart**
[`datetime`](../data-types.md) \| `null` | `PROJECT_DATE_START` | Project start date ||
|| **projectDateFinish**
[`datetime`](../data-types.md) \| `null` | `PROJECT_DATE_FINISH` | Project end date ||
|| **scrumOwnerId**
[`integer`](../data-types.md) | `SCRUM_OWNER_ID` | Scrum owner identifier ||
|| **scrumMasterId**
[`integer`](../data-types.md) | `SCRUM_MASTER_ID` | Scrum master identifier ||
|| **scrumSprintDuration**
[`integer`](../data-types.md) | `SCRUM_SPRINT_DURATION` | Sprint duration in seconds ||
|| **scrumTaskResponsible**
[`enum`](../data-types.md) | `SCRUM_TASK_RESPONSIBLE` | Default assignee: `A` — task creator, `M` — Scrum master ||
|| **type**
[`string`](../data-types.md) \| `null` | `TYPE` | Group type: `group`, `project`, `scrum`, `collab` ||
|| **additionalData**
[`object`](../data-types.md) | `params[mode] = mobile` | Additional group and current user data [(detailed description)](#additional-data) ||
|| **dialogId**
[`string`](../data-types.md) | `params[shouldSelectDialogId] = Y` | Group chat identifier. Returns an empty string if the chat is not found ||
|#

#### additionalData Object {#additional-data}

#|
|| **Name**
`type` | **Description** ||
|| **role**
[`string`](../data-types.md) | Current user's role in the group. Returns an empty string if the user is not associated with the group ||
|| **initiatedByType**
[`string`](../data-types.md) | Who initiated the user's association with the group:

- `U` — user
- `G` — group

Returns an empty string if the user is not associated with the group ||
|| **features**
[`string[]`](../data-types.md) | Available group tool codes. Returned if `features` or `mandatoryFeatures` are passed in `params` ||
|| **hasCollabers**
[`boolean`](../data-types.md) | Whether the group has external members. Returned if `params[shouldSelectHasCollabers]` is `true` or `Y` ||
|#

## Error Handling

HTTP Status: **401**

```json
{
    "error": "insufficient_scope",
    "error_description": "The request requires higher privileges than provided by the webhook token"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `insufficient_scope` | Insufficient token scope | The token does not include the `socialnetwork` scope ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-create.md)
- [{#T}](./sonet-group-update.md)
- [{#T}](./socialnetwork-api-workgroup-get.md)
- [{#T}](./sonet-group-get.md)
- [{#T}](./sonet-group-delete.md)
