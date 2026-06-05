# Get a list of users with personal data search user.search

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `user.search` method allows you to retrieve a list of users with accelerated search based on personal data (first name, last name, patronymic, department name, job title). It operates in two modes: quickly using **Fulltext Index** and a slower option through [right LIKE](*key_right LIKE) (support is determined automatically).

{% note info "" %}

The list of Bitrix24 user fields that will be retrieved as a result of the method execution depends on the scope of the application/webhook. Details regarding access to user data can be found in the [article](index.md).

{% endnote %}

The method inherits the behavior of the [user.get](./user-get.md) method; all parameters from this function are also available.

## Method Parameters

{% include [Note on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FILTER**
[`string`](../data-types.md) | The array can contain fields in any combination:
- `NAME` — first name
- `LAST_NAME` — last name
- `WORK_POSITION` — position
- `UF_DEPARTMENT_NAME` — department name
- `USER_TYPE` — user type. It can take the following values: 
    - `employee` — employee
    - `extranet` — extranet user
    - `email` — mail user

Or `FIND` — a field that will be searched for in all listed fields.

{% note info "" %}

The method can work either with filtering using the FIND key or with all other fields. It is not possible to use FIND and any other field simultaneously.

{% endnote %} ||
|| **sort**
[`string`](../data-types.md) | The field by which the results are sorted. Sorting works for all fields from [user.add](./user-add.md) ||
|| **order**
[`string`](../data-types.md) | Sorting direction:
- `ASC` — ascending
- `DESC` — descending ||
|| **ADMIN_MODE**
[`boolean`](../data-types.md) | [Key for operation](*key_Key for operation) in administrator mode. Used to retrieve data about any users ||
|| **start**
[`integer`](../data-types.md) | The parameter is used to manage pagination.

The result page size is always static: 50 records.

To select the second page of results, you must pass the value `50`. To select the third page of results — the value `100` and so on.

Formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "UF_DEPARTMENT": 1,
        "SORT": "ID",
        "ORDER": "asc",
        "start": 10
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.search
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "UF_DEPARTMENT": 1,
        "SORT": "ID",
        "ORDER": "asc",
        "start": 10,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.search
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each user object returned in result[]
    type UserItem = {
      ID: string
      ACTIVE: boolean
      NAME: string
      LAST_NAME: string
      SECOND_NAME?: string
      EMAIL: string
      LAST_LOGIN: ISODate | ''
      DATE_REGISTER: ISODate | ''
      IS_ONLINE: string
      PERSONAL_BIRTHDAY?: ISODate | ''
      WORK_POSITION?: string
      UF_DEPARTMENT?: number[]
      USER_TYPE: 'employee' | 'extranet' | 'email'
    }

    try {
      // user.search returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<UserItem[]>({
        method: 'user.search',
        params: {
          UF_DEPARTMENT: 1,
          SORT: 'ID',
          ORDER: 'asc',
          start: 10,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Users found on this page:', result.length, result)
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
      async function searchUsers() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // user.search returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'user.search',
            params: {
              UF_DEPARTMENT: 1,
              SORT: 'ID',
              ORDER: 'asc',
              start: 10,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Users found on this page:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchUsers)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'user.search',
                [
                    'UF_DEPARTMENT' => 1,
                    'SORT'         => 'ID',
                    'ORDER'        => 'asc',
                    'start'        => 10,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error searching for users: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "user.search",
        {
            "UF_DEPARTMENT": 1,
            "SORT": "ID",
            "ORDER": "asc"
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
                return;
            }

            console.dir(result.data());

            if (result.more())
            {
                result.next();
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.search',
        [
            "UF_DEPARTMENT" => 1,
            "SORT" => 'ID',
            "ORDER" => 'asc',
            "start" => 10
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
                "ID": "1",
                "ACTIVE": true,
                "NAME": "Vadim",
                "LAST_NAME": "Valeev",
                "SECOND_NAME": "",
                "EMAIL": "v.r.valeev@bitrix.com",
                "LAST_LOGIN": "2024-07-25T13:06:54+00:00",
                "DATE_REGISTER": "2024-07-15T00:00:00+00:00",
                "TIME_ZONE": "",
                "IS_ONLINE": "Y",
                "TIMESTAMP_X": {
                },
                "LAST_ACTIVITY_DATE": {
                },
                "PERSONAL_GENDER": "",
                "PERSONAL_WWW": "",
                "PERSONAL_BIRTHDAY": "2018-07-14T00:00:00+00:00",
                "PERSONAL_MOBILE": "",
                "PERSONAL_CITY": "",
                "WORK_PHONE": "",
                "WORK_POSITION": "",
                "UF_EMPLOYMENT_DATE": "",
                "UF_DEPARTMENT": [1],
                "USER_TYPE": "employee"
            },
            {
                "ID": "3",
                "ACTIVE": true,
                "NAME": "John",
                "LAST_NAME": "Smith",
                "EMAIL": "test@gmail.com",
                "LAST_LOGIN": "2024-07-24T09:01:55+00:00",
                "DATE_REGISTER": "2024-07-22T00:00:00+00:00",
                "IS_ONLINE": "N",
                "TIMESTAMP_X": {
                },
                "LAST_ACTIVITY_DATE": {
                },
                "PERSONAL_GENDER": "",
                "PERSONAL_BIRTHDAY": "",
                "WORK_POSITION": "",
                "UF_EMPLOYMENT_DATE": "",
                "UF_DEPARTMENT": [1],
                "USER_TYPE": "employee"
            }
        ],
        "total": 2,
        "time": {
            "start": 1721913235.39648,
            "finish": 1721913235.45078,
            "duration": 0.05430006980896,
            "processing": 0.0187909603118897,
            "date_start": "2024-07-25T13:13:55+00:00",
            "date_finish": "2024-07-25T13:13:55+00:00",
            "operating": 0
        }
    }
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The response root element, which contains the filtered list of users ||
|| **total**
[`integer`](../data-types.md) | Total number of records found ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-add.md)
- [{#T}](./user-update.md)
- [{#T}](./user-get.md)
- [{#T}](./user-current.md)
- [{#T}](./user-fields.md)

[*right LIKE key]: USER_NAME LIKE "Text%" — this is called a right like, where the search is performed only on text that begins with the specified phrase but may contain different endings. Such a search is significantly faster than a two-sided like "%text%" or a left-side like "%text" due to the architecture of indexed field storage in the database.