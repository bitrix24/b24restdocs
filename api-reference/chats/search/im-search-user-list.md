# Find Users im.search.user.list

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.user.list` allows you to search for users by first name, last name, job title, and department.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **FIND***
[`string`](../../data-types.md) | Search phrase. The minimum number of characters for the search is `3` ||
|| **BUSINESS**
[`string`](../../data-types.md) | Search only among business users. 

Allowed values:
- `Y` — yes
- `N` — no

Default value is `N` ||

|| **OFFSET**
[`integer`](../../data-types.md) | Offset for the user selection. Default is `0` ||
|| **LIMIT**
[`integer`](../../data-types.md) | Number of items in the selection. Default is `10`. Maximum value is `50` ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FIND":"John","BUSINESS":"N","OFFSET":0,"LIMIT":10}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.search.user.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FIND":"John","BUSINESS":"N","OFFSET":0,"LIMIT":10,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.search.user.list
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.search.user.list', {
        FIND: 'John',
        BUSINESS: 'N',
        OFFSET: 0,
        LIMIT: 10,
      });

      const { result, total, next } = response.getData();
      console.log(result, total, next);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.search.user.list',
            [
                'FIND' => 'John',
                'BUSINESS' => 'N',
                'OFFSET' => 0,
                'LIMIT' => 10,
            ]
        );

        $result = $response->getResponseData()->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            var_dump($result->data());
        }
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.search.user.list',
        {
            FIND: 'John',
            BUSINESS: 'N',
            OFFSET: 0,
            LIMIT: 10,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data(), result.total(), result.next());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.search.user.list',
        [
            'FIND' => 'John',
            'BUSINESS' => 'N',
            'OFFSET' => 0,
            'LIMIT' => 10,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```
{% endlist %}

## Response Handling

HTTP Status Code: **200**

```json
{
    "result": [
        {
            "id": 103,
            "name": "Svetlana Ivanova",
            "first_name": "Svetlana",
            "last_name": "Ivanova",
            "work_position": "IT Department Head",
            "color": "#4ba984",
            "avatar": "https://example.bitrix24.com/upload/main/avatar.png",
            "gender": "F",
            "birthday": "08-03",
            "extranet": false,
            "network": false,
            "bot": false,
            "connector": false,
            "external_auth_id": "socservices",
            "status": "online",
            "idle": false,
            "last_activity_date": "2026-03-04T15:40:56+01:00",
            "mobile_last_date": false,
            "departments": [1, 7],
            "absent": false,
            "phones": {
                "work_phone": "19123456789",
                "personal_mobile": "12123456789",
                "inner_phone": "78"
            }
        },
        ... // description for each user
    ],
    "total": 2,
    "time": {
        "start": 1772628089,
        "finish": 1772628089.061656,
        "duration": 0.06165599822998047,
        "processing": 0,
        "date_start": "2026-03-04T15:41:29+01:00",
        "date_finish": "2026-03-04T15:41:29+01:00",
        "operating_reset_at": 1772628689,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | List of found users.

The structure of the user object is described in detail [below](#user-object) ||
|| **total**
[`integer`](../../data-types.md) | Total number of found users ||
|| **next**
[`integer`](../../data-types.md) | Offset for the next page. This field is returned if there is a next page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### User Object {#user-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | User identifier ||
|| **name**
[`string`](../../data-types.md) | User's full name ||
|| **first_name**
[`string`](../../data-types.md) | User's first name ||
|| **last_name**
[`string`](../../data-types.md) | User's last name ||
|| **work_position**
[`string`](../../data-types.md) | User's job title ||
|| **color**
[`string`](../../data-types.md) | User's color in HEX format ||
|| **avatar**
[`string`](../../data-types.md) 
[`null`](../../data-types.md) | Link to the user's avatar ||
|| **gender**
[`string`](../../data-types.md) | User's gender: `M` or `F` ||
|| **birthday**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Birthday in `DD-MM` format or `false` ||
|| **extranet**
[`boolean`](../../data-types.md) | Indicator of extranet user ||
|| **network**
[`boolean`](../../data-types.md) | Indicator of Bitrix24 Network user ||
|| **bot**
[`boolean`](../../data-types.md) | Indicator of bot user ||
|| **connector**
[`boolean`](../../data-types.md) | Indicator of open channels connector user ||
|| **external_auth_id**
[`string`](../../data-types.md) | External authorization identifier ||
|| **status**
[`string`](../../data-types.md) | Current status of the user ||
|| **idle**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Time of transition to "Away" status in ISO 8601 (RFC3339) format or `false` ||
|| **last_activity_date**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Time of last activity in ISO 8601 (RFC3339) format or `false` ||
|| **mobile_last_date**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Time of last mobile activity in ISO 8601 (RFC3339) format or `false` ||
|| **departments**
[`array`](../../data-types.md) | Array of department identifiers ||
|| **absent**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Date of absence end in ISO 8601 (RFC3339) format or `false` ||
|| **phones**
[`object`](../../data-types.md) | User's phones or `false`.

The structure of the object is described in detail [below](#phones-object) ||
|#

### Phones Object {#phones-object}

#|
|| **Name**
`Type` | **Description** ||
|| **work_phone**
[`string`](../../data-types.md) | Work phone ||
|| **personal_mobile**
[`string`](../../data-types.md) | Mobile phone ||
|| **inner_phone**
[`string`](../../data-types.md) | Internal phone ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "FIND_SHORT",
    "error_description": "Too short a search phrase."
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `FIND_SHORT` | Too short a search phrase | The search phrase is not provided or is too short for the internal search filter ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-search-chat-list.md)
- [{#T}](./im-search-department-list.md)
- [{#T}](./im-search-last-add.md)
- [{#T}](./im-search-last-get.md)
- [{#T}](./im-search-last-delete.md)