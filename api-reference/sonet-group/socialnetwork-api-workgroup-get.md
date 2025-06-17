# Get data about the workgroup socialnetwork.api.workgroup.get

> Scope: [`socialnetwork`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `socialnetwork.api.workgroup.get` returns information about a workgroup, project, scrum, or collaboration by its identifier. An administrator can retrieve information about any group on the account, even if it is secret and they are not a member.

## Method parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name** `type` | **Description** ||
|| **params*** [`object`](../data-types.md) | Request parameters to get the group ||
|#

### Parameter params

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name** `type` | **Description** ||
|| **groupId*** [`integer`](../data-types.md#standart-types) | Group identifier. The value for this field can be obtained using the method [sonet_group.get](./sonet-group-get.md) ||
|| **select** [`array`](../data-types.md#standart-types) | List of additional fields to retrieve, returned in `result` ||
|| **mode** [`string`](../data-types.md#standart-types) | Request mode. Can only take the value `mobile`, which allows obtaining additional data in the `result[ADDITIONAL_DATA]` array ||
|#

#### Parameter params[select] {#paramsselect}

#|
|| **ACTIONS** | Available operations for the current user on the group ||
|| **AVATAR** | URL of the group's compressed user avatar ||
|| **AVATAR_DATA** | Information about the group's avatar ||
|| **AVATAR_TYPES** | Types of avatars for groups ||
|| **COUNTERS** | Number of unaccepted requests and invitations to join the group ||
|| **DATE_CREATE** | Date and time of group creation in a more readable format ||
|| **DEPARTMENTS** | Departments of employees added to the group ||
|| **EFFICIENCY** | Group efficiency ||
|| **FEATURES** | Available tools in the group specified in the advanced group settings ||
|| **GROUP_MEMBERS_LIST** | List of active group members, invited users, and users awaiting confirmation to join the group ||
|| **LIST_OF_MEMBERS** | List of group members with information about them ||
|| **LIST_OF_MEMBERS_AWAITING_INVITE** | Users awaiting confirmation to join the group ||
|| **OWNER_DATA** | Data about the group owner ||
|| **PIN** | Whether the group is pinned by the current user on the groups and projects page. Returned as `result[IS_PIN]` ||
|| **PRIVACY_TYPE** | Level of privacy of the group. Returned as `result[PRIVACY_CODE]` ||
|| **SUBJECT_DATA** | Information about the group's subject specified in the advanced group settings ||
|| **TAGS** | Group tags specified in the advanced group settings ||
|| **USER_DATA** | Data about the current user's role in the group ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod('socialnetwork.api.workgroup.get', {
        params: {
            groupId: 622,
            select: [ 'DEPARTMENTS', 'TAGS' ],
        },
    }, result => {
        console.log(result);
    });
    ```

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

- PHP

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

{% include [Note on examples](../../_includes/examples.md) %}

## Response handling

HTTP status: **200**

```json
{
    "result": {
        "ID": 622,
        "ACTIVE": "Y",
        "SITE_ID": "s1",
        "SUBJECT_ID": 1,
        "NAME": "Group for demonstrating the method",
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
        "SEARCH_INDEX": "Group for demonstrating the method First line of the group description\r\nSecond line of the group description group tag #group tag another group tag #another group tag group@example.com",
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

### Returned data
#|
|| **Name** `type` | **Description** ||
|| **result** [`object`](../data-types.md#standart-types) | Result of the request execution ||
|| **time** [`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Key result
#|
|| **Name** `type` | **Description** ||
|| **ID** [`integer`](../data-types.md#standart-types) | Group identifier ||
|| **ACTIVE** [`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` - whether the group is active. You can activate or deactivate the group using the method [sonet_group.update](sonet-group-update.md) ||
|| **SITE_ID** [`string`](../data-types.md#standart-types) | Identifier of the site to which the group belongs ||
|| **SUBJECT_ID** [`integer`](../data-types.md#standart-types) | Identifier of the group's subject. The subject of the group is specified in the advanced group settings ||
|| **NAME** [`string`](../data-types.md#standart-types) | Group name ||
|| **DESCRIPTION** [`text`](../data-types.md#standart-types) | Group description ||
|| **KEYWORDS** [`string`](../data-types.md#standart-types) | Group tags separated by commas: `tag1,tag2` ||
|| **CLOSED** [`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` - whether the group is archived ||
|| **VISIBLE** [`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` - whether the group is visible in the group list ||
|| **OPENED** [`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` - whether the group is open ||
|| **DATE_CREATE** [`string`](../data-types.md#standart-types) | Date of group creation in the format `DD.MM.YYYY hh:mm:ss`. If the key `DATE_CREATE` is requested in `select`, the date is returned in a more readable format, for example:
- `today, 12:16`
- `yesterday, 14:42`
- `April 15 14:42`, if the group was created this year
- `June 11, 2024 15:08`, if the group was created in a different year
||
|| **DATE_UPDATE** [`string`](../data-types.md#standart-types) | Date of group update in the format `DD.MM.YYYY hh:mm:ss` ||
|| **DATE_ACTIVITY** [`string`](../data-types.md#standart-types) | Date of last activity in the group in the format `DD.MM.YYYY hh:mm:ss` ||
|| **IMAGE_ID** [`integer`](../data-types.md#standart-types) | Identifier of the group's user avatar in the `b_file` table. `0` if a system image is used as the avatar ||
|| **AVATAR_TYPE** [`string`](../data-types.md#standart-types) | Type of the last set system avatar:
- `folder` - folder avatar
- `checks` - checkbox avatar
- `pie` - pie chart avatar
- `bag` - briefcase avatar
- `members` - silhouettes avatar
- `""` - other system avatar
||
|| **OWNER_ID** [`user`](../data-types.md#standart-objects) | Identifier of the group owner ||
|| **INITIATE_PERMS** [`enum`](../data-types.md#standart-types) | Who has the right to invite users to the group:
- `A` - only the group owner
- `E` - group owner and group moderators
- `K` - all group members
||
|| **NUMBER_OF_MEMBERS** [`integer`](../data-types.md#standart-types) | Number of group members ||
|| **NUMBER_OF_MODERATORS** [`integer`](../data-types.md#standart-types) | Number of group moderators ||
|| **PROJECT** [`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` - whether the group is a project ||
|| **PROJECT_DATE_START** [`any`](../data-types.md#standart-types) | Project start date in the format `DD.MM.YYYY hh:mm:ss`. `null` if not specified ||
|| **PROJECT_DATE_FINISH** [`any`](../data-types.md#standart-types) | Project end date in the format `DD.MM.YYYY hh:mm:ss`. `null` if not specified ||
|| **SEARCH_INDEX** [`string`](../data-types.md#standart-types) | Index, keywords for searching the group ||
|| **LANDING** [`boolean`](../data-types.md#standart-types) | Flag `Y`/`N` - whether the group is a publication group ||
|| **SCRUM_MASTER_ID** [`integer`](../data-types.md#standart-objects) | Identifier of the scrum master. `0` if the group is not a scrum ||
|| **SCRUM_SPRINT_DURATION** [`integer`](../data-types.md#standart-types) | Duration of the scrum sprint in seconds. `0` if the group is not a scrum ||
|| **SCRUM_TASK_RESPONSIBLE** [`string`](../data-types.md#standart-types) | Default assignee when creating tasks:
- `A` - creator
- `M` - scrum master
- `""` - group is not a scrum
||
|| **TYPE** [`enum`](../data-types.md#standart-types) | Type of group:
- `group` - group
- `project` - project
- `scrum` - scrum
- `collab` - collaboration
||
|| **MEMBERS** [`array`](../data-types.md#standart-types) | Identifiers of group members ||
|| **CHAT_ID** [`integer`](../data-types.md#standart-types) | Identifier of the group chat ||
|| **DIALOG_ID** [`string`](../data-types.md#standart-types) | Identifier of the group dialog ||
|| **ORDINARY_MEMBERS** [`array`](../data-types.md#standart-types) | Array of identifiers of group users who are not owners or moderators ||
|| **INVITED_MEMBERS** [`array`](../data-types.md#standart-types) | Array of identifiers of portal users who have been invited to the group but have not yet accepted ||
|| **MODERATOR_MEMBERS** [`array`](../data-types.md#standart-types) | Array of identifiers of group members with the role of moderator ||
|| **SITE_IDS** [`array`](../data-types.md#standart-types) | List of identifiers of sites to which the group belongs ||
|| **AVATAR** [`string`](../data-types.md#standart-types) | URL of the group's compressed user avatar. `""` if no user avatar is set ||
|| **AVATAR_TYPES** [`object`](../data-types.md#standart-types) | Object containing group avatars:
- `AVATAR_TYPE[avatar_id][sort]` [`integer`](../data-types.md#standart-types) - sorting of the avatar
- `AVATAR_TYPE[avatar_id][mobileUrl]` [`string`](../data-types.md#standart-types) - URL for displaying the avatar in the mobile app
- `AVATAR_TYPE[avatar_id][webCssClass]` [`string`](../data-types.md#standart-types) - CSS class of the avatar
- `AVATAR_TYPE[avatar_id][entitySelectorUrl]` [`string`](../data-types.md#standart-types) - Entity selector URL
||
|| **AVATAR_DATA** [`object`](../data-types.md#standart-types) | Information about the group's avatar:
- `AVATAR_DATA[type]` [`string`](../data-types.md#standart-types) - type of avatar:
    - `icon` - system avatar
    - `image` - user image
- `AVATAR_DATA[id]` [`string`](../data-types.md#standart-types) - identifier of the avatar:
    - `folder` - folder avatar
    - `tasks` - checkbox avatar
    - `chart` - pie chart avatar
    - `briefcase` - briefcase avatar
    - `group` - silhouettes avatar
    - `""` - other system avatar
    - `URL` - link to the compressed user avatar if set
||
|| **OWNER_DATA** [`object`](../data-types.md#standart-types) | Information about the group owner:
- `OWNER_DATA[ID]` [`user`](../data-types.md#standart-objects) - identifier of the group owner
- `OWNER_DATA[PHOTO]` [`string`](../data-types.md#standart-types) - URL of the owner's compressed avatar
- `OWNER_DATA[FORMATTED_NAME]` [`string`](../data-types.md#standart-types) - formatted name of the group owner according to account settings
||
|| **SUBJECT_DATA** [`object`](../data-types.md#standart-types) | Information about the group's subject specified in the advanced group settings:
- `SUBJECT_DATA[ID]` [`integer`](../data-types.md#standart-types) - identifier of the subject
- `SUBJECT_DATA[NAME]` [`string`](../data-types.md#standart-types) - name of the subject
||
|| **TAGS** [`array`](../data-types.md#standart-types) | Group tags, similar to `KEYWORDS`, but in array format ||
|| **ACTIONS** [`object`](../data-types.md#standart-types) | Data about available operations for the current user on the group:
- `ACTIONS[EDIT]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether editing the group is available
- `ACTIONS[DELETE]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether deleting the group is available
- `ACTIONS[INVITE]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether inviting external participants to the group is available
- `ACTIONS[JOIN]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user can join the group
- `ACTIONS[LEAVE]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user can leave the group
- `ACTIONS[FOLLOW]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user can subscribe to group updates
- `ACTIONS[PIN]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether pinning the group is available
- `ACTIONS[EDIT_FEATURES]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether changing group features is available
||
|| **USER_DATA** [`object`](../data-types.md#standart-types) | Information about the current user regarding the group:
- `USER_DATA[ROLE]` [`any`](../data-types.md#standart-types) - user's role in the group:
    - `A` - group owner
    - `E` - group moderator
    - `K` - group participant
    - `Z` - awaiting membership
    - `false` - value is absent
- `USER_DATA[INITIATED_BY_TYPE]` [`any`](../data-types.md#standart-types) - who initiated the user's connection to the group:
    - `U` - by a user, e.g., the user sent a request to join the group
    - `G` - by the group, e.g., the user was invited
    - `false` - value is absent
- `USER_DATA[IS_SUBSCRIBED]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user is subscribed to group updates
||
|| **DEPARTMENTS** [`array`](../data-types.md#standart-types) | Array of identifiers of departments added to the group ||
|| **IS_PIN** [`boolean`](../data-types.md#standart-types) | Value `true`/`false` - whether the group is pinned by the current user on the groups and projects page ||
|| **PRIVACY_CODE** [`string`](../data-types.md#standart-types) | Level of privacy of the group:
- `open` - open group
- `closed` - closed group
- `secret` - secret group
||
|| **LIST_OF_MEMBERS** [`object`](../data-types.md#standart-types) | Array with information about group users:
- `LIST_OF_MEMBERS[0][id]` [`user`](../data-types.md#standart-objects) - user identifier
- `LIST_OF_MEMBERS[0][isOwner]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user is the group owner
- `LIST_OF_MEMBERS[0][isModerator]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user is a moderator
- `LIST_OF_MEMBERS[0][isScrumMaster]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user is a scrum master (in the case of scrum groups)
- `LIST_OF_MEMBERS[0][isAutoMember]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user was added to the group automatically (without an invitation)
- `LIST_OF_MEMBERS[0][name]` [`string`](../data-types.md#standart-types) - user's first name
- `LIST_OF_MEMBERS[0][lastName]` [`string`](../data-types.md#standart-types) - user's last name
- `LIST_OF_MEMBERS[0][position]` [`string`](../data-types.md#standart-types) - user's position
- `LIST_OF_MEMBERS[0][photo]` [`string`](../data-types.md#standart-types) - URL of the user's compressed avatar
||
|| **FEATURES** [`object`](../data-types.md#standart-types) | Array with information about group features:
- `FEATURES[0][featureName]` [`string`](../data-types.md#standart-types) - symbolic identifier of the feature
- `FEATURES[0][name]` [`string`](../data-types.md#standart-types) - name of the feature
- `FEATURES[0][customName]` [`string`](../data-types.md#standart-types) - custom name of the feature in the advanced group settings
- `FEATURES[0][id]` [`string`](../data-types.md#standart-types) - numeric identifier of the feature
- `FEATURES[0][active]` [`boolean`](../data-types.md#standart-types) - Value `true` / `false` - whether the feature is enabled in the group
||
|| **LIST_OF_MEMBERS_AWAITING_INVITE** [`object`](../data-types.md#standart-types) | Information about users awaiting confirmation to join the group:
- `LIST_OF_MEMBERS_AWAITING_INVITE[0][id]` [`user`](../data-types.md#standart-objects) - user identifier
- `LIST_OF_MEMBERS_AWAITING_INVITE[0][name]` [`string`](../data-types.md#standart-types) - formatted name according to account settings
- `LIST_OF_MEMBERS_AWAITING_INVITE[0][photo]` [`string`](../data-types.md#standart-types) - URL of the user's compressed avatar
||
|| **GROUP_MEMBERS_LIST** [`object`](../data-types.md#standart-types) | Information about users associated with the group:
- `GROUP_MEMBERS_LIST[0][id]` [`user`](../data-types.md#standart-objects) - participant identifier
- `GROUP_MEMBERS_LIST[0][invited]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user was invited
- `GROUP_MEMBERS_LIST[0][isAwaiting]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user is awaiting acceptance into the group
- `GROUP_MEMBERS_LIST[0][isMember]` [`boolean`](../data-types.md#standart-types) - Value `true`/`false` - whether the user is a member of the group
||
|| **COUNTERS** [`object`](../data-types.md#standart-types) | Counters:
- `COUNTERS[workgroup_requests_out]` [`integer`](../data-types.md#standart-types) - current number of unaccepted invitations to the group
- `COUNTERS[workgroup_requests_in]` [`integer`](../data-types.md#standart-types) - current number of requests to join the group
||
|| **EFFICIENCY** [`integer`](../data-types.md#standart-types) | Group efficiency ||
|| **ADDITIONAL_DATA** [`object`](../data-types.md#standart-types) | Additional data for the current user:
- `ADDITIONAL_DATA[ROLE]` [`string`](../data-types.md#standart-types) - role of the current user in the group:
    - `A` - group owner
    - `E` - group moderator
    - `K` - group participant
    - `Z` - awaiting membership
    - `""` - user has no relation to the group
- `ADDITIONAL_DATA[INITIATED_BY_TYPE]` [`string`](../data-types.md#standart-types) - who initiated the user's connection to the group:
    - `U` - by a user, e.g., the user sent a request to join the group
    - `G` - by the group, e.g., the user was invited
    - `""` - user has no relation to the group
||
|#

## Error handling

HTTP status: **400**

```json
{
    "error": "SONET_CONTROLLER_WORKGROUP_EMPTY",
    "error_description": "No value for the workgroup ID was provided."
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible error codes

#|
|| **Code** | **Description** | **Status** ||
|| `SONET_CONTROLLER_WORKGROUP_EMPTY` | The parameter `groupId` was not provided in the `params` array | 400 ||
|| `SONET_CONTROLLER_WORKGROUP_NOT_FOUND` | The group with the identifier `params[groupId]` was not found or the current user does not have access to it | 400 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}