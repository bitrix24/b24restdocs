# Update Dataset biconnector.dataset.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Analyst Workspace" section

The method `biconnector.dataset.update` updates an existing dataset.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the dataset, can be obtained using the methods [biconnector.dataset.list](./biconnector-dataset-list.md) and [biconnector.dataset.add](./biconnector-dataset-add.md) ||
|| **fields***
[`object`](../../data-types.md) | An object containing the updated data.
Object format: 

```
{
    "field_1": "value_1",
    "field_2": "value_2",
    ...,
    "field_n": "value_n"
}
```

- `field_n` — field name
- `value_n` — field value

[Detailed description below](#fields)||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **description**
[`string`](../../data-types.md) | Description of the dataset ||
|#

To change the fields of the dataset, use the method [biconnector.dataset.fields.update](./biconnector-dataset-fields-update.md).

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": 10,
        "fields": {
            "description": "New description"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.dataset.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": 10,
        "fields": {
            "description": "New description"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/biconnector.dataset.update
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
        method: 'biconnector.dataset.update',
        params: {
          id: 10,
          fields: {
            description: 'New description',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Dataset updated:', result)
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
      async function updateDataset() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.dataset.update',
            params: {
              id: 10,
              fields: {
                description: 'New description',
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
          console.info('Dataset updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateDataset)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'biconnector.dataset.update',
                [
                    'id' => 10,
                    'fields' => [
                        "description" => "New description",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating dataset: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.dataset.update',
        {
            id: 10,
            fields: {
                "description": "New description",
            }
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.dataset.update',
        [
            'id' => 10,
            'fields' => [
                'description' => 'New description'
            ]
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
[`boolean`](../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **200**

```json
{
    "error": "VALIDATION_FIELDS_NOT_PROVIDED",
    "error_description": "Fields not provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `VALIDATION_ID_NOT_PROVIDED` | ID is missing. | Identifier is not specified ||
|| `VALIDATION_INVALID_ID_FORMAT` | ID has to be a positive integer. | Invalid ID format ||
|| `VALIDATION_FIELDS_NOT_PROVIDED` | Fields not provided. | Fields were not passed in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | Unknown parameters: #LIST_OF_PARAMS# | Unknown parameters detected: #LIST\_OF\_PARAMS#. ||
|| `VALIDATION_REQUIRED_FIELD_MISSING` | Field "#TITLE#" is required. | Required field #TITLE# was not provided ||
|| `VALIDATION_READ_ONLY_FIELD` | Field "#TITLE#" is read only. | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_IMMUTABLE_FIELD` | Field "#TITLE#" is immutable. | Field #TITLE# is immutable ||
|| `VALIDATION_INVALID_FIELD_TYPE` | Field "#TITLE#" must be of type #TYPE#. | Field #TITLE# must be of type #TYPE# ||
|| `DATASET_NOT_FOUND` | Dataset was not found. | Dataset not found ||
|| `INVALID_METHOD` | Use the method "biconnector.dataset.fields.update" to update the dataset fields. | To update fields, use the method `biconnector.dataset.fields.update` ||
|| `-` | `Error updating dataset.` | Error updating dataset ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-dataset-add.md)
- [{#T}](./biconnector-dataset-fields.md)
- [{#T}](./biconnector-dataset-fields-update.md)
- [{#T}](./biconnector-dataset-get.md)
- [{#T}](./biconnector-dataset-list.md)
- [{#T}](./biconnector-dataset-delete.md)