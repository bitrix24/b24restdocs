# Get a list of users by filter user.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `user.get` method returns a list of users with filtering, sorting, and pagination.

{% note info "" %}

The method does not return bots, integrators, e-mail users, Open Channels users, or Replica users. The set of fields in the response depends on the `user` scope version — see the [User Scope Versions](user-scope.md) article for details.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`object`](../data-types.md) | An object with user selection conditions [(detailed description)](#filter).

Without this parameter, the method returns all users available to the current user ||
|| **select**
[`array`](../data-types.md) | An array with the names of the fields to return in the response. Without this parameter, the method returns all fields available to the application or webhook scope.

When selecting, use masks:

- `*` — all available fields
- `UF_*` — all available custom fields, including those created in Bitrix24

The method skips fields that are unavailable to the scope or do not exist, without returning an error ||
|| **sort**
[`string`](../data-types.md) | The field by which the list is sorted, for example `LAST_NAME`. Fields from [user.add](./user-add.md) and custom `UF_*` fields are available.

Sorting is applied only if both `sort` and `order` are passed as strings. Otherwise, the method sorts the list by ascending `ID`.

An unknown field causes error `100` ||
|| **order**
[`string`](../data-types.md) | Sorting direction:
- `ASC` — ascending
- `DESC` — descending

The value is case-insensitive. Any other value causes the `ERROR_ARGUMENT` error ||
|| **ADMIN_MODE**
[`boolean`](../data-types.md) | Administrator mode:
- `true` — the method returns all users, including extranet users from any groups
- `false` — the method returns company employees and extranet users from the groups the current user belongs to. An extranet user retrieves only members of their own groups and themselves

The mode is enabled only if the request is made by an administrator. For other users, the parameter has no effect.

Default: `false` ||
|| **IMAGE_RESIZE**
[`string`](../data-types.md) | The size of the photo copy in the `PERSONAL_PHOTO` field:

- `small` — 150 × 150 pixels
- `medium` — 300 × 300 pixels
- `large` — 1000 × 1000 pixels

Without this parameter or with any other value, the method returns a link to the original image ||
|| **start**
[`integer`](../data-types.md) | The parameter is used to control pagination.

The results page size is always static: 50 records.

To select the second page of results, you must pass the value `50`. To select the third page of results — the value `100` and so on.

Formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` — the number of the desired page.

The value `-1` disables the total record count: the method returns the first page and `total` with the value `0`.

Default: `0` ||
|#

Parameter names are case-insensitive: `filter` and `FILTER` are equivalent.

### Filter Parameter {#filter}

The object key is a user field, and the value is the value to search for or an array of values. Conditions are combined with AND logic.

You can filter by fields from [user.add](./user-add.md) that are allowed for the application or webhook scope, and by the additional fields from the table. The method ignores unknown fields and fields unavailable to the scope without an error.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) \| [`array`](../data-types.md) | User ID or an array of IDs ||
|| **ACTIVE**
[`boolean`](../data-types.md) | Employee status:
- `true` — active employees
- `false` — dismissed employees

Without this field, the method returns both ||
|| **UF_DEPARTMENT**
[`integer`](../data-types.md) \| [`array`](../data-types.md) | Department ID or an array of IDs. The method selects users who belong directly to the department, without its child departments.

Retrieve department IDs using the [department.get](../departments/department-get.md) method ||
|| **HAS_DEPARTAMENT**
[`string`](../data-types.md) | Selection of users with a department:
- `Y` — only users who have a department specified
- `N` — the condition is not applied ||
|| **USER_TYPE**
[`string`](../data-types.md) | User type:
- `employee` — employee
- `extranet` — extranet user

The method accepts the `email` and `bot` values but returns an empty list: it does not return such users ||
|| **IS_ONLINE**
[`string`](../data-types.md) | Whether the user is online:
- `Y` — online
- `N` — not online ||
|| **NAME_SEARCH**
[`string`](../data-types.md) | Quick search by personal data: first name, last name, and other fields of the user search index. Prefixes are not applied to this field ||
|| **UF_PHONE_INNER**
[`string`](../data-types.md) | Internal phone number ||
|| **UF_\***
[`any`](../data-types.md) | Custom fields, for example `UF_USR_1705409592930`. If you pass an empty array for a non-string custom field with the `@` prefix, the method returns an empty result; with the `!@` prefix, the method ignores the condition ||
|#

You can add a prefix to a key to refine the selection condition:

- `=` — equal, exact match. Used by default
- `!=`, `!` — not equal
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — is in the list, the value is passed as an array
- `!@` — is not in the list, the value is passed as an array
- `%` — contains a substring in any position. You do not need to pass the `%` symbol in the value
- `!%` — does not contain a substring. You do not need to pass the `%` symbol in the value
- `=%` and `%=` — LIKE pattern, the `%` symbol is passed in the value:
    - `"Kla%"` — values starting with "Kla"
    - `"%mann"` — values ending with "mann"
    - `"%aus%"` — values where "aus" occurs in any position

Without a prefix, a value with the `%` symbol also works as a LIKE pattern: `"NAME": "Kla%"` selects users whose first name starts with "Kla".

The method does not recognize prefixes longer than two characters, such as `!=%` or `!%=`, and ignores the condition. To select values that do not match a pattern, use `!%` with a substring.

Pass the values of date and time fields in ISO 8601 format, for example `2020-01-01T00:00:00+02:00`, or in the date format of your Bitrix24. This applies to the `TIMESTAMP_X`, `DATE_REGISTER`, `LAST_LOGIN`, `LAST_ACTIVITY_DATE`, `PERSONAL_BIRTHDAY`, `UF_EMPLOYMENT_DATE` fields and to custom fields of the date and date-time types. If the value is not recognized as a date, the condition retrieves no records.

{% note info "" %}

You can also pass filter fields at the top level of the request, without `filter`, for example `{"UF_DEPARTMENT": 1}`. This is the legacy request format. If the request contains `filter`, the method ignores top-level fields.

{% endnote %}

{% note tip "" %}

To speed up list retrieval:

- Pass only the fields you need in the `select` parameter. If none of them are custom fields, the method skips loading custom field data
- Disable the total count calculation with `start = -1` when you retrieve large volumes of data. In this mode, select pages by filtering on the last received identifier — [How to Retrieve Large Volumes of Data](../../settings/performance/huge-data.md)

{% endnote %}

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

The example retrieves active employees of the department with ID `1`, sorted by last name.

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "filter": {
            "UF_DEPARTMENT": 1,
            "ACTIVE": true
        },
        "select": ["ID", "NAME", "LAST_NAME", "EMAIL", "WORK_POSITION", "UF_DEPARTMENT", "USER_TYPE"],
        "sort": "LAST_NAME",
        "order": "ASC",
        "start": 0
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.get
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "filter": {
            "UF_DEPARTMENT": 1,
            "ACTIVE": true
        },
        "select": ["ID", "NAME", "LAST_NAME", "EMAIL", "WORK_POSITION", "UF_DEPARTMENT", "USER_TYPE"],
        "sort": "LAST_NAME",
        "order": "ASC",
        "start": 0,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type UserData = {
      ID: string
      NAME: string
      LAST_NAME: string
      EMAIL: string
      WORK_POSITION: string
      UF_DEPARTMENT: number[]
      USER_TYPE: string
    }

    try {
      // user.get returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<UserData[]>({
        method: 'user.get',
        params: {
          filter: {
            UF_DEPARTMENT: 1,
            ACTIVE: true,
          },
          select: ['ID', 'NAME', 'LAST_NAME', 'EMAIL', 'WORK_POSITION', 'UF_DEPARTMENT', 'USER_TYPE'],
          sort: 'LAST_NAME',
          order: 'ASC',
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(`Fetched ${result.length} users:`, result.map(u => `${u.NAME} ${u.LAST_NAME}`))
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
      async function getUsers() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // user.get returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'user.get',
            params: {
              filter: {
                UF_DEPARTMENT: 1,
                ACTIVE: true,
              },
              select: ['ID', 'NAME', 'LAST_NAME', 'EMAIL', 'WORK_POSITION', 'UF_DEPARTMENT', 'USER_TYPE'],
              sort: 'LAST_NAME',
              order: 'ASC',
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
          console.info(`Fetched ${result.length} users:`, result.map(u => `${u.NAME} ${u.LAST_NAME}`))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getUsers)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.get(
            filter={
                "UF_DEPARTMENT": 1,
                "ACTIVE": True,
            },
            select=["ID", "NAME", "LAST_NAME", "EMAIL", "WORK_POSITION", "UF_DEPARTMENT", "USER_TYPE"],
            sort="LAST_NAME",
            order="ASC",
            start=0,
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
                'user.get',
                [
                    'filter' => [
                        'UF_DEPARTMENT' => 1,
                        'ACTIVE'        => true,
                    ],
                    'select' => ['ID', 'NAME', 'LAST_NAME', 'EMAIL', 'WORK_POSITION', 'UF_DEPARTMENT', 'USER_TYPE'],
                    'sort'   => 'LAST_NAME',
                    'order'  => 'ASC',
                    'start'  => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "user.get",
        {
            "filter": {
                "UF_DEPARTMENT": 1,
                "ACTIVE": true
            },
            "select": ["ID", "NAME", "LAST_NAME", "EMAIL", "WORK_POSITION", "UF_DEPARTMENT", "USER_TYPE"],
            "sort": "LAST_NAME",
            "order": "ASC",
            "start": 0
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
        'user.get',
        [
            'filter' => [
                'UF_DEPARTMENT' => 1,
                'ACTIVE' => true
            ],
            'select' => ['ID', 'NAME', 'LAST_NAME', 'EMAIL', 'WORK_POSITION', 'UF_DEPARTMENT', 'USER_TYPE'],
            'sort' => 'LAST_NAME',
            'order' => 'ASC',
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "user.get", b24.Params{
    	"filter": b24.Params{
    		"UF_DEPARTMENT": 1,
    		"ACTIVE":        true,
    	},
    	"select": []string{"ID", "NAME", "LAST_NAME", "EMAIL", "WORK_POSITION", "UF_DEPARTMENT", "USER_TYPE"},
    	"sort":   "LAST_NAME",
    	"order":  "ASC",
    	"start":  0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("user.get: %w", err)
    }

    var items []struct {
    	ID           b24.ID `json:"ID"`
    	Name         string `json:"NAME"`
    	LastName     string `json:"LAST_NAME"`
    	Email        string `json:"EMAIL"`
    	WorkPosition string `json:"WORK_POSITION"`
    	Department   []int  `json:"UF_DEPARTMENT"`
    	UserType     string `json:"USER_TYPE"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Name, it.LastName)
    }
    ```

{% endlist %}

### Filtering by a name starting with "Kla"

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"NAME":"Kla%"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"NAME":"Kla%"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/user.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type UserData = {
      ID: string
      ACTIVE: boolean
      NAME: string
      LAST_NAME: string
      EMAIL: string
      LAST_LOGIN: ISODate | ''
      DATE_REGISTER: ISODate | ''
      IS_ONLINE: string
      UF_DEPARTMENT: number[]
      USER_TYPE: string
    }

    try {
      // user.get returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<UserData[]>({
        method: 'user.get',
        params: {
          filter: {
            NAME: 'Kla%',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(`Fetched ${result.length} users:`, result.map(u => `${u.NAME} ${u.LAST_NAME}`))
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
      async function getUsersByName() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // user.get returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'user.get',
            params: {
              filter: {
                NAME: 'Kla%',
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
          console.info(`Fetched ${result.length} users:`, result.map(u => `${u.NAME} ${u.LAST_NAME}`))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getUsersByName)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.get(
            filter={
                "NAME": "Kla%",
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
                'user.get',
                [
                    'filter' => [
                        'NAME' => 'Kla%'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching user data: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "user.get",
        {
            filter: {
                "NAME": "Kla%"
            }
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
        'user.get',
        [
            'filter' => [
                'NAME' => 'Kla%'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "user.get", b24.Params{
    	"filter": b24.Params{
    		"NAME": "Kla%",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("user.get: %w", err)
    }

    var items []struct {
    	ID         b24.ID `json:"ID"`
    	Active     bool   `json:"ACTIVE"`
    	Name       string `json:"NAME"`
    	LastName   string `json:"LAST_NAME"`
    	SecondName string `json:"SECOND_NAME"`
    	Email      string `json:"EMAIL"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Active)
    }
    ```

{% endlist %}

### Filtering by a last name not containing "er"

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"!%LAST_NAME":"er"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"!%LAST_NAME":"er"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/user.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type UserData = {
      ID: string
      ACTIVE: boolean
      NAME: string
      LAST_NAME: string
      EMAIL: string
      LAST_LOGIN: ISODate | ''
      DATE_REGISTER: ISODate | ''
      IS_ONLINE: string
      UF_DEPARTMENT: number[]
      USER_TYPE: string
    }

    try {
      // user.get returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<UserData[]>({
        method: 'user.get',
        params: {
          filter: {
            '!%LAST_NAME': 'er',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(`Fetched ${result.length} users:`, result.map(u => `${u.NAME} ${u.LAST_NAME}`))
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
      async function getUsersByLastName() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // user.get returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'user.get',
            params: {
              filter: {
                '!%LAST_NAME': 'er',
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
          console.info(`Fetched ${result.length} users:`, result.map(u => `${u.NAME} ${u.LAST_NAME}`))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getUsersByLastName)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.get(
            filter={
                "!%LAST_NAME": "er",
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
                'user.get',
                [
                    'filter' => [
                        '!%LAST_NAME' => 'er',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting users: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "user.get",
        {
            filter: {
                "!%LAST_NAME": "er"
            }
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
        'user.get',
        [
            'filter' => [
                '!%LAST_NAME' => 'er'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "user.get", b24.Params{
    	"filter": b24.Params{
    		"!%LAST_NAME": "er",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("user.get: %w", err)
    }

    var items []struct {
    	ID         b24.ID `json:"ID"`
    	Active     bool   `json:"ACTIVE"`
    	Name       string `json:"NAME"`
    	LastName   string `json:"LAST_NAME"`
    	SecondName string `json:"SECOND_NAME"`
    	Email      string `json:"EMAIL"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Active)
    }
    ```

{% endlist %}

### Filtering by several cities of residence

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"@PERSONAL_CITY":["Berlin","Munich"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"@PERSONAL_CITY":["Berlin","Munich"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/user.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type UserData = {
      ID: string
      ACTIVE: boolean
      NAME: string
      LAST_NAME: string
      EMAIL: string
      LAST_LOGIN: ISODate | ''
      DATE_REGISTER: ISODate | ''
      IS_ONLINE: string
      PERSONAL_CITY: string
      UF_DEPARTMENT: number[]
      USER_TYPE: string
    }

    try {
      // user.get returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<UserData[]>({
        method: 'user.get',
        params: {
          filter: {
            '@PERSONAL_CITY': ['Berlin', 'Munich'],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(`Fetched ${result.length} users:`, result.map(u => `${u.NAME} ${u.LAST_NAME} (${u.PERSONAL_CITY})`))
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
      async function getUsersByCity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // user.get returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'user.get',
            params: {
              filter: {
                '@PERSONAL_CITY': ['Berlin', 'Munich'],
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
          console.info(`Fetched ${result.length} users:`, result.map(u => `${u.NAME} ${u.LAST_NAME} (${u.PERSONAL_CITY})`))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getUsersByCity)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.get(
            filter={
                "@PERSONAL_CITY": [
                    "Berlin",
                    "Munich",
                ],
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
                'user.get',
                [
                    'filter' => [
                        '@PERSONAL_CITY' => ['Berlin', 'Munich']
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting users: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "user.get",
        {
            filter: {
                "@PERSONAL_CITY": ["Berlin", "Munich"]
            }
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
        'user.get',
        [
            'filter' => [
                '@PERSONAL_CITY' => ['Berlin', 'Munich']
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "user.get", b24.Params{
    	"filter": b24.Params{
    		"@PERSONAL_CITY": []string{"Berlin", "Munich"},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("user.get: %w", err)
    }

    var items []struct {
    	ID         b24.ID `json:"ID"`
    	Active     bool   `json:"ACTIVE"`
    	Name       string `json:"NAME"`
    	LastName   string `json:"LAST_NAME"`
    	SecondName string `json:"SECOND_NAME"`
    	Email      string `json:"EMAIL"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Active)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "1",
            "NAME": "Anna",
            "LAST_NAME": "Schmidt",
            "EMAIL": "a.schmidt@example.com",
            "WORK_POSITION": "Head of Sales",
            "UF_DEPARTMENT": [1, 47],
            "USER_TYPE": "employee"
        },
        {
            "ID": "1271",
            "NAME": "Klaus",
            "LAST_NAME": "Weber",
            "EMAIL": "k.weber@example.com",
            "WORK_POSITION": "Sales Manager",
            "UF_DEPARTMENT": [1],
            "USER_TYPE": "employee"
        }
    ],
    "total": 2,
    "time": {
        "start": 1794316750,
        "finish": 1794316750.180457,
        "duration": 0.18045711517333984,
        "processing": 0,
        "date_start": "2026-11-10T14:19:10+01:00",
        "date_finish": "2026-11-10T14:19:10+01:00",
        "operating_reset_at": 1794317340,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | An array of users, up to 50 records per page [(detailed description)](#user). If there are no users, an empty array is returned ||
|| **total**
[`integer`](../data-types.md) | The total number of users found. Not returned if `result` is empty. Equals `0` when `start = -1` ||
|| **next**
[`integer`](../data-types.md) | The `start` value for the next page. Returned if there are more records than on the current page ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### User Object {#user}

The method returns the fields from `select` or all fields available to the scope version — see the [User Scope Versions](user-scope.md) article for the list for each version. A field with a `null` value is omitted from the response; empty strings are returned. The table lists the main fields; the full list is in the [user field description](user-current.md#user-fields).

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | User ID. A number as a string ||
|| **XML_ID**
[`string`](../data-types.md) | External user code ||
|| **ACTIVE**
[`boolean`](../data-types.md) | Employee status:
- `true` — active
- `false` — dismissed ||
|| **NAME**
[`string`](../data-types.md) | First name ||
|| **LAST_NAME**
[`string`](../data-types.md) | Last name ||
|| **SECOND_NAME**
[`string`](../data-types.md) | Middle name ||
|| **EMAIL**
[`string`](../data-types.md) | E-mail ||
|| **LAST_LOGIN**
[`string`](../data-types.md) | Date and time of the last login in ISO 8601 format, for example `2026-11-09T13:34:45+01:00` ||
|| **DATE_REGISTER**
[`string`](../data-types.md) | Registration date in ISO 8601 format ||
|| **TIME_ZONE**
[`string`](../data-types.md) | User time zone, for example `Europe/Berlin` ||
|| **IS_ONLINE**
[`string`](../data-types.md) | Whether the user is online:
- `Y` — online
- `N` — not online ||
|| **TIMESTAMP_X**
[`object`](../data-types.md) | Date of the last profile change. The method returns an empty object `{}` ||
|| **LAST_ACTIVITY_DATE**
[`object`](../data-types.md) | Date of the last activity. The method returns an empty object `{}` ||
|| **PERSONAL_GENDER**
[`string`](../data-types.md) | Gender:
- `M` — male
- `F` — female
- empty string — not specified ||
|| **PERSONAL_BIRTHDAY**
[`string`](../data-types.md) | Date of birth in ISO 8601 format. An empty string if not specified ||
|| **PERSONAL_PHOTO**
[`string`](../data-types.md) | Profile photo link. The size of the copy is set by the `IMAGE_RESIZE` parameter ||
|| **PERSONAL_MOBILE**
[`string`](../data-types.md) | Personal mobile ||
|| **PERSONAL_CITY**
[`string`](../data-types.md) | Residential city ||
|| **WORK_PHONE**
[`string`](../data-types.md) | Company phone ||
|| **WORK_POSITION**
[`string`](../data-types.md) | Job title ||
|| **UF_EMPLOYMENT_DATE**
[`string`](../data-types.md) | Hiring date. An empty string if not specified ||
|| **UF_DEPARTMENT**
[`array`](../data-types.md) | An array of the user's department IDs ||
|| **UF_PHONE_INNER**
[`string`](../data-types.md) | Internal phone number ||
|| **USER_TYPE**
[`string`](../data-types.md) | User type:
- `employee` — employee
- `extranet` — extranet user ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Invalid order \"ZZZ\"",
    "argument": ""
}
```

{% include notitle [Error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_ARGUMENT` | Invalid order "`value`" | The `order` parameter contains a value other than `ASC` or `DESC` ||
|| `400` | `100` | Unknown field definition `FIELD` (`FIELD`) for \Bitrix\Intranet\User Entity. | The `sort` parameter contains a field that the user does not have ||
|| `401` | `insufficient_scope` | The request requires higher privileges than provided by the webhook token | The webhook or application has none of the scope versions: `user`, `user_basic`, or `user_brief` ||
|| `401` | `INVALID_CREDENTIALS` | Invalid request credentials | Invalid webhook code or authorization token ||
|#

The method ignores an invalid `filter` or an unknown field in it without an error and returns the list without that condition.

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-add.md)
- [{#T}](./user-update.md)
- [{#T}](./user-current.md)
- [{#T}](./user-search.md)
- [{#T}](./user-fields.md)
- [{#T}](../departments/department-get.md)
- [{#T}](../../tutorials/crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md)
- [{#T}](../../tutorials/crm/how-to-add-crm-objects/how-to-send-email.md)
- [{#T}](../../tutorials/crm/how-to-get-lists/get-activity-list-by-deals.md)
