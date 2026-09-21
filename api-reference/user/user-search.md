# Get a list of users with personal data search user.search

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

Searches for users by first name, last name, middle name, department name, and job title.

{% note info "" %}

The list of Bitrix24 user fields that will be retrieved as a result of the method execution depends on the scope of the application/webhook. The fields available in each version are listed in the [User Scope Versions](user-scope.md) article.

{% endnote %}

The method inherits the behavior of the [user.get](./user-get.md) method; all parameters from this function are also available.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FILTER**
[`object`](../data-types.md) | Object containing search fields [(detailed description)](#filter) ||
|| **sort**
[`string`](../data-types.md) | Field used to sort the results. Sorting works for all fields from [user.add](./user-add.md).

The default value is `ID` ||
|| **order**
[`string`](../data-types.md) | Sorting direction:
- `ASC` — ascending
- `DESC` — descending

The default value is `ASC` ||
|| **ADMIN_MODE**
[`boolean`](../data-types.md) | Enables administrator mode for retrieving user data. The parameter applies only when the request is made by an administrator.

The default value is `false` ||
|| **select**
[`array`](../data-types.md) | An array with the names of the fields to return in the response. Without this parameter, the method returns all fields available to the application or webhook scope.

When selecting, use masks:

- `*` — all available fields
- `UF_*` — all available custom fields, including those created in Bitrix24

The method skips fields that are unavailable to the scope or do not exist, without returning an error ||
|| **start**
[`integer`](../data-types.md) | The parameter is used to manage pagination.

The result page size is always static: 50 records.

To select the second page of results, you must pass the value `50`. To select the third page of results — the value `100` and so on.

Formula for calculating the value of the `start` parameter:

`start = (N - 1) * 50`, where `N` is the desired page number.

The default value is `0` ||
|#

### The FILTER Parameter {#filter}

#|
|| **Name**
`type` | **Description** ||
|| **FIND**
[`string`](../data-types.md) | Search string applied simultaneously to the first name, last name, middle name, job title, and department name ||
|| **NAME**
[`string`](../data-types.md) | User's first name ||
|| **LAST_NAME**
[`string`](../data-types.md) | User's last name ||
|| **SECOND_NAME**
[`string`](../data-types.md) | User's middle name ||
|| **WORK_POSITION**
[`string`](../data-types.md) | User's job title ||
|| **UF_DEPARTMENT_NAME**
[`string`](../data-types.md) | Department name ||
|| **USER_TYPE**
[`string`](../data-types.md) | User type. Possible values:

- `employee` — employee
- `extranet` — extranet user
- `email` — email user ||
|#

{% note info "" %}

To search across personal data, pass only `FIND`. To search by specific fields, omit `FIND` and specify one or more of the following fields: `NAME`, `LAST_NAME`, `SECOND_NAME`, `WORK_POSITION`, `UF_DEPARTMENT_NAME`.

The method automatically selects full-text search or a beginning-of-string search using `USER_NAME LIKE "Text%"`. A beginning-of-string search is faster than a two-sided `LIKE "%text%"` or left-sided `LIKE "%text"` search because the database fields are indexed.

{% endnote %}

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "FILTER": {
            "FIND": "Klaus"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.search
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "FILTER": {
            "FIND": "Klaus"
        },
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
      const response = await $b24.actions.v2.call.make<UserItem[]>({
        method: 'user.search',
        params: {
          FILTER: {
            FIND: 'Klaus',
          },
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

          const response = await $b24.actions.v2.call.make({
            method: 'user.search',
            params: {
              FILTER: {
                FIND: 'Klaus',
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
          console.info('Users found on this page:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchUsers)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.search(
            filter={
                "FIND": "Klaus",
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'user.search',
                [
                    'FILTER' => [
                        'FIND' => 'Klaus',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
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
            "FILTER": {
                "FIND": "Klaus"
            }
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
            "FILTER" => [
                "FIND" => "Klaus",
            ],
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
        "total": 1,
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
[`object[]`](../data-types.md) | Array of users that match the search conditions [(detailed description)](#result).

The set of fields depends on the scope and the `select` parameter ||
|| **total**
[`integer`](../data-types.md) | Total number of records found ||
|| **next**
[`integer`](../data-types.md) | Offset for the next page. The field is omitted on the last page ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### The result Object {#result}

The method returns fields available to the application or webhook scope. You can retrieve the complete list using the [user.fields](./user-fields.md) method.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | User ID ||
|| **ACTIVE**
[`boolean`](../data-types.md) | User activity indicator ||
|| **NAME**
[`string`](../data-types.md) | User's first name ||
|| **LAST_NAME**
[`string`](../data-types.md) | User's last name ||
|| **SECOND_NAME**
[`string`](../data-types.md) | User's middle name ||
|| **EMAIL**
[`string`](../data-types.md) | Email address ||
|| **LAST_LOGIN**
[`datetime`](../data-types.md) | Date and time of the last login ||
|| **DATE_REGISTER**
[`datetime`](../data-types.md) | Registration date and time ||
|| **IS_ONLINE**
[`string`](../data-types.md) | Online activity indicator. Possible values:

- `Y` — the user is online
- `N` — the user is not online ||
|| **PERSONAL_BIRTHDAY**
[`date`](../data-types.md) | Date of birth ||
|| **WORK_POSITION**
[`string`](../data-types.md) | Job title ||
|| **UF_DEPARTMENT**
[`integer[]`](../data-types.md) | User's department IDs ||
|| **USER_TYPE**
[`string`](../data-types.md) | User type: `employee`, `extranet`, or `email` ||
|#

## Error Handling

The method does not return specific errors.

{% include notitle [error handling](../../_includes/error-info.md) %}

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-add.md)
- [{#T}](./user-update.md)
- [{#T}](./user-get.md)
- [{#T}](./user-current.md)
- [{#T}](./user-fields.md)
