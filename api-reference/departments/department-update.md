# Update Department department.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: user with permissions to modify the structure

The `department.update` method updates a department in the company structure. Fields that are not passed remain unchanged.

The method belongs to the previous API version. In REST 3.0, the name is changed by [humanresources.node.edit](./node/humanresources-node-edit.md), the department is moved by [humanresources.node.move](./node/humanresources-node-move.md), and the head is assigned by [participant methods](./node-member/index.md). Method correspondence is described in the [section overview](./index.md#v2-v3).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../data-types.md) | Identifier of the department.

The identifier can be retrieved using [department.get](./department-get.md) ||
|| **NAME**
[`string`](../data-types.md) | Name of the department ||
|| **SORT**
[`integer`](../data-types.md) | Sorting field of the department ||
|| **PARENT**
[`integer`](../data-types.md) | Identifier of the parent department.

Pass a number. If a non-numeric string is passed, including an empty one, the method returns the `PARENT must be numeric.` error ||
|| **UF_HEAD**
[`integer`](../data-types.md) | Identifier of the user who will be the head of the department. To remove the head, pass `0`.

The user identifier can be retrieved using [user.get](../user/user-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "ID": 18,
        "NAME": "Regional Sales Department",
        "SORT": 500
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/department.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "ID": 18,
        "NAME": "Regional Sales Department",
        "SORT": 500,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/department.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'department.update',
        params: {
          ID: 18,
          NAME: 'Regional Sales Department',
          SORT: 500,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated:', result)
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
      async function updateDepartment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'department.update',
            params: {
              ID: 18,
              NAME: 'Regional Sales Department',
              SORT: 500,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateDepartment)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.department.update(
            bitrix_id=18,
            name="Regional Sales Department",
            sort=500,
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
                'department.update',
                [
                    'ID'   => 18,
                    'NAME' => 'Regional Sales Department',
                    'SORT' => 500,
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
        'department.update',
        {
            ID: 18,
            NAME: 'Regional Sales Department',
            SORT: 500,
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
        'department.update',
        [
            'ID'   => 18,
            'NAME' => 'Regional Sales Department',
            'SORT' => 500,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "department.update", b24.Params{
    	"ID":   18,
    	"NAME": "Regional Sales Department",
    	"SORT": 500,
    })
    if err != nil {
    	return fmt.Errorf("department.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1736929002.119324,
        "finish": 1736929002.469626,
        "duration": 0.35030198097229004,
        "processing": 0.13488411903381348,
        "date_start": "2025-01-15T08:16:42+00:00",
        "date_finish": "2025-01-15T08:16:42+00:00",
        "operating": 0.13484978675842285
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if the department is updated. On failure, the method returns an error ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "510",
    "error_description": "Department not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `510` | Department not found | `ID` is not passed, or there is no department with this identifier ||
|| `400` | `ERROR_ARGUMENT` | PARENT must be numeric. | A non-numeric string, including an empty one, is passed in `PARENT` ||
|| `400` | Empty value | `Incorrect parent section!<br>` | There is no department with the identifier from `PARENT` ||
|| `400` | `ERROR_CORE` | The company structure may contain only one top level section. | `0` is passed in `PARENT`. There can be only one top-level department in the company structure ||
|| `403` | `ACCESS_DENIED` | Access denied! | The user does not have permission to change the company structure ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./department-add.md)
- [{#T}](./department-get.md)
- [{#T}](./department-delete.md)
- [{#T}](./department-fields.md)