# Get Table by ID biconnector.table.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

The `biconnector.table.get` method returns information about a table by its identifier.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and returns only the tables that the application created itself. When called via a webhook, the method returns the `ACCESS_DENIED` error

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Table identifier, can be obtained with the [biconnector.table.list](./biconnector-table-list.md) and [biconnector.table.add](./biconnector-table-add.md) methods ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/biconnector.table.get
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
    type TableGetResult = {
      item: {
        id: number
        type: string
        name: string
        description: string
        externalCode: string
        externalName: string
        dateCreate: string
        dateUpdate: string
        createdById: number
        updatedById: number
        externalId: number
        csvDelimiter: string
        csvEncoding: string
        csvHasHeaders: boolean
        fields: Array<{
          id: number
          datasetId: number
          type: string
          name: string
          externalCode: string
          visible: boolean
          description: string
        }>
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<TableGetResult | BiconnectorError>({
        method: 'biconnector.table.get',
        params: {
          id: 2,
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
          console.info(result.item.id, result.item.name, result.item.type)
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
      async function getTable() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.table.get',
            params: {
              id: 2,
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

          console.info(result.item.id, result.item.name, result.item.type)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTable)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        # b24pysdk has no ready-made wrapper for biconnector.table.*, so the method
        # is called directly through bitrix_token.call_method()
        response = bitrix_token.call_method(
            api_method="biconnector.table.get",
            params={
                "id": 2,
            },
        )
        result = response["result"]

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
                'biconnector.table.get',
                [
                    'id' => 2,
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
        echo 'Error getting table: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.table.get',
        {
            id: 2,
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
        'biconnector.table.get',
        [
            'id' => 2
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
    res, err := client.Core().Call(ctx, "biconnector.table.get", b24.Params{
    	"id": 2,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("biconnector.table.get: %w", err)
    }

    // Methods of this section put errors inside result and answer with HTTP 200.
    var apiErr struct {
    	Error *struct {
    		Error       string `json:"error"`
    		Description string `json:"error_description"`
    	} `json:"error"`
    }
    if err := json.Unmarshal(res.Result, &apiErr); err == nil && apiErr.Error != nil {
    	return fmt.Errorf("biconnector.table.get: %s: %s", apiErr.Error.Error, apiErr.Error.Description)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID           b24.ID `json:"id"`
    	Type         string `json:"type"`
    	Name         string `json:"name"`
    	Description  string `json:"description"`
    	ExternalCode string `json:"externalCode"`
    	ExternalName string `json:"externalName"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Type)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 27,
            "type": "rest",
            "name": "sales_orders",
            "description": "Orders from an external service",
            "externalCode": "sales_orders",
            "externalName": "Sales orders",
            "dateCreate": "2026-09-16 12:00:46",
            "dateUpdate": null,
            "createdById": 1,
            "updatedById": 0,
            "externalId": 0,
            "csvDelimiter": "",
            "csvEncoding": "",
            "csvHasHeaders": false,
            "fields": [
                {"id": 43, "datasetId": 27, "type": "int", "name": "ID", "externalCode": "id", "visible": true, "description": ""},
                {"id": 45, "datasetId": 27, "type": "string", "name": "CUSTOMER", "externalCode": "customer", "visible": true, "description": ""},
                {"id": 47, "datasetId": 27, "type": "money", "name": "AMOUNT", "externalCode": "amount", "visible": true, "description": ""},
                {"id": 49, "datasetId": 27, "type": "date", "name": "ORDER_DATE", "externalCode": "order_date", "visible": true, "description": ""}
            ]
        }
    },
    "time": {
        "start": 1789549489,
        "finish": 1789549489.125967,
        "duration": 0.12596702575683594,
        "processing": 0,
        "date_start": "2026-09-16T12:04:49+03:00",
        "date_finish": "2026-09-16T12:04:49+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. It contains the single `item` key with the [table](#table) object ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### The table object {#table}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Table identifier ||
|| **type**
[`string`](../../data-types.md) | Table type. For tables of a REST source the value is always `rest` ||
|| **name**
[`string`](../../data-types.md) | Table name ||
|| **description**
[`string`](../../data-types.md) | Table description ||
|| **externalCode**
[`string`](../../data-types.md) | External code of the table ||
|| **externalName**
[`string`](../../data-types.md) | External name of the table ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Creation date in the `Y-m-d H:i:s` format ||
|| **dateUpdate**
[`datetime`](../../data-types.md) | Update date in the `Y-m-d H:i:s` format. For a table that has never been updated, the value is `null` ||
|| **createdById**
[`integer`](../../data-types.md) | Identifier of the user who created the table ||
|| **updatedById**
[`integer`](../../data-types.md) | Identifier of the user who updated the table. For a table that has never been updated, the value is `0` ||
|| **externalId**
[`integer`](../../data-types.md) | Identifier of the BI Builder dataset created together with the object by the deprecated `biconnector.dataset.add` method. For tables created with the [biconnector.table.add](./biconnector-table-add.md) method, the value is always `0` ||
|| **csvDelimiter**, **csvEncoding**, **csvHasHeaders**
[`string`](../../data-types.md), [`string`](../../data-types.md), [`boolean`](../../data-types.md) | Parameters for parsing a CSV file. For tables of a REST source they are always empty ||
|| **fields**
[`array`](../../data-types.md) | Array of table [columns](#field) ||
|#

There is no `sourceId` field in the response — the source identifier is returned only by the [biconnector.table.list](./biconnector-table-list.md) method. The composition of columns, on the contrary, is available only here: the [biconnector.table.list](./biconnector-table-list.md) selection carries no `fields` array.

#### Element of the fields array {#field}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Column identifier ||
|| **datasetId**
[`integer`](../../data-types.md) | Identifier of the table the column belongs to. The key is named this way for historical reasons ||
|| **type**
[`string`](../../data-types.md) | [Data type](./index.md#fields) of the column ||
|| **name**
[`string`](../../data-types.md) | Column name ||
|| **externalCode**
[`string`](../../data-types.md) | External code of the column ||
|| **visible**
[`boolean`](../../data-types.md) | Column visibility flag ||
|| **description**
[`string`](../../data-types.md) | Column description. It is not filled in via the REST API ||
|#

## Error Handling

HTTP status: **200**

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
|| `DATASET_NOT_FOUND` | Dataset was not found. | The table does not exist or belongs to another application ||

|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-table-add.md)
- [{#T}](./biconnector-table-update.md)
- [{#T}](./biconnector-table-list.md)
- [{#T}](./biconnector-table-delete.md)
- [{#T}](./biconnector-table-fields-update.md)
- [{#T}](./biconnector-table-fields.md)