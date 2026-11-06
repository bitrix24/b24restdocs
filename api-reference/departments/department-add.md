# Create Department department.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`department`](../scopes/permissions.md)
>
> Who can execute the method: user with rights to modify the structure

The `department.add` method creates a department in the company structure and returns its identifier. The new department belongs to the parent department specified in `PARENT`.

The method belongs to the previous API version. The REST 3.0 counterpart is [humanresources.node.add](./node/humanresources-node-add.md); method correspondence is described in the [section overview](./index.md#v2-v3).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../data-types.md) | Department name ||
|| **SORT**
[`integer`](../data-types.md) | Department sorting field ||
|| **PARENT***
[`integer`](../data-types.md) | Identifier of the parent department.

Pass a number. If the parameter is not passed, the method returns the `Argument 'PARENT' is null or empty` error. If a non-numeric string is passed, including an empty one, the method returns the `PARENT must be numeric.` error

The department identifier can be retrieved using [department.get](./department-get.md) ||
|| **UF_HEAD**
[`integer`](../data-types.md) | Identifier of the user who will become the head of the department. The method does not check whether such a user exists in Bitrix24: with a non-existent identifier, the department is created without an error, and this identifier is retained in `UF_HEAD`.

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
        "NAME": "Regional Sales Department",
        "SORT": 450,
        "UF_HEAD": 1,
        "PARENT": 1
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/department.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "NAME": "Regional Sales Department",
        "SORT": 450,
        "UF_HEAD": 1,
        "PARENT": 1,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/department.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'department.add',
        params: {
          NAME: 'Regional Sales Department',
          SORT: 450,
          UF_HEAD: 1,
          PARENT: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created department ID:', result)
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
      async function addDepartment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'department.add',
            params: {
              NAME: 'Regional Sales Department',
              SORT: 450,
              UF_HEAD: 1,
              PARENT: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created department ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addDepartment)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.department.add(
            name="Regional Sales Department",
            sort=450,
            uf_head=1,
            parent=1,
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
                'department.add',
                [
                    'NAME'    => 'Regional Sales Department',
                    'SORT'    => 450,
                    'UF_HEAD' => 1,
                    'PARENT'  => 1,
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
        'department.add',
        {
            NAME: 'Regional Sales Department',
            SORT: 450,
            UF_HEAD: 1,
            PARENT: 1,
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
        'department.add',
        [
            'NAME'    => 'Regional Sales Department',
            'SORT'    => 450,
            'UF_HEAD' => 1,
            'PARENT'  => 1,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "department.add", b24.Params{
    	"NAME":    "Regional Sales Department",
    	"SORT":    450,
    	"UF_HEAD": 1,
    	"PARENT":  1,
    })
    if err != nil {
    	return fmt.Errorf("department.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 18,
    "time": {
        "start": 1736927311.779587,
        "finish": 1736927312.132503,
        "duration": 0.35291600227355957,
        "processing": 0.17050600051879883,
        "date_start": "2025-01-15T07:48:31+00:00",
        "date_finish": "2025-01-15T07:48:32+00:00",
        "operating": 0.1704881191253662
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the created department ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Incorrect parent section!<br>"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Section name is not specified.<br>` | The `NAME` parameter is not passed ||
|| `400` | `ERROR_ARGUMENT` | Argument 'PARENT' is null or empty | The required `PARENT` parameter is not passed. The response contains the `argument` field with the `PARENT` value ||
|| `400` | `ERROR_ARGUMENT` | PARENT must be numeric. | A non-numeric string, including an empty one, is passed in `PARENT` ||
|| `400` | Empty value | `Incorrect parent section!<br>` | There is no department with the identifier from `PARENT` ||
|| `400` | `ERROR_CORE` | The company structure may contain only one top level section. | `0` is passed in `PARENT`. There can be only one top-level department in the company structure ||
|| `403` | `ACCESS_DENIED` | Access denied! | The user does not have permission to change the company structure ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./department-update.md)
- [{#T}](./department-get.md)
- [{#T}](./department-delete.md)
- [{#T}](./department-fields.md)