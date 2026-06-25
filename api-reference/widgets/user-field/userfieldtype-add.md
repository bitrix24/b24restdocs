# Register a New User Field Type userfieldtype.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [depending on the placement](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `userfieldtype.add` registers a new type of user fields. After registering the type, create a user field using the [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig-add.md) method.

When opening a card with a user type field, an array `PLACEMENT_OPTIONS` containing data about the field and entity is passed to the application handler.

```json
{
    "MODE": "view",
    "ENTITY_ID": "CRM_DEAL",
    "FIELD_NAME": "UF_CRM_TEST_TYPE_1",
    "ENTITY_VALUE_ID": "7303",
    "VALUE": null,
    "MULTIPLE": "N",
    "MANDATORY": "N",
    "XML_ID": null,
    "ENTITY_DATA": {
        "entityTypeId": 2,
        "entityId": "7303",
        "module": "crm"
    }
}
```

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** | **Restrictions** ||
|| **USER_TYPE_ID***
[`string`](../../data-types.md) | String type code | 
- a-z0-9
- must be unique
- the final code is formed as `rest_<APP_ID>_<USER_TYPE_ID>` and cannot exceed 50 characters, so the length of `USER_TYPE_ID` must be no more than `50 - length("rest_<APP_ID>_")` ||
|| **HANDLER***
[`URL`](../../data-types.md) | Custom type handler address | 
- in the same domain as the main application address
- must be unique ||
|| **TITLE**
[`string`](../../data-types.md) | Textual type name. Will be displayed in the custom field settings administrative interface | ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Textual type description. Will be displayed in the custom field settings administrative interface | ||
|| **OPTIONS**
[`array`](../../data-types.md) | Additional settings. Currently, one key is available: `height` — specifies the custom field height in pixels. Any positive value will be applied.
Default — `0`. If specified `0`, the standard height for displaying this widget will be used | ||
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
        "USER_TYPE_ID": "test_type",
        "HANDLER": "https://www.myapplication.com/handler/",
        "TITLE": "Updated test type",
        "DESCRIPTION": "Test userfield type for documentation with updated description",
        "OPTIONS": {
            "height": 60
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldtype.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "USER_TYPE_ID": "test_type",
        "HANDLER": "https://www.myapplication.com/handler/",
        "TITLE": "Updated test type",
        "DESCRIPTION": "Test userfield type for documentation with updated description",
        "OPTIONS": {
            "height": 60
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/userfieldtype.add
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
        method: 'userfieldtype.add',
        params: {
          USER_TYPE_ID: 'test_type',
          HANDLER: 'https://www.myapplication.com/handler/',
          TITLE: 'Updated test type',
          DESCRIPTION: 'Test userfield type for documentation with updated description',
          OPTIONS: {
            height: 60,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Registration result:', result)
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
      async function addUserFieldType() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'userfieldtype.add',
            params: {
              USER_TYPE_ID: 'test_type',
              HANDLER: 'https://www.myapplication.com/handler/',
              TITLE: 'Updated test type',
              DESCRIPTION: 'Test userfield type for documentation with updated description',
              OPTIONS: {
                height: 60,
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
          console.info('Registration result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addUserFieldType)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'userfieldtype.add',
                [
                    'USER_TYPE_ID' => 'test_type',
                    'HANDLER'      => 'https://www.myapplication.com/handler/',
                    'TITLE'        => 'Updated test type',
                    'DESCRIPTION'  => 'Test userfield type for documentation with updated description',
                    'OPTIONS'      => [
                        'height' => 60,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding user field type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'userfieldtype.add',
        {
            USER_TYPE_ID: 'test_type',
            HANDLER: 'https://www.myapplication.com/handler/',
            TITLE: 'Updated test type',
            DESCRIPTION: 'Test userfield type for documentation with updated description',
            OPTIONS: {
                height: 60,
            },
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldtype.add',
        [
            'USER_TYPE_ID' => 'test_type',
            'HANDLER' => 'https://www.myapplication.com/handler/',
            'TITLE' => 'Upd ated test type',
            'DESCRIPTION' => 'Test userfield type for documentation with updated description',
            'OPTIONS' => [
                'height' => 60
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
    "result":true,
    "time":{
        "start":1724421710.397825,
        "finish":1724421711.040353,
        "duration":0.6425280570983887,
        "processing":5.888938903808594e-5,
        "date_start":"2024-08-23T16:01:50+02:00",
        "date_finish":"2024-08-23T16:01:51+02:00","operating":0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of registering the new user field type ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"Unable to set placement handler: Handler already binded"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %} 

### Possible Error Codes

#|
|| **Code** | **Error message** | **Description** ||
|| `ERROR_CORE` | Unable to set placement handler: Handler already binded | `HANDLER` is already occupied by another user field type of this application or `USER_TYPE_ID` is already used by another application ||
|| `ERROR_ARGUMENT` | Argument 'USER_TYPE_ID' is null or empty | `USER_TYPE_ID` is not set ||
|| `ERROR_ARGUMENT` | Argument 'HANDLER' is null or empty | `HANDLER` is not set ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldtype-update.md)
- [{#T}](./userfieldtype-list.md)
- [{#T}](./userfieldtype-delete.md)
- [{#T}](../../crm/universal/userfieldconfig/userfieldconfig-add.md)
- [{#T}](../../crm/universal/user-defined-fields/userfield-type.md)
