# Change settings for user field type userfieldtype.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`depending on the embedding location`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `userfieldtype.update` modifies the settings of a user field type registered by the application. It returns _true_ or an error with a description of the reason.

## Method parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** | **Restrictions** ||
|| **USER_TYPE_ID***
[`string`](../../data-types.md) | String code of the type | 
- a-z0-9
- must be unique ||
|| **HANDLER***
[`URL`](../../data-types.md) | Address of the user type handler | 
- in the same domain as the main application address
- must be unique ||
|| **TITLE***
[`string`](../../data-types.md) | Text name of the type. Will be displayed in the administrative interface for user field settings | ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Text description of the type. Will be displayed in the administrative interface for user field settings | ||
|| **OPTIONS**
[`array`](../../data-types.md) | Additional settings. Currently, one key is available: `height` — specifies the height of the user field in pixels. Any positive value will apply.
Default is `0`. If `0` is specified, the standard height for displaying this embedding will be used | ||
|#

## Code examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
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
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldtype.update
    ```

- cURL (OAuth)

    ```curl
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
    https://**put_your_bitrix24_address**/rest/userfieldtype.update
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
        method: 'userfieldtype.update',
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
        console.info('userfieldtype.update result:', result)
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
      async function updateUserFieldType() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'userfieldtype.update',
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
          console.info('userfieldtype.update result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateUserFieldType)
    </script>
    ```

- PHP

    ```php        
    try {
        $result = $serviceBuilder->getPlacementScope()
            ->userFieldType()
            ->update(
                'custom_user_type',  // userTypeId
                'https://example.com/handler',  // handlerUrl
                'Custom User Type',  // title
                'Description of custom user type'  // description
            );
        if ($result->isSuccess()) {
            print("Update successful.");
        } else {
            print("Update failed.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'userfieldtype.update',
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
        'userfieldtype.update',
        [
            'USER_TYPE_ID' => 'test_type',
            'HANDLER' => 'https://www.myapplication.com/handler/',
            'TITLE' => 'Updated test type',
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

## Response handling

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
        "date_finish":"2024-08-23T16:01:51+02:00",
        "operating":0
    }
}
```

### Returned data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of changing the user field type ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error handling

HTTP status: **400**

```json
{
    "error":"ERROR_NOT_FOUND",
    "error_description":"User Field Type not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %} 

### Possible error codes

#|
|| **Code** | **Error message** | **Description** ||
|| `ERROR_CORE` | Unable to set placement handler: Handler already binded | `HANDLER` is already occupied by another user field type of this application or `USER_TYPE_ID` is already used by another application ||
|| `ERROR_ARGUMENT` | Argument 'USER_TYPE_ID' is null or empty | `USER_TYPE_ID` is not specified ||
|| `ERROR_NOT_FOUND` | User Field Type not found | User field with the specified `USER_TYPE_ID` not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue learning

- [{#T}](./userfieldtype-add.md)
- [{#T}](./userfieldtype-list.md)
- [{#T}](./userfieldtype-delete.md)
