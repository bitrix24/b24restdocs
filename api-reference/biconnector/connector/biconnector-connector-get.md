# Get Connector by ID biconnector.connector.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

The method `biconnector.connector.get` returns information about the connector by its identifier.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and returns only the connectors that the application created itself. When called via a webhook, the method returns the `ACCESS_DENIED` error

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | The identifier of the connector, which can be obtained using the methods [biconnector.connector.list](./biconnector-connector-list.md) and [biconnector.connector.add](./biconnector-connector-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "id": 4,
             "auth": "**put_access_token_here**"
             }' \
         https://**put_your_bitrix24_address**/rest/biconnector.connector.get
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
    type ConnectorGetResult = {
      item: {
        id: number
        title: string
        dateCreate: string // Y-m-d H:i:s, not ISO 8601
        logo: string
        description: string
        sort: number
        urlCheck: string
        settings: Array<{
          name: string
          code: string
          type: string
        }>
        urlData: string
        urlTableList: string
        urlTableDescription: string
        supportMapping: boolean
        sourceCode: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ConnectorGetResult | BiconnectorError>({
        method: 'biconnector.connector.get',
        params: {
          id: 4,
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
          console.info(result.item.id, result.item.title, result.item.settings)
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
      async function getConnector() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.connector.get',
            params: {
              id: 4,
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

          console.info(result.item.id, result.item.title, result.item.settings)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getConnector)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.biconnector.connector.get(
            bitrix_id=4,
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
                'biconnector.connector.get',
                [
                    'id' => 4,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        $data = $result;

        // Methods of this section put errors inside result and answer with HTTP 200
        if (isset($data['error'])) {
            echo 'BIconnector error: ' . $data['error']['error'] . ': ' . $data['error']['error_description'];
        } else {
            echo 'Info: ' . print_r($data, true);
        }
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.connector.get',
        {
            id: 4,
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
        'biconnector.connector.get',
        [
            'id' => 4
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
    res, err := client.Core().Call(ctx, "biconnector.connector.get", b24.Params{
    	"id": 4,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("biconnector.connector.get: %w", err)
    }

    // Methods of this section put errors inside result and answer with HTTP 200.
    var apiErr struct {
    	Error *struct {
    		Error       string `json:"error"`
    		Description string `json:"error_description"`
    	} `json:"error"`
    }
    if err := json.Unmarshal(res.Result, &apiErr); err == nil && apiErr.Error != nil {
    	return fmt.Errorf("biconnector.connector.get: %s: %s", apiErr.Error.Error, apiErr.Error.Description)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID          b24.ID `json:"id"`
    	Title       string `json:"title"`
    	DateCreate  string `json:"dateCreate"`
    	Logo        string `json:"logo"`
    	Description string `json:"description"`
    	Sort        int    `json:"sort"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Title)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "item": {
            "id": 4,
            "title": "SUPER REST CONNECTOR",
            "dateCreate": "2025-03-24 07:25:59",
            "logo": "https://app.domain/logo.png",
            "description": "Connector with token",
            "sort": 100,
            "urlCheck": "http://app.domain/check",
            "settings": [
                {
                    "name": "Login",
                    "code": "login",
                    "type": "STRING"
                },
                {
                    "name": "Password",
                    "code": "password",
                    "type": "STRING"
                }
            ],
            "urlData": "http://app.domain/data",
            "urlTableList": "http://app.domain/table_list",
            "urlTableDescription": "http://app.domain/table_description",
            "supportMapping": false,
            "sourceCode": ""
        }
    },
    "time": {
        "start": 1725365418.056843,
        "finish": 1725365419.671506,
        "duration": 1.6146628856658936,
        "processing": 1.3475170135498047,
        "date_start": "2024-09-03T14:10:18+02:00",
        "date_finish": "2024-09-03T14:10:19+02:00"
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
[`object`](../../data-types.md) | Connector data [(detailed description)](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### The item object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the connector ||
|| **title**
[`string`](../../data-types.md) | Connector name ||
|| **logo**
[`string`](../../data-types.md) | Logo URL or a base64 string ||
|| **description**
[`string`](../../data-types.md) | Connector description ||
|| **sort**
[`integer`](../../data-types.md) | Sorting order. If the value was not passed on creation, `100` is returned ||
|| **urlCheck**
[`string`](../../data-types.md) | [Endpoint for checking the connection](./index.md#urlCheck) ||
|| **urlData**
[`string`](../../data-types.md) | [Endpoint for retrieving data](./index.md#urlData) ||
|| **urlTableList**
[`string`](../../data-types.md) | [Endpoint for retrieving the list of tables](./index.md#urlTableList) ||
|| **urlTableDescription**
[`string`](../../data-types.md) | [Endpoint for retrieving the table description](./index.md#urlTableDescription) ||
|| **settings**
[`array`](../../data-types.md) | Array of connection parameters. Each element contains the `code` and `name` fields of the [`string`](../../data-types.md) type and the `type` field with the `STRING` or `INT` value, [(detailed description)](./index.md#settings). The parameter values are retained by the source, while the connector returns only their codes and names ||
|| **supportMapping**
[`boolean`](../../data-types.md) | Support for mapping the table fields to the fields of the external system ||
|| **sourceCode**
[`string`](../../data-types.md) | String code of the external system. If the code was not set, the field is returned as an empty string ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date the connector was created, in the `Y-m-d H:i:s` format ||
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
|| `CONNECTOR_NOT_FOUND` | Connector was not found. | The connector does not exist or belongs to another application ||

|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-connector-add.md)
- [{#T}](./biconnector-connector-update.md)
- [{#T}](./biconnector-connector-list.md)
- [{#T}](./biconnector-connector-delete.md)
- [{#T}](./biconnector-connector-fields.md)
