# Get a list of custom fields user.userfield.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `user.userfield.list` method retrieves a list of custom fields by filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`string`](../../data-types.md)\|[`array`](../../data-types.md) | Sorting of selected custom fields in `{"field_1": "order_1", ... "field_N": "order_N"}` format.

Possible values for `field_N`:

- `ID` — custom field identifier
- `FIELD_NAME` — field name
- `USER_TYPE_ID` — field data type
- `XML_ID` — external code
- `SORT` — field sorting order
- `MULTIPLE` — multi-value capability
- `MANDATORY` — mandatory field
- `SHOW_FILTER` — show field in user list filter
- `SHOW_IN_LIST` — show field in user list
- `EDIT_IN_LIST` — field editing capability in user list
- `IS_SEARCHABLE` — field search capability

Possible values for `order_N`:

- `asc` — ascending
- `desc` — descending
 ||
|| **filter** 
[`array`](../../data-types.md)| Filter of selected custom fields in `{"field_1": "value_1", ... "field_N": "value_N"}` format.

Possible values for `field_N` are similar to the fields in sorting.

A key can be assigned an additional prefix to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as a value)
- `!@` — NOT IN (an array is passed as a value)
- `%` — LIKE, substring search. The `%` character does not need to be passed in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character must be passed in the value. Examples:
  - `"mol%"` — searching for values starting with "mol"
  - `"%mol"` — searching for values ending with "mol"
  - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character does not need to be passed in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` character must be passed in the value. Examples:
  - `"mol%"` — searching for values not starting with "mol"
  - `"%mol"` — searching for values not ending with "mol"
  - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"id":"desc"},"filter":{"id":13}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.userfield.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"id":"desc"},"filter":{"id":13},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/user.userfield.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each user field returned in result[]
    type UserUserfieldItem = {
      ID: string
      ENTITY_ID: string
      FIELD_NAME: string
      USER_TYPE_ID: string
      XML_ID: string | null
      SORT: string
      MULTIPLE: 'Y' | 'N'
      MANDATORY: 'Y' | 'N'
      SHOW_FILTER: string
      SHOW_IN_LIST: 'Y' | 'N'
      EDIT_IN_LIST: 'Y' | 'N'
      IS_SEARCHABLE: 'Y' | 'N'
      SETTINGS: Record<string, unknown>
      LIST?: Array<{ ID: string; SORT: string; VALUE: string; DEF: 'Y' | 'N'; XML_ID: string }>
    }

    // user.userfield.list returns a single page (max 50 records). For the whole result set
    // use a list helper: $b24.actions.v2.callList.make() returns every record as one
    // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
    // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
    // passing it is a TS error) — keep this call.make + `start` variant when sort matters.

    try {
      const response = await $b24.actions.v2.call.make<UserUserfieldItem[]>({
        method: 'user.userfield.list',
        params: {
          order: { id: 'desc' },
          filter: { id: 13 },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Loaded user fields:', result.length, result)
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
      async function listUserUserfields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // user.userfield.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.

          const response = await $b24.actions.v2.call.make({
            method: 'user.userfield.list',
            params: {
              order: { id: 'desc' },
              filter: { id: 13 },
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
          console.info('Loaded user fields:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listUserUserfields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'user.userfield.list',
                [
                    'order' => [
                        'id' => 'desc',
                    ],
                    'filter' => [
                        'id' => 13
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // The data processing logic you need
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching user fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "user.userfield.list",
        {
            order: {
                id: 'desc',
            },
            filter: {
                id: 13
            },
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.userfield.list',
        [
            'order' => [
                'id' => 'desc',
            ]
            'filter' => [
                'id' => 13,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    order = {
        "ID": "desc",
        "SORT": "asc",
    }
    filter = {
        "USER_TYPE_ID": "string",
        "SHOW_IN_LIST": "Y",
    }

    try:
        bitrix_response = client.user.userfield.list(
            order=order,
            filter=filter,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print("Bitrix SDK error", error.message, sep="\n")
    except Exception as error:
        print("Unexpected error", error, sep="\n")
    ```

    Example `as_list`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    order = {
        "ID": "desc",
    }
    filter = {
        "SHOW_IN_LIST": "Y",
    }

    try:
        bitrix_response = client.user.userfield.list(
            order=order,
            filter=filter,
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print("Bitrix SDK error", error.message, sep="\n")
    except Exception as error:
        print("Unexpected error", error, sep="\n")
    ```

    Example `as_list_fast`

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    order = {
        "ID": "desc",
    }
    filter = {
        "SHOW_IN_LIST": "Y",
    }

    try:
        bitrix_response = client.user.userfield.list(
            order=order,
            filter=filter,
        ).as_list_fast(descending=True).response
        result = bitrix_response.result
        for item in result:
            print(item)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print("Bitrix SDK error", error.message, sep="\n")
    except Exception as error:
        print("Unexpected error", error, sep="\n")
    ```
{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result":[
        {
            "ID":"176",
            "ENTITY_ID":"USER",
            "FIELD_NAME":"UF_USR_1724228142162",
            "USER_TYPE_ID":"enumeration",
            "XML_ID":null,
            "SORT":"100",
            "MULTIPLE":"Y",
            "MANDATORY":"N",
            "SHOW_FILTER":"E",
            "SHOW_IN_LIST":"Y",
            "EDIT_IN_LIST":"Y",
            "IS_SEARCHABLE":"N",
            "SETTINGS":{
                "DISPLAY":"UI",
                "LIST_HEIGHT":1,
                "CAPTION_NO_VALUE":"",
                "SHOW_NO_VALUE":"Y"
            },
            "LIST":[
                {
                    "ID":"26",
                    "SORT":"0",
                    "VALUE":"1",
                    "DEF":"N",
                    "XML_ID":"2a53ce08b86a6aba152b1b19204fdef2"
                },
                {
                    "ID":"27",
                    "SORT":"100",
                    "VALUE":"2",
                    "DEF":"N",
                    "XML_ID":"292df2be1171ed6ab038996deac29ac8"
                },
                {
                    "ID":"28",
                    "SORT":"200",
                    "VALUE":"3",
                    "DEF":"N",
                    "XML_ID":"3c5af70eafbba79a6cf52e299ed75123"
                }
            ]
        },
        {
            "ID":"177",
            "ENTITY_ID":"USER",
            "FIELD_NAME":"UF_USR_MONEY",
            "USER_TYPE_ID":"money",
            "XML_ID":null,
            "SORT":"100",
            "MULTIPLE":"N",
            "MANDATORY":"N",
            "SHOW_FILTER":"N",
            "SHOW_IN_LIST":"Y",
            "EDIT_IN_LIST":"Y",
            "IS_SEARCHABLE":"N",
            "SETTINGS":{
                "DEFAULT_VALUE":""
            }
        }
    ],
    "total":2,
    "time":{
        "start":1747313326.788124,
        "finish":1747313328.641663,
        "duration":1.853538990020752,
        "processing":0.011211156845092773,
        "date_start":"2025-05-15T14:48:46+02:00",
        "date_finish":"2025-05-15T14:48:48+02:00",
        "operating":0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Response root element ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Query execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{	
   "error":"",
   "error_description":"Access denied."
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | A field with this `id` does not exist or access is denied ||
|| Empty string | ID is not defined or invalid | `id` is not set or is invalid ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-userfield-add.md)
- [{#T}](./user-userfield-update.md)
- [{#T}](./user-userfield-delete.md)