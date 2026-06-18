# Create Department humanresources.node.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Add new departments" or "Add new teams" permission.

The method `humanresources.node.add` creates a new department or team.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../../../data-types.md) | The type of the structure element being created.

Possible values:

- `DEPARTMENT` — department
- `TEAM` — team ||
|| **name***
[`string`](../../../data-types.md) | The name of the department or team ||
|| **parentId***
[`integer`](../../../data-types.md) | The identifier of the parent department or team.

The identifier can be obtained using the [humanresources.node.list](./humanresources-node-list.md) method. ||
|| **description**
[`string`](../../../data-types.md) | Description of the department or team ||
|| **colorName**
[`string`](../../../data-types.md) | The color of the team.

Possible values:

- `blue` — blue
- `green` — green
- `cyan` — cyan
- `orange` — orange
- `purple` — purple
- `pink` — pink ||
|| **userIds**
[`object`](../../../data-types.md) | Users to be added to the department or team. [Description of the object structure](#userids)

User identifiers can be obtained using the [user.get](../../../user/user-get.md) method. ||
|| **moveUsersToNode**
[`boolean`](../../../data-types.md) | Determines whether to transfer users from `userIds` to the new department.

Possible values:

- `true` — users will be removed from the previous department and moved to the new department
- `false` — users will only be added to the new department

Default: `false`

Use if `type` is `DEPARTMENT`. ||
|| **createChat**
[`boolean`](../../../data-types.md) | Determines whether to create a new chat for the department.

Possible values:

- `true` — create a chat
- `false` — do not create a chat

Default: `false` ||
|| **bindingChatIds**
[`array`](../../../data-types.md) | An array of identifiers of existing chats to be linked to the department.

Chat identifiers can be obtained using the [im.recent.list](../../../chats/im-recent-list.md) method. ||
|| **createChannel**
[`boolean`](../../../data-types.md) | Determines whether to create a new channel for the department.

Possible values:

- `true` — create a channel
- `false` — do not create a channel

Default: `false` ||
|| **bindingChannelIds**
[`array`](../../../data-types.md) | An array of identifiers of existing channels to be linked to the department.

Channel identifiers can be obtained using the [im.recent.list](../../../chats/im-recent-list.md) method. ||
|| **createCollab**
[`boolean`](../../../data-types.md) | Determines whether to create a new collaboration for the department if collaborations are available in the plan.

Possible values:

- `true` — create a collaboration
- `false` — do not create a collaboration

Default: `false` ||
|| **bindingCollabIds**
[`array`](../../../data-types.md) | An array of identifiers of existing collaborations to be linked to the department.

Collaboration identifiers can be obtained using the [socialnetwork.api.workgroup.list](../../../sonet-group/socialnetwork-api-workgroup-list.md) method. ||
|| **settings**
[`object`](../../../data-types.md) | Additional settings for the department or team. [Description of the object structure](#settings) ||
|#

### Parameter userIds {#userids}

#|
|| **Name**
`type` | **Description** ||
|| **MEMBER_HEAD**
[`array`](../../../data-types.md) | Identifiers of department heads ||
|| **MEMBER_DEPUTY_HEAD**
[`array`](../../../data-types.md) | Identifiers of deputy heads of the department ||
|| **MEMBER_EMPLOYEE**
[`array`](../../../data-types.md) | Identifiers of department employees ||
|| **MEMBER_TEAM_HEAD**
[`array`](../../../data-types.md) | Identifiers of team leaders ||
|| **MEMBER_TEAM_DEPUTY_HEAD**
[`array`](../../../data-types.md) | Identifiers of deputy team leaders ||
|| **MEMBER_TEAM_EMPLOYEE**
[`array`](../../../data-types.md) | Identifiers of team members ||
|#

### Parameter settings {#settings}

#|
|| **Name**
`type` | **Description** ||
|| **BUSINESS_PROC_AUTHORITY**
[`array`](../../../data-types.md) | Roles allowed to work with the department or team workflows.

Possible values:

- `HEAD` — department head
- `DEPUTY_HEAD` — deputy department head
- `ALL_DEPARTMENT_HEADS` — all department heads
- `EMPLOYEE` — department employee
- `TEAM_HEAD` — team leader
- `TEAM_DEPUTY` — deputy team leader
- `TEAM_EMPLOYEE` — team member ||
|| **REPORTS_AUTHORITY**
[`array`](../../../data-types.md) | Roles allowed to work with the department or team reports.

Possible values:

- `HEAD` — department head
- `DEPUTY_HEAD` — deputy department head
- `ALL_DEPARTMENT_HEADS` — all department heads
- `EMPLOYEE` — department employee
- `TEAM_HEAD` — team leader
- `TEAM_DEPUTY` — deputy team leader
- `TEAM_EMPLOYEE` — team member ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"DEPARTMENT","name":"Marketing Department","parentId":1,"description":"Responsible for promotion","userIds":{"MEMBER_HEAD":[7],"MEMBER_EMPLOYEE":[12,15]},"moveUsersToNode":true,"createChat":true,"bindingChatIds":[31],"createChannel":false,"createCollab":false,"settings":{"BUSINESS_PROC_AUTHORITY":["HEAD","DEPUTY_HEAD"],"REPORTS_AUTHORITY":["HEAD"]}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"DEPARTMENT","name":"Marketing Department","parentId":1,"description":"Responsible for promotion","userIds":{"MEMBER_HEAD":[7],"MEMBER_EMPLOYEE":[12,15]},"moveUsersToNode":true,"createChat":true,"bindingChatIds":[31],"createChannel":false,"createCollab":false,"settings":{"BUSINESS_PROC_AUTHORITY":["HEAD","DEPUTY_HEAD"],"REPORTS_AUTHORITY":["HEAD"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NodeAddResult = {
      id: number
      name: string
      type: string
      structureId: number
      parentId: number
      description: string | null
      accessCode: string
      userCount: number
      colorName: string | null
      xmlId: string | null
      createdAt: ISODate
      updatedAt: ISODate
      members: Array<{
        userId: number
        name: string
        workPosition: string
        role: string
        avatar: string | null
        url: string
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<NodeAddResult>({
        method: 'humanresources.node.add',
        params: {
          type: 'DEPARTMENT',
          name: 'Marketing department',
          parentId: 1,
          description: 'Handles promotion',
          userIds: {
            MEMBER_HEAD: [7],
            MEMBER_EMPLOYEE: [12, 15],
          },
          moveUsersToNode: true,
          createChat: true,
          bindingChatIds: [31],
          settings: {
            BUSINESS_PROC_AUTHORITY: ['HEAD', 'DEPUTY_HEAD'],
            REPORTS_AUTHORITY: ['HEAD'],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Node created:', result.id, result.name, result.type)
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
      async function addNode() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'humanresources.node.add',
            params: {
              type: 'DEPARTMENT',
              name: 'Marketing department',
              parentId: 1,
              description: 'Handles promotion',
              userIds: {
                MEMBER_HEAD: [7],
                MEMBER_EMPLOYEE: [12, 15],
              },
              moveUsersToNode: true,
              createChat: true,
              bindingChatIds: [31],
              settings: {
                BUSINESS_PROC_AUTHORITY: ['HEAD', 'DEPUTY_HEAD'],
                REPORTS_AUTHORITY: ['HEAD'],
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Node created:', result.id, result.name, result.type)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addNode)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.add',
                [
                    'type' => 'DEPARTMENT',
                    'name' => 'Marketing Department',
                    'parentId' => 1,
                    'description' => 'Responsible for promotion',
                    'userIds' => [
                        'MEMBER_HEAD' => [7],
                        'MEMBER_EMPLOYEE' => [12, 15],
                    ],
                    'moveUsersToNode' => true,
                    'createChat' => true,
                    'bindingChatIds' => [31],
                    'settings' => [
                        'BUSINESS_PROC_AUTHORITY' => ['HEAD', 'DEPUTY_HEAD'],
                        'REPORTS_AUTHORITY' => ['HEAD'],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating department: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'humanresources.node.add',
        {
            type: 'DEPARTMENT',
            name: 'Marketing Department',
            parentId: 1,
            description: 'Responsible for promotion',
            userIds: {
                MEMBER_HEAD: [7],
                MEMBER_EMPLOYEE: [12, 15]
            },
            moveUsersToNode: true,
            createChat: true,
            bindingChatIds: [31],
            settings: {
                BUSINESS_PROC_AUTHORITY: ['HEAD', 'DEPUTY_HEAD'],
                REPORTS_AUTHORITY: ['HEAD']
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.node.add',
        [
            'type' => 'DEPARTMENT',
            'name' => 'Marketing Department',
            'parentId' => 1,
            'description' => 'Responsible for promotion',
            'userIds' => [
                'MEMBER_HEAD' => [7],
                'MEMBER_EMPLOYEE' => [12, 15]
            ],
            'moveUsersToNode' => true,
            'createChat' => true,
            'bindingChatIds' => [31],
            'settings' => [
                'BUSINESS_PROC_AUTHORITY' => ['HEAD', 'DEPUTY_HEAD'],
                'REPORTS_AUTHORITY' => ['HEAD']
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "id": 44,
        "name": "Marketing Department",
        "type": "DEPARTMENT",
        "structureId": 1,
        "parentId": 1,
        "description": "Responsible for promotion",
        "accessCode": "DR44",
        "userCount": 3,
        "colorName": null,
        "xmlId": null,
        "createdAt": "2026-06-02T11:15:20+02:00",
        "updatedAt": "2026-06-02T11:15:20+02:00",
        "members": [
            {
                "userId": 7,
                "name": "Anna Smith",
                "workPosition": "Head of Marketing Department",
                "role": "MEMBER_HEAD",
                "avatar": "https://example.bitrix24.com/upload/main/1/avatar.jpg",
                "url": "/company/personal/user/7/"
            },
            {
                "userId": 12,
                "name": "John Doe",
                "workPosition": "Marketer",
                "role": "MEMBER_EMPLOYEE",
                "avatar": null,
                "url": "/company/personal/user/12/"
            }
        ]
    },
    "time": {
        "start": 1780388120,
        "finish": 1780388120.645321,
        "duration": 0.6453211307525635,
        "processing": 0.6032140254974365,
        "date_start": "2026-06-02T11:15:20+02:00",
        "date_finish": "2026-06-02T11:15:20+02:00",
        "operating_reset_at": 1780388720,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing data of the created structure element ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the created department or team ||
|| **name**
[`string`](../../../data-types.md) | Name of the department or team ||
|| **type**
[`string`](../../../data-types.md) | Type of the structure element ||
|| **structureId**
[`integer`](../../../data-types.md) | Identifier of the company structure ||
|| **parentId**
[`integer`](../../../data-types.md) | Identifier of the parent department or team ||
|| **description**
[`string`](../../../data-types.md) | Description of the department or team ||
|| **accessCode**
[`string`](../../../data-types.md) | Access code of the structure element ||
|| **userCount**
[`integer`](../../../data-types.md) | Number of users in the department or team ||
|| **colorName**
[`string`](../../../data-types.md) | Team color, if specified ||
|| **xmlId**
[`string`](../../../data-types.md) | External identifier of the structure element ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Date and time of creation of the structure element ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Date and time of the last update of the structure element ||
|| **members**
[`array`](../../../data-types.md) | List of users added to the department or team with roles ||
|| **members[]**
[`object`](../../../data-types.md) | User object of the department or team ||
|| **userId**
[`integer`](../../../data-types.md) | User identifier ||
|| **name**
[`string`](../../../data-types.md) | User name ||
|| **workPosition**
[`string`](../../../data-types.md) | User position ||
|| **role**
[`string`](../../../data-types.md) | User role in the department or team ||
|| **avatar**
[`string`](../../../data-types.md) | Link to the user's avatar ||
|| **url**
[`string`](../../../data-types.md) | Link to the user's profile ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating the request object",
        "validation": [
            {
                "message": "Required field `name` is not specified",
                "field": "name"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `type`
`name`
`parentId` | Required field `#FIELD#` is not specified | Add the specified field to the request body ||
|| `#FIELD#` | Field `#FIELD#` requires data type `#TYPE#` for this request | Ensure the value passed is of the correct type ||
|| `type` | An invalid value for the structure element type was provided | Use `DEPARTMENT` for a department or `TEAM` for a team ||
|| `-` | Default company structure not found | Ensure the company structure is created and accessible ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | The user does not have permission to create a structure element in the specified parent department or team ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient permissions: required scope is missing | Check the `humanresources` scope ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-get.md)
- [{#T}](./humanresources-node-edit.md)
- [{#T}](./humanresources-node-list.md)
- [{#T}](./index.md)
