# Delete Table biconnector.table.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions


The `biconnector.table.delete` method deletes an existing table. Its fields, settings, and link to the source are deleted together with the table.


{% note info "" %}

The method does not touch the datasets built on the table. If the table had a dataset, after the table is deleted the dataset remains in BI Builder, in the Analytics hub > Unused elements section, and can be deleted only manually

{% endnote %}

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and deletes only the tables that the application created itself. When called via a webhook, the method returns the `ACCESS_DENIED` error

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Table identifier, can be obtained with the [biconnector.table.list](./biconnector-table-list.md) or [biconnector.table.add](./biconnector-table-add.md) method ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/biconnector.table.delete
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

    try {
      const response = await $b24.actions.v2.call.make<boolean | BiconnectorError>({
        method: 'biconnector.table.delete',
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
        if (typeof result === 'object' && result !== null && 'error' in result) {
          console.error(result.error.error, result.error.error_description)
        } else {
          console.info('Table deleted:', result)
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
      async function deleteTable() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.table.delete',
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

          console.info('Table deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteTable)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        # b24pysdk has no ready-made wrapper for biconnector.table.*, so the method
        # is called directly through bitrix_token.call_method()
        response = bitrix_token.call_method(
            api_method="biconnector.table.delete",
            params={
                "id": 4,
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
                'biconnector.table.delete',
                [
                    'id' => 4,
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
                echo 'Info: ' . print_r($data, true);
            }
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting table: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.table.delete',
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
        'biconnector.table.delete',
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
    res, err := client.Core().Call(ctx, "biconnector.table.delete", b24.Params{
    	"id": 4,
    })
    if err != nil {
    	return fmt.Errorf("biconnector.table.delete: %w", err)
    }

    // Methods of this section put errors inside result and answer with HTTP 200.
    var apiErr struct {
    	Error *struct {
    		Error       string `json:"error"`
    		Description string `json:"error_description"`
    	} `json:"error"`
    }
    if err := json.Unmarshal(res.Result, &apiErr); err == nil && apiErr.Error != nil {
    	return fmt.Errorf("biconnector.table.delete: %s: %s", apiErr.Error.Error, apiErr.Error.Description)
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
[`boolean`](../../data-types.md) | Root element of the response. It contains `true` if the table was deleted. The method returns no other value on success and does not return the object of the deleted table. If the table could not be deleted, an object with the `error` key arrives in `result` instead of `true` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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
|| `VALIDATION_ID_NOT_PROVIDED` | ID is missing. | Identifier is not specified ||
|| `VALIDATION_INVALID_ID_FORMAT` | ID has to be a positive integer. | Invalid ID format ||
|| `DATASET_NOT_FOUND` | Dataset was not found. | The table does not exist or belongs to another application ||

|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-table-add.md)
- [{#T}](./biconnector-table-update.md)
- [{#T}](./biconnector-table-get.md)
- [{#T}](./biconnector-table-list.md)
- [{#T}](./biconnector-table-fields-update.md)
- [{#T}](./biconnector-table-fields.md)