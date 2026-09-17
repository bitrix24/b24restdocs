# Get Source by ID biconnector.source.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

The method `biconnector.source.get` returns information about the source by its identifier.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and returns only the sources that the application created itself. When called via a webhook, the method returns the `ACCESS_DENIED` error

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the source, which can be obtained using the methods [biconnector.source.list](./biconnector-source-list.md) and [biconnector.source.add](./biconnector-source-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/biconnector.source.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Methods of this section put errors inside result and answer with HTTP 200
    type BiconnectorError = {
      error: {
        error: string
        error_description: string
      }
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SourceGetResult = {
      item: {
        connection: {
          id: number
          type: string
          code: string
          title: string
          description: string
          active: boolean
          // Dates come as "2025-03-20 14:50:06", not as an ISO string
          dateCreate: string | null
          dateUpdate: string | null
          createdById: number
          updatedById: number
        }
        connectorId: number
        settings: Array<{
          code: string
          name: string
          type: string
          value: string
          id: number
        }>
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<SourceGetResult | BiconnectorError>({
        method: 'biconnector.source.get',
        params: {
          id: 6,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result

        // The SDK sees HTTP 200 as success, so check the error inside result yourself
        if ('error' in result) {
          console.error(result.error.error, result.error.error_description)
        } else {
          console.info(result.item.connection.id, result.item.connection.title, result.item.settings)
        }
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
      async function getSource() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.source.get',
            params: {
              id: 6,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result

          // The SDK sees HTTP 200 as success, so check the error inside result yourself
          if (result && result.error) {
            console.error(result.error.error, result.error.error_description)
            return
          }

          console.info(result.item.connection.id, result.item.connection.title, result.item.settings)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSource)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.biconnector.source.get(
            bitrix_id=6,
        ).response
        result = bitrix_response.result

        # Methods of this section put errors inside result and answer with HTTP 200
        if isinstance(result, dict) and "error" in result:
            print(
                "BIconnector error",
                f"error: {result['error']['error']}",
                f"error_description: {result['error']['error_description']}",
                sep="\n",
            )
        else:
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
                'biconnector.source.get',
                [
                    'id' => 6,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            $data = $result->data();

            // Methods of this section put errors inside result and answer with HTTP 200
            if (isset($data['error'])) {
                echo 'BIconnector error: ' . $data['error']['error'] . ': ' . $data['error']['error_description'];
            } else {
                echo 'Data: ' . print_r($data, true);
            }
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting source: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.source.get',
        {
            id: 6,
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            const data = result.data();

            // Methods of this section put errors inside result and answer with HTTP 200
            if (data && data.error) {
                console.error(data.error.error, data.error.error_description);
                return;
            }

            console.info(data);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.source.get',
        [
            'id' => 6
        ]
    );

    // Methods of this section put errors inside result and answer with HTTP 200
    if (isset($result['result']['error'])) {
        echo 'BIconnector error: ' . $result['result']['error']['error']
            . ': ' . $result['result']['error']['error_description'];
    } else {
        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "biconnector.source.get", b24.Params{
    	"id": 6,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("biconnector.source.get: %w", err)
    }

    // Methods of this section put errors inside result and answer with HTTP 200.
    var apiErr struct {
    	Error *struct {
    		Error       string `json:"error"`
    		Description string `json:"error_description"`
    	} `json:"error"`
    }
    if err := json.Unmarshal(res.Result, &apiErr); err == nil && apiErr.Error != nil {
    	return fmt.Errorf("biconnector.source.get: %s: %s", apiErr.Error.Error, apiErr.Error.Description)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ConnectorID b24.ID `json:"connectorId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ConnectorID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "item": {
            "connection": {
                "id": 6,
                "type": "rest",
                "code": "rest_1",
                "title": "Rest source SQL",
                "description": "Connection for working with mySql",
                "active": true,
                "dateCreate": "2025-03-20 14:50:06",
                "dateUpdate": "2025-03-20 14:50:06",
                "createdById": 1,
                "updatedById": 1
            },
            "connectorId": 1,
            "settings": [
                {
                    "code": "token",
                    "name": "Token",
                    "type": "STRING",
                    "value": "a1b2c3d4e5",
                    "id": 8
                }
            ]
        }
    },
    "time": {
        "start": 1742929480.368097,
        "finish": 1742929480.449558,
        "duration": 0.08146095275878906,
        "processing": 0.006555080413818359,
        "date_start": "2025-03-25T19:04:40+00:00",
        "date_finish": "2025-03-25T19:04:40+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. It contains the single `item` key ||
|| **result.item**
[`object`](../../data-types.md) | Source data [(detailed description)](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### The item object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **connection**
[`object`](../../data-types.md) | Connection data: the source's own fields [(detailed description)](#connection) ||
|| **connectorId**
[`integer`](../../data-types.md) | Identifier of the connector the source is linked to ||
|| **settings**
[`array`](../../data-types.md) | Authorization parameters of the source — one object per connector parameter [(detailed description)](#settings) ||
|#

#### The connection object {#connection}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the source ||
|| **type**
[`string`](../../data-types.md) | Source type. For sources created via REST, the value is always `rest` ||
|| **code**
[`string`](../../data-types.md) | Source code. It is generated automatically using the `rest_<connectorId>` template ||
|| **title**
[`string`](../../data-types.md) | Source name ||
|| **description**
[`string`](../../data-types.md) | Source description ||
|| **active**
[`boolean`](../../data-types.md) | Source activity. An inactive source stops returning data ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date the source was created, in the `Y-m-d H:i:s` format ||
|| **dateUpdate**
[`datetime`](../../data-types.md) | Date the source was updated, in the `Y-m-d H:i:s` format ||
|| **createdById**
[`integer`](../../data-types.md) | Identifier of the user who created the source ||
|| **updatedById**
[`integer`](../../data-types.md) | Identifier of the user who updated the source ||
|#

#### The settings object {#settings}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the saved parameter ||
|| **code**
[`string`](../../data-types.md) | Parameter code set by the connector ||
|| **name**
[`string`](../../data-types.md) | Parameter name that the user sees in the interface ||
|| **type**
[`string`](../../data-types.md) | Parameter type: `STRING` or `INT` ||
|| **value**
[`string`](../../data-types.md) | Value specified when the source was created or updated. It arrives in plain text, including passwords and tokens ||
|#

## Error Handling

HTTP Status: **200**

```json
{
    "result": {
        "error": {
            "error": "VALIDATION_ID_NOT_PROVIDED",
            "error_description": "ID is missing."
        }
    }
}
```

{% note warning "" %}

The method returns an error [inside the `result` field](../index.md#errors) and with HTTP status 200. Check `result.error`: the SDK wrappers parse only the top level of the response and treat such an error as a success

{% endnote %}

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | Access denied. | One of the two permissions is missing, or the method was called via a webhook or outside the application context ||
|| `VALIDATION_ID_NOT_PROVIDED` | ID is missing. | Identifier is not provided ||
|| `VALIDATION_INVALID_ID_FORMAT` | ID has to be a positive integer. | Invalid ID format ||
|| `SOURCE_NOT_FOUND` | Source was not found. | The source does not exist or belongs to another application ||
|| `CONNECTOR_NOT_FOUND` | Connector was not found. | The connector the source is linked to was not found ||

|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-source-add.md)
- [{#T}](./biconnector-source-update.md)
- [{#T}](./biconnector-source-list.md)
- [{#T}](./biconnector-source-delete.md)
- [{#T}](./biconnector-source-fields.md)
