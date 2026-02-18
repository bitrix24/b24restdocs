# Find Departments im.search.department.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.search.department.list` performs a search for departments.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **FIND^*^**
[`unknown`](../../data-types.md) | `Los Angeles` | Search phrase | 19 ||
|| **USER_DATA**
[`unknown`](../../data-types.md) | `N` | Load user data | 19 ||
|| **OFFSET**
[`unknown`](../../data-types.md) | `0` | Offset for user selection | 19 ||
|| **LIMIT**
[`unknown`](../../data-types.md) | `10` | Limit for user selection | 19 ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

- If the parameter `USER_DATA = Y` is passed, data about the manager will be loaded with the result.
- The search is conducted on the following field: **Full department name**.
- The method supports standard pagination of the Bitrix24 Rest API, but in addition, it allows navigation using the `OFFSET` and `LIMIT` parameters.

## Examples

{% list tabs %}

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'im.search.department.list',
        {
          FIND: 'Los Angeles'
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('im.search.department.list', { FIND: 'Los Angeles' }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('im.search.department.list', { FIND: 'Los Angeles' }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.search.department.list',
                [
                    'FIND' => 'Los Angeles'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'users: ' . print_r($result->data(), true);
        echo 'total: ' . $result->total();
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error searching department list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.search.department.list',
        {
            FIND: 'Los Angeles'
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log('users', result.data());
                console.log('total', result.total());
            }
        }
    );
    ```

- PHP CRest

    ```php
    $result = restCommand(
        'im.search.department.list',
        Array(
            'FIND' => 'Los Angeles'
        ),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

- cURL

    // example for cURL

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Successful Response

```json
{    
    "result": [
        {
            "id": 51,
            "name": "Los Angeles Branch",
            "full_name": "Los Angeles Branch / Bitrix",
            "manager_user_id": 11
        }
    ],
    "total": 1
}            
```

### Key Descriptions

- `id` – department identifier
- `name` – short department name
- `full_name` – full department name
- `manager_user_data` – object describing manager data (not available if `USER_DATA != 'Y'`)
- `id` – user identifier
- `name` – user's first and last name
- `first_name` – user's first name
- `last_name` – user's last name
- `work_position` – position
- `color` – user's color in hex format
- `avatar` – link to avatar (if empty, avatar is not set)
- `gender` – user's gender
- `birthday` – user's birthday in DD-MM format, if empty – not set
- `extranet` – indicator of external extranet user (`true/false`)
- `network` – indicator of Bitrix24.Network user (`true/false`)
- `bot` – indicator of bot (`true/false`)
- `connector` – indicator of open channel user (`true/false`)
- `external_auth_id` – external authorization code
- `status` – selected user status
- `idle` – date when the user stepped away from the computer, in ATOM format (if not set, `false`)
- `last_activity_date` – date of the user's last action in ATOM format
- `mobile_last_date` – date of the last action in the mobile app in ATOM format (if not set, `false`)
- `absent` – date until which the user is on vacation, in ATOM format (if not set, `false`)

## Error Response

```json
{
    "error": "FIND_SHORT",
    "error_description": "Too short a search phrase."
}
```

### Key Descriptions

- `error` – error code
- `error_description` – brief description of the error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **FIND_SHORT** | Search phrase is too short; searching starts from three characters. ||
|#

