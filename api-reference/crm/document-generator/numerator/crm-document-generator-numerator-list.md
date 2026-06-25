# Get the List of Numerators crm.documentgenerator.numerator.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify" access permission for Document Generator templates

The method `crm.documentgenerator.numerator.list` returns a list of numerators.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | Offset for pagination. More details in the article [Features of List Methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.numerator.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.numerator.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NumeratorResult = {
      numerators: {
        id: string
        name: string
        template: string
        settings: Record<string, {
          start: number
          step: number
          length?: number
          padString?: string
          periodicBy: string | null
          timezone: string | null
          isDirectNumeration: boolean
        }>
      }[]
    }

    try {
      // crm.documentgenerator.numerator.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<NumeratorResult>({
        method: 'crm.documentgenerator.numerator.list',
        params: {
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Numerators count:', result.numerators.length, result.numerators)
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
      async function listNumerators() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.documentgenerator.numerator.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.numerator.list',
            params: {
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
          console.info('Numerators count:', result.numerators.length, result.numerators)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listNumerators)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.documentgenerator.numerator.list',
                [
                    'start' => 0,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo '<pre>';
        print_r($result);
        echo '</pre>';

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting numerators list: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.documentgenerator.numerator.list(start=0).response
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
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    `as_list` Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.documentgenerator.numerator.list().as_list().response
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
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    `as_list_fast` Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.documentgenerator.numerator.list().as_list_fast(descending=True).response
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
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.numerator.list',
        {},
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.info(result.data());

            if (result.more()) {
                result.next();
            }
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.documentgenerator.numerator.list',
        [
            'start' => 0,
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
    "result": {
        "numerators": [
            {
                "id": "2",
                "name": "Certificate (United States)",
                "template": "{NUMBER}",
                "settings": {
                    "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
                        "start": 1,
                        "step": 1,
                        "periodicBy": null,
                        "timezone": null,
                        "isDirectNumeration": false
                    }
                }
            },
            {
                "id": "45",
                "name": "REST Enumerator (updated)",
                "template": "INV-{NUMBER}",
                "settings": {
                    "Bitrix_Main_Numerator_Generator_SequentNumberGenerator": {
                        "start": 100,
                        "step": 1,
                        "length": 0,
                        "padString": "0",
                        "periodicBy": null,
                        "timezone": null,
                        "isDirectNumeration": false
                    }
                }
            }
        ]
    },
    "total": 19,
    "time": {
        "start": 1773750210,
        "finish": 1773750210.283658,
        "duration": 0.2836580276489258,
        "processing": 0,
        "date_start": "2026-03-17T15:23:30+03:00",
        "date_finish": "2026-03-17T15:23:30+03:00",
        "operating_reset_at": 1773750810,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response. Contains an array of [`numerators`](#numerators) ||
|| **total**
[`integer`](../../data-types.md) | Total number of numerators ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Array of Numerators {#numerators}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | The identifier of the numerator ||
|| **name**
[`string`](../../data-types.md) | The name of the numerator ||
|| **template**
[`string`](../../data-types.md) | The number template ||
|| **settings**
[`object`](../../data-types.md) | Saved settings for sequential numbering of type [`settings`](#settings) ||
|#

#### Settings Type {#settings}

#|
|| **Name**
`type` | **Description** ||
|| **start**
[`integer`](../../data-types.md) | The initial value of the counter ||
|| **step**
[`integer`](../../data-types.md) | The increment step of the counter ||
|| **length**
[`integer`](../../data-types.md) | The minimum length of the number ||
|| **padString**
[`string`](../../data-types.md) | The left padding character ||
|| **periodicBy**
[`string`](../../data-types.md) | The reset period for the counter: `null`, `day`, `month`, or `year` ||
|| **timezone**
[`string`](../../data-types.md) | The timezone identifier for periodic reset. Can be `null` ||
|| **isDirectNumeration**
[`boolean`](../../data-types.md) | Indicator of direct numbering ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "You do not have permissions to modify templates"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty value | `You do not have permissions to modify templates` | Insufficient permissions to modify document generator templates ||
|| Empty value | `Module documentgenerator is not installed` | The `documentgenerator` module is not available ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-numerator-add.md)
- [{#T}](./crm-document-generator-numerator-update.md)
- [{#T}](./crm-document-generator-numerator-get.md)
- [{#T}](./crm-document-generator-numerator-delete.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)