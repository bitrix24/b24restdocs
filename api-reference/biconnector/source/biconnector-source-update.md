# Change source biconnector.source.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the "Analyst's workspace" section

The method `biconnector.source.update` updates an existing source.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the source, can be obtained using the methods [biconnector.source.list](./biconnector-source-list.md) and [biconnector.source.add](./biconnector-source-add.md) ||
|| **fields***
[`object`](../../data-types.md) | Object containing the updated data.
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
|| **title**
[`string`](../../data-types.md) | New name of the source ||
|| **description**
[`string`](../../data-types.md) | New description of the source ||
|| **active**
[`boolean`](../../data-types.md) | Activity status of the source ||
|| **settings**
[`object`](../../data-types.md) | List of parameters for authorization, passed as an object where the key is the `code` of the parameter. 
Parameters can be obtained using the methods [biconnector.connector.list](../connector/biconnector-connector-list.md) or [biconnector.connector.get](../connector/biconnector-connector-get.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}


{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": 4,
        "fields": {
            "title": "New source name",
            "description": "Updated source description",
            "active": false,
            "settings": {
                "login": "new_admin",
                "password": "new_password"
            }
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/biconnector.source.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "id": 4,
        "fields": {
            "title": "New source name",
            "description": "Updated source description",
            "active": false,
            "settings": {
                "login": "new_admin",
                "password": "new_password"
            }
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/biconnector.source.update
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
        method: 'biconnector.source.update',
        params: {
          id: 4,
          fields: {
            title: 'New source title',
            description: 'Updated source description',
            active: false,
            settings: {
              login: 'new_admin',
              password: 'new_password',
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Source updated:', result)
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
      async function updateSource() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.source.update',
            params: {
              id: 4,
              fields: {
                title: 'New source title',
                description: 'Updated source description',
                active: false,
                settings: {
                  login: 'new_admin',
                  password: 'new_password',
                },
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
          console.info('Source updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateSource)
    </script>
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'biconnector.source.update',
                [
                    'id' => 4,
                    'fields' => [
                        "title"       => "New source name",
                        "description" => "Updated source description",
                        "active"      => false,
                        "settings"    => [
                            "login"    => "new_admin",
                            "password" => "new_password"
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating source: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.source.update',
        {
            id: 4,
            fields: {
                "title": "New source name",
                "description": "Updated source description",
                "active": false,
                "settings": {
                    "login": "new_admin",
                    "password": "new_password"
                }
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
        'biconnector.source.update',
        [
            'id' => 4,
            'fields' => [
                'title' => 'New source name',
                'description' => 'Updated source description',
                'active' => false,
                'settings' => [
                    'login' => 'new_admin',
                    'password' => 'new_password'
                ]
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
[`boolean`](../../data-types.md) | Root element of the response, contains `true` on success ||
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
|| `VALIDATION_FIELDS_NOT_PROVIDED` | Fields not provided. | Fields not passed in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | Unknown parameters: #LIST_OF_PARAMS# | Unknown parameters detected: list ||
|| `VALIDATION_REQUIRED_FIELD_MISSING` | Field "#TITLE#" is required. | Required field #TITLE# not provided ||
|| `VALIDATION_READ_ONLY_FIELD` | Field "#TITLE#" is read only. | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_IMMUTABLE_FIELD` | Field "#TITLE#" is immutable. | Field #TITLE# is immutable ||
|| `VALIDATION_INVALID_FIELD_TYPE` | Field "#TITLE#" must be of type #TYPE#. | Field #TITLE# must be of type #TYPE# ||
|| `SOURCE_NOT_FOUND` | Source was not found. | Source not found ||
|| `SOURCE_CREATE_CONNECTION_ERROR` | Cannot create connection. | Error creating connection ||
|| `SOURCE_UPDATE_CONNECTION_ERROR` | Cannot update connection. | Error updating connection ||
|| `BX_ERROR` | Cannot delete source. Delete all related datasets first. | Cannot delete source while related datasets exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./biconnector-source-add.md)
- [{#T}](./biconnector-source-get.md)
- [{#T}](./biconnector-source-list.md)
- [{#T}](./biconnector-source-delete.md)
- [{#T}](./biconnector-source-fields.md)