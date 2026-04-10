# Retrieve Data on Workgroup socialnetwork.api.workgroup.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`socialnetwork`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `socialnetwork.api.workgroup.get` returns information about a workgroup, project, scrum, or collaboration based on the identifier.

An administrator can retrieve information about any group on the account, even if it is secret and they are not a member.

## Method Parameters

{% include [Footnote on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **params***
[`object`](../data-types.md) | Request parameters for retrieving the group. More details [below](#params) ||
|#

### Parameter params {#params}

{% include [Footnote on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **groupId***
[`integer`](../data-types.md#standart-types) | Group identifier. The value for this field can be obtained using the method [sonet_group.get](./sonet-group-get.md) ||
|| **select**
[`array`](../data-types.md#standart-types) | List of additional fields to retrieve, returned in `result`. More details [below](#paramsselect) ||
|| **mode**
[`string`](../data-types.md#standart-types) | Request mode. Can only take the value `mobile`, which allows retrieving additional data in the array `result[ADDITIONAL_DATA]` ||
|#

#### Parameter select {#paramsselect}

#|
|| **Name**
`type` | **Description** ||
|| **ACTIONS**
[`string`](../data-types.md#standart-types) | Operations available to the current user on the group ||
|| **AVATAR**
[`string`](../data-types.md#standart-types) | URL of the group's compressed user avatar ||
|| **AVATAR_DATA**
[`string`](../data-types.md#standart-types) | Information about the group's avatar ||
|| **AVATAR_TYPES**
[`string`](../data-types.md#standart-types) | Types of avatars for groups ||
|| **COUNTERS**
[`string`](../data-types.md#standart-types) | Number of unaccepted requests and invitations to join the group ||
|| **DATE_CREATE**
[`string`](../data-types.md#standart-types) | Date and time of group creation in a more readable format ||
|| **DEPARTMENTS**
[`string`](../data-types.md#standart-types) | Departments of employees added to the group ||
|| **EFFICIENCY**
[`string`](../data-types.md#standart-types) | Group efficiency ||
|| **FEATURES**
[`string`](../data-types.md#standart-types) | Available tools in the group specified in the advanced group settings ||
|| **GROUP_MEMBERS_LIST**
[`string`](../data-types.md#standart-types) | List of active group members, invited users, and users awaiting confirmation to join the group ||
|| **LIST_OF_MEMBERS**
[`string`](../data-types.md#standart-types) | List of group members with their information ||
|| **LIST_OF_MEMBERS_AWAITING_INVITE**
[`string`](../data-types.md#standart-types) | Users awaiting confirmation to join the group ||
|| **OWNER_DATA**
[`string`](../data-types.md#standart-types) | Data about the group owner ||
|| **PIN**
[`string`](../data-types.md#standart-types) | Whether the group is pinned for the current user on the groups and projects page. Returned as `result[IS_PIN]` ||
|| **PRIVACY_TYPE**
[`string`](../data-types.md#standart-types) | Group privacy level. Returned as `result[PRIVACY_CODE]` ||
|| **SUBJECT_DATA**
[`string`](../data-types.md#standart-types) | Information about the group's subject specified in the advanced group settings ||
|| **TAGS**
[`string`](../data-types.md#standart-types) | Group tags specified in the advanced group settings ||
|| **USER_DATA**
[`string`](../data-types.md#standart-types) | Data about the current user's role in the group ||
|#

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"params":{"groupId":622,"select":["DEPARTMENTS","TAGS"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/socialnetwork.api.workgroup.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"params":{"groupId":622,"select":["DEPARTMENTS","TAGS"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/socialnetwork.api.workgroup.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'socialnetwork.api.workgroup.get',
    		{
    			params: {
    				groupId: 622,
    				select: [ 'DEPARTMENTS', 'TAGS' ],
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
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
                'socialnetwork.api.workgroup.get',
                [
                    'params' => [
                        'groupId' => 622,
                        'select'  => [ 'DEPARTMENTS', 'TAGS' ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting workgroup info: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'socialnetwork.api.workgroup.get', {
        params: {
            groupId: 622,
            select: [ 'DEPARTMENTS', 'TAGS' ],
        },
    }, result => {
        console.log(result);
    });
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'socialnetwork.api.workgroup.get',
        [
            'params' => [
                'groupId' => 622,
                'select' => ['DEPARTMENTS', 'TAGS']
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

{% include [Footnote on examples](../../_includes/examples.md) %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": 622,
        "ACTIVE": "Y",
        "SITE_ID": "s1",
        "SUBJECT_ID": 1,
        "NAME": "Group for Demonstrating the Method",
        "DESCRIPTION": "First line of the group description\r\nSecond line of the group description",
        "KEYWORDS": "group tag, another group tag",
        "CLOSED": "N",
        "VISIBLE": "Y",
        "OPENED": "N",
        "DATE_CREATE": "04/17/2025 19:37:55",
        "DATE_UPDATE": "04/17/2025 19:40:48",
        "DATE_ACTIVITY": "04/17/2025 19:40:48",
        "IMAGE_ID": 0,
        "AVATAR_TYPE": "folder",
        "OWNER_ID": 1,
        "INITIATE_PERMS": "K",
        "NUMBER_OF_MEMBERS": 1,
        "NUMBER_OF_MODERATORS": 1,
        "PROJECT": "N",
        "PROJECT_DATE_START": null,
        "PROJECT_DATE_FINISH": null,
        "SEARCH_INDEX": "Group for Demonstrating the Method First line of the group description\r\nSecond line of the group description group tag #group tag another group tag #another group tag group@example.com",
        "LANDING": "N",
        "SCRUM_OWNER_ID": 0,
        "SCRUM_SPRINT_DURATION": 0,
        "SCRUM_TASK_RESPONSIBLE": "",
        "TYPE": "group",
        "MEMBERS": [
            1,
            10,
            20
        ],
        "CHAT_ID": 1034,
        "DIALOG_ID": "chat1034",
        "ORDINARY_MEMBERS": [
            10
        ],
        "INVITED_MEMBERS": [
            38
        ],
        "MODERATOR_MEMBERS": [
            20
        ],
        "SITE_IDS": [
            "s1"
        ],
        "TAGS": [
            "another group tag",
            "group tag"
        ],
        "DEPARTMENTS": [
            8
        ],
        "NUMBER_OF_MEMBERS_PLURAL": 0
    },
    "time": {
        "start": 1744908074.244266,
        "finish": 1744908074.279072,
        "duration": 0.034806013107299805,
        "processing": 0.010703086853027344,
        "date_start": "2025-04-17T19:41:14+02:00",
        "date_finish": "2025-04-17T19:41:14+02:00",
        "operating_reset_at": 1744908674,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md#standart-types) | Result of the request execution. More details [below](#result) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Key result {#result}
#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md#standart-types) | Group identifier ||
|| **ACTIVE**
[`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` — whether the group is active. The group can be activated or deactivated using the method [sonet_group.update](sonet-group-update.md) ||
|| **SITE_ID**
[`string`](../data-types.md#standart-types) | Identifier of the site to which the group belongs ||
|| **SUBJECT_ID**
[`integer`](../data-types.md#standart-types) | Identifier of the group's subject. The subject of the group is specified in the advanced group settings ||
|| **NAME**
[`string`](../data-types.md#standart-types) | Group name ||
|| **DESCRIPTION**
[`text`](../data-types.md#standart-types) | Group description ||
|| **KEYWORDS**
[`string`](../data-types.md#standart-types) | Group tags separated by commas: `tag1,tag2` ||
|| **CLOSED**
[`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` — whether the group is archived ||
|| **VISIBLE**
[`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` — whether the group is visible in the group list ||
|| **OPENED**
[`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` — whether the group is open ||
|| **DATE_CREATE**
[`string`](../data-types.md#standart-types) | Date of group creation in the format `DD.MM.YYYY hh:mm:ss`. If the `DATE_CREATE` key is requested in `select`, the date is returned in a more readable format, for example:
- `today, 12:16`
- `yesterday, 14:42`
- `April 15, 14:42`, if the group was created this year
- `June 11, 2024 15:08`, if the group was created in a different year
||
|| **DATE_UPDATE**
[`string`](../data-types.md#standart-types) | Date of group update in the format `DD.MM.YYYY hh:mm:ss` ||
|| **DATE_ACTIVITY**
[`string`](../data-types.md#standart-types) | Date of last activity in the group in the format `DD.MM.YYYY hh:mm:ss` ||
|| **IMAGE_ID**
[`integer`](../data-types.md#standart-types) | Identifier of the group's user avatar in the `b_file` table. `0` if a system image is used as the avatar ||
|| **AVATAR_TYPE**
[`string`](../data-types.md#standart-types) | Type of the last set system avatar:
- `folder` — folder avatar
- `checks` — checkbox avatar
- `pie` — pie chart avatar
- `bag` — briefcase avatar
- `members` — silhouettes avatar
- `""` — other system avatar
||
|| **OWNER_ID**
[`user`](../data-types.md#standart-objects) | Identifier of the group owner ||
|| **INITIATE_PERMS**
[`enum`](../data-types.md#standart-types) | Who has the right to invite users to the group:
- `A` — only the group owner
- `E` — the group owner and group moderators
- `K` — all group members
||
|| **NUMBER_OF_MEMBERS**
[`integer`](../data-types.md#standart-types) | Number of group members ||
|| **NUMBER_OF_MODERATORS**
[`integer`](../data-types.md#standart-types) | Number of group moderators ||
|| **PROJECT**
[`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` — whether the group is a project ||
|| **PROJECT_DATE_START**
[`any`](../data-types.md#standart-types) | Project start date in the format `DD.MM.YYYY hh:mm:ss`. `null` if not specified ||
|| **PROJECT_DATE_FINISH**
[`any`](../data-types.md#standart-types) | Project end date in the format `DD.MM.YYYY hh:mm:ss`. `null` if not specified ||
|| **SEARCH_INDEX**
[`string`](../data-types.md#standart-types) | Index, keywords for searching the group ||
|| **LANDING**
[`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` — whether the group is a publication group ||
|| **SCRUM_MASTER_ID**
[`integer`](../data-types.md#standart-objects) | Identifier of the scrum master. `0` if the group is not a scrum ||
|| **SCRUM_SPRINT_DURATION**
[`integer`](../data-types.md#standart-types) | Duration of the scrum sprint in seconds. `0` if the group is not a scrum ||
|| **SCRUM_TASK_RESPONSIBLE**
[`string`](../data-types.md#standart-types) | Default executor when assigning tasks:
- `A` — Creator
- `M` — Scrum master
- `""` — the group is not a scrum
||
|| **TYPE**
[`enum`](../data-types.md#standart-types) | Type of group:
- `group` — group
- `project` — project
- `scrum` — scrum
- `collab` — collaboration
||
|| **MEMBERS**
[`array`](../data-types.md#standart-types) | Identifiers of group members ||
|| **CHAT_ID**
[`integer`](../data-types.md#standart-types) | Identifier of the group chat ||
|| **DIALOG_ID**
[`string`](../data-types.md#standart-types) | Identifier of the group dialog ||
|| **ORDINARY_MEMBERS**
[`array`](../data-types.md#standart-types) | Array of user identifiers in the group who are not owners or moderators ||
|| **INVITED_MEMBERS**
[`array`](../data-types.md#standart-types) | Array of portal user identifiers who have been invited to the group but have not yet accepted ||
|| **MODERATOR_MEMBERS**
[`array`](../data-types.md#standart-types) | Array of group member identifiers with the role of moderator ||
|| **SITE_IDS**
[`array`](../data-types.md#standart-types) | List of site identifiers to which the group belongs ||
|| **AVATAR**
[`string`](../data-types.md#standart-types) | URL of the group's compressed user avatar. `""` if no user avatar is set ||
|| **AVATAR_TYPES**
[`object`](../data-types.md#standart-types) | Object containing group avatars [(detailed description)](#avatartypes) ||
|| **AVATAR_DATA**
[`object`](../data-types.md#standart-types) | Information about the group's avatar [(detailed description)](#avatardata) ||
|| **OWNER_DATA**
[`object`](../data-types.md#standart-types) | Information about the group owner [(detailed description)](#ownerdata) ||
|| **SUBJECT_DATA**
[`object`](../data-types.md#standart-types) | Information about the group's subject specified in the advanced group settings [(detailed description)](#subjectdata) ||
|| **TAGS**
[`array`](../data-types.md#standart-types) | Group tags, similar to `KEYWORDS`, but in array format ||
|| **ACTIONS**
[`object`](../data-types.md#standart-types) | Data about operations available to the current user on the group [(detailed description)](#actions) ||
|| **USER_DATA**
[`object`](../data-types.md#standart-types) | Information about the current user regarding the group [(detailed description)](#userdata) ||
|| **DEPARTMENTS**
[`array`](../data-types.md#standart-types) | Array of identifiers of departments added to the group ||
|| **IS_PIN**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the group is pinned for the current user on the groups and projects page ||
|| **PRIVACY_CODE**
[`string`](../data-types.md#standart-types) | Group privacy level:
- `open` — open group
- `closed` — closed group
- `secret` — secret group
||
|| **LIST_OF_MEMBERS**
[`object`](../data-types.md#standart-types) | Array with information about group users [(detailed description)](#listofmembers) ||
|| **FEATURES**
[`object`](../data-types.md#standart-types) | Array with information about group tools [(detailed description)](#features) ||
|| **LIST_OF_MEMBERS_AWAITING_INVITE**
[`object`](../data-types.md#standart-types) | Information about users awaiting confirmation to join the group [(detailed description)](#listofmembersawaitinginvite) ||
|| **GROUP_MEMBERS_LIST**
[`object`](../data-types.md#standart-types) | Information about users associated with the group [(detailed description)](#groupmemberslist) ||
|| **COUNTERS**
[`object`](../data-types.md#standart-types) | Counters [(detailed description)](#counters) ||
|| **EFFICIENCY**
[`integer`](../data-types.md#standart-types) | Group efficiency ||
|| **ADDITIONAL_DATA**
[`object`](../data-types.md#standart-types) | Additional data for the current user [(detailed description)](#additionaldata) ||
|#

### Object AVATAR_TYPES {#avatartypes}

#|
|| **Name**
`type` | **Description** ||
|| **sort**
[`integer`](../data-types.md#standart-types) | Avatar sorting ||
|| **mobileUrl**
[`string`](../data-types.md#standart-types) | URL for displaying the avatar in the mobile application ||
|| **webCssClass**
[`string`](../data-types.md#standart-types) | CSS class of the avatar ||
|| **entitySelectorUrl**
[`string`](../data-types.md#standart-types) | URL of the avatar for entity selector ||
|#

### Object AVATAR_DATA {#avatardata}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../data-types.md#standart-types) | Type of avatar:
- `icon` — system avatar
- `image` — user image ||
|| **id**
[`string`](../data-types.md#standart-types) | Identifier of the avatar:
- `folder` — folder avatar
- `tasks` — checkbox avatar
- `chart` — pie chart avatar
- `briefcase` — briefcase avatar
- `group` — silhouettes avatar
- `""` — other system avatar
- `URL` — link to the compressed user avatar if set ||
|#

### Object OWNER_DATA {#ownerdata}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`user`](../data-types.md#standart-objects) | Identifier of the group owner ||
|| **PHOTO**
[`string`](../data-types.md#standart-types) | URL of the compressed avatar of the group owner ||
|| **FORMATTED_NAME**
[`string`](../data-types.md#standart-types) | Formatted name of the group owner according to portal settings ||
|#

### Object SUBJECT_DATA {#subjectdata}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md#standart-types) | Identifier of the subject ||
|| **NAME**
[`string`](../data-types.md#standart-types) | Name of the subject ||
|#

### Object ACTIONS {#actions}

#|
|| **Name**
`type` | **Description** ||
|| **EDIT**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether editing the group is available ||
|| **DELETE**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether deleting the group is available ||
|| **INVITE**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether inviting external participants to the group is available ||
|| **JOIN**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user can join the group ||
|| **LEAVE**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user can leave the group ||
|| **FOLLOW**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user can subscribe to group updates ||
|| **PIN**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether pinning the group is available ||
|| **EDIT_FEATURES**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether changing group tools is available ||
|#

### Object USER_DATA {#userdata}

#|
|| **Name**
`type` | **Description** ||
|| **ROLE**
[`any`](../data-types.md#standart-types) | User's role in the group:
- `A` — group owner
- `E` — group moderator
- `K` — group member
- `Z` — awaiting membership
- `false` — value does not exist ||
|| **INITIATED_BY_TYPE**
[`any`](../data-types.md#standart-types) | Who initiated the user's connection to the group:
- `U` — user, e.g., the user sent a request to join the group
- `G` — group, e.g., the user was invited by the group
- `false` — value does not exist ||
|| **IS_SUBSCRIBED**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user is subscribed to group updates ||
|#

### Object LIST_OF_MEMBERS {#listofmembers}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`user`](../data-types.md#standart-objects) | User identifier ||
|| **isOwner**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user is the group owner ||
|| **isModerator**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user is a moderator ||
|| **isScrumMaster**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user is a scrum master (for scrum groups) ||
|| **isAutoMember**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user was added to the group automatically (without invitation) ||
|| **name**
[`string`](../data-types.md#standart-types) | User's first name ||
|| **lastName**
[`string`](../data-types.md#standart-types) | User's last name ||
|| **position**
[`string`](../data-types.md#standart-types) | User's position ||
|| **photo**
[`string`](../data-types.md#standart-types) | URL of the user's compressed avatar ||
|#

### Object FEATURES {#features}

#|
|| **Name**
`type` | **Description** ||
|| **featureName**
[`string`](../data-types.md#standart-types) | Symbolic identifier of the tool ||
|| **name**
[`string`](../data-types.md#standart-types) | Name of the tool ||
|| **customName**
[`string`](../data-types.md#standart-types) | Custom name of the tool in the advanced group settings ||
|| **id**
[`string`](../data-types.md#standart-types) | Numeric identifier of the tool ||
|| **active**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the tool is enabled in the group ||
|#

### Object LIST_OF_MEMBERS_AWAITING_INVITE {#listofmembersawaitinginvite}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`user`](../data-types.md#standart-objects) | User identifier ||
|| **name**
[`string`](../data-types.md#standart-types) | Formatted name according to portal settings ||
|| **photo**
[`string`](../data-types.md#standart-types) | URL of the user's compressed avatar ||
|#

### Object GROUP_MEMBERS_LIST {#groupmemberslist}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`user`](../data-types.md#standart-objects) | Member identifier ||
|| **invited**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user was invited ||
|| **isAwaiting**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user is awaiting acceptance into the group ||
|| **isMember**
[`boolean`](../data-types.md#standart-types) | Value `true`/`false` — whether the user is a member of the group ||
|#

### Object COUNTERS {#counters}

#|
|| **Name**
`type` | **Description** ||
|| **workgroup_requests_out**
[`integer`](../data-types.md#standart-types) | Current number of unaccepted invitations to the group ||
|| **workgroup_requests_in**
[`integer`](../data-types.md#standart-types) | Current number of requests to join the group ||
|#

### Object ADDITIONAL_DATA {#additionaldata}

#|
|| **Name**
`type` | **Description** ||
|| **ROLE**
[`string`](../data-types.md#standart-types) | Current user's role in the group:
- `A` — group owner
- `E` — group moderator
- `K` — group member
- `Z` — awaiting membership
- `""` — user has no relation to the group ||
|| **INITIATED_BY_TYPE**
[`string`](../data-types.md#standart-types) | Who initiated the user's connection to the group:
- `U` — user, e.g., the user sent a request to join the group
- `G` — group, e.g., the user was invited by the group
- `""` — user has no relation to the group ||
|#

## Error Handling

HTTP Code: **400**

```json
{
    "error": "SONET_CONTROLLER_WORKGROUP_EMPTY",
    "error_description": "No value for the workgroup ID was provided."
}
```
{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `SONET_CONTROLLER_WORKGROUP_EMPTY` | `No value for the workgroup ID was provided.` | The `groupId` parameter was not passed in the `params` array ||
|| `SONET_CONTROLLER_WORKGROUP_NOT_FOUND` | `Workgroup not found.` | The group with identifier `params[groupId]` was not found or the current user does not have access to it ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sonet-group-create.md)
- [{#T}](./sonet-group-update.md)
- [{#T}](./socialnetwork-api-workgroup-list.md)
- [{#T}](./sonet-group-get.md)
- [{#T}](./sonet-group-delete.md)