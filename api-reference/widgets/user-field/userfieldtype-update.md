# Change settings for user field type userfieldtype.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `userfieldtype.update` method modifies the settings of a user field type previously registered by the application. It updates the handler URL, name, description, and field height, but does not change the type code.

A field of this type is displayed in the CRM item form. When a user opens the form, Bitrix24 loads the URL from `HANDLER` in a frame within the field. The general workflow and handler data format are described in [User Field Types](./index.md).

The method returns `true` if the settings are updated.

{% note info "" %}

The method works only in the [application](../../../settings/app-installation/index.md) context.

{% endnote %}

## Method parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** | **Restrictions** ||
|| **USER_TYPE_ID***
[`string`](../../data-types.md) | Short code of the previously registered user field type. Retrieve the code using [userfieldtype.list](./userfieldtype-list.md) | ||
|| **HANDLER**
[`string`](../../data-types.md) | New handler URL for the user field type. Bitrix24 loads this URL in a frame within the field | Absolute URLs with the `http` or `https` protocol are allowed ||
|| **TITLE**
[`string`](../../data-types.md) | Text name of the type. Will be displayed in the administrative interface for user field settings | ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Text description of the type. Will be displayed in the administrative interface for user field settings | ||
|| **OPTIONS**
[`object`](../../data-types.md) | Additional settings. Currently, one key is available: `height`—specifies the height of the user field in pixels. The value is converted to an integer.
Default is `0`. If `0` is specified, the standard height for displaying this widget will be used | ||
|| **LANG_ALL**
[`object`](../../data-types.md) | Type name and description for different languages. The object key is the language code, and the value is an object with the `TITLE` and `DESCRIPTION` fields | ||
|#

{% note info "" %}

In addition to `USER_TYPE_ID`, pass at least one parameter with new settings: `HANDLER`, `TITLE`, `DESCRIPTION`, `OPTIONS`, or `LANG_ALL`.

{% endnote %}

## Code examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    options = {
        "height": 60,
    }

    try:
        bitrix_response = client.userfieldtype.update(
            user_type_id="test_type",
            handler="https://www.myapplication.com/handler/",
            title="Updated test type",
            description="Test userfield type for documentation with updated description",
            options=options,
        ).response
        result = bitrix_response.result
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "userfieldtype.update", b24.Params{
    	"USER_TYPE_ID": "test_type",
    	"HANDLER":      "https://www.myapplication.com/handler/",
    	"TITLE":        "Updated test type",
    	"DESCRIPTION":  "Test userfield type for documentation with updated description",
    	"OPTIONS": b24.Params{
    		"height": 60,
    	},
    })
    if err != nil {
    	return fmt.Errorf("userfieldtype.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
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

HTTP status: **400 or 403**

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
|| `ERROR_CORE` | Unable to update User Field Type: Handler already binded | `HANDLER` is already occupied by another user field type of this application ||
|| `ERROR_ARGUMENT` | Argument 'USER_TYPE_ID' is null or empty | `USER_TYPE_ID` is not specified ||
|| `ERROR_ARGUMENT` | Argument 'HANDLER\|TITLE\|DESCRIPTION' is null or empty | No settings to update were passed: `HANDLER`, `TITLE`, `DESCRIPTION`, `OPTIONS`, or `LANG_ALL` ||
|| `ERROR_NOT_FOUND` | User Field Type not found | No registered user field type with the specified `USER_TYPE_ID` was found ||
|| `ERROR_UNSUPPORTED_PROTOCOL` | Unsupported handler protocol | `HANDLER` uses a protocol other than `http` or `https` ||
|| `ERROR_WRONG_HANDLER_URL` | Wrong handler URL | `HANDLER` contains an invalid absolute URL ||
|| `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | The method was called outside the application context ||
|| `ACCESS_DENIED` | Access denied! | The method was called by a user without administrator permissions ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](./userfieldtype-add.md)
- [{#T}](./userfieldtype-list.md)
- [{#T}](./userfieldtype-delete.md)
- [{#T}](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md)
