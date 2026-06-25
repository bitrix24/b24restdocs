# Get Information About the Numerator crm.documentgenerator.numerator.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify" access permission for document generator templates

The method `crm.documentgenerator.numerator.get` returns information about the numerator by its identifier.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | The identifier of the numerator ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of retrieving a numerator with `id = 45`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":45}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.documentgenerator.numerator.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":45,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.documentgenerator.numerator.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type NumeratorGetResult = {
      numerator: {
        id: string
        name: string
        template: string
        code: string | null
        settings: Record<string, {
          start: number
          step: number
          length: number
          padString: string
          periodicBy: string | null
          timezone: string | null
          isDirectNumeration: boolean
        }>
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<NumeratorGetResult>({
        method: 'crm.documentgenerator.numerator.get',
        params: {
          id: 45,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.numerator.id, result.numerator.name, result.numerator.template)
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
      async function getNumerator() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.documentgenerator.numerator.get',
            params: {
              id: 45,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.numerator.id, result.numerator.name, result.numerator.template)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getNumerator)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.documentgenerator.numerator.get',
                [
                    'id' => 45,
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
        echo 'Error getting numerator: ' . $e->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.documentgenerator.numerator.get(bitrix_id=45).response
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

- BX24.js

    ```js
    BX24.callMethod(
        'crm.documentgenerator.numerator.get',
        {
            id: 45,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.documentgenerator.numerator.get',
        [
            'id' => 45,
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
        "numerator": {
            "id": "45",
            "name": "REST Enumerator (updated)",
            "template": "INV-{NUMBER}",
            "code": null,
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
    },
    "time": {
        "start": 1773747475,
        "finish": 1773747475.904903,
        "duration": 0.9049029350280762,
        "processing": 0,
        "date_start": "2026-03-17T14:37:55+03:00",
        "date_finish": "2026-03-17T14:37:55+03:00",
        "operating_reset_at": 1773748075,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response. Contains the [`numerator`](#numerator) object ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Numerator Type {#numerator}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../data-types.md) | The identifier of the numerator ||
|| **name**
[`string`](../../data-types.md) | The name of the numerator ||
|| **template**
[`string`](../../data-types.md) | The number template ||
|| **code**
[`string`](../../data-types.md) | The symbolic code of the numerator. Can be `null` ||
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
    "error": "100",
    "error_description": "Could not construct parameter {numerator}"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | `Bitrix\Main\Numerator\Numerator constructor must be is public` | Error creating the numerator object ||
|| `100` | `Could not construct parameter {numerator}` | Numerator with the specified `id` not found ||
|| `DOCGEN_ACCESS_ERROR` | `Access denied` | No access to the numerator, or the numerator does not belong to the document generator module ||
|| Empty value | `You do not have permissions to modify templates` | Insufficient permissions to modify document generator templates ||
|| Empty value | `Module documentgenerator is not installed` | The `documentgenerator` module is not available ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-document-generator-numerator-add.md)
- [{#T}](./crm-document-generator-numerator-update.md)
- [{#T}](./crm-document-generator-numerator-list.md)
- [{#T}](./crm-document-generator-numerator-delete.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)

