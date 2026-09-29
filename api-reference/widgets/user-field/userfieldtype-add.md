# Register a New User Field Type userfieldtype.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `userfieldtype.add` method registers an application's user field type and works only in the context of an [application](../../../settings/app-installation/index.md). After registration, create a field of this type using the [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig-add.md) method: pass the full code `rest_<APP_ID>_<USER_TYPE_ID>` in `field.userTypeId`, where `APP_ID` is the application identifier from the [app.info](../../common/system/app-info.md) method. To create a field, the application also needs the `userfieldconfig` and `crm` scopes.

A custom type is needed when a field must display the application interface, for example, data from an external service. For plain text, a number, or a list, standard field types are sufficient.

When a user opens a card with a field of this type, Bitrix24 loads the `HANDLER` address in the field frame and passes the field and item data in `PLACEMENT_OPTIONS`. Example for a field in a deal card:

```json
{
    "MODE": "view",
    "ENTITY_ID": "CRM_DEAL",
    "FIELD_NAME": "UF_CRM_DCTEST",
    "ENTITY_VALUE_ID": "22",
    "VALUE": "Field value",
    "MULTIPLE": "N",
    "MANDATORY": "N",
    "XML_ID": null,
    "ENTITY_DATA": {
        "entityTypeId": 2,
        "entityId": "22",
        "module": "crm"
    },
    "URI": "/crm/deal/details/22/?IFRAME=Y&IFRAME_TYPE=SIDE_SLIDER"
}
```

What each key means and what else the request contains is described in the section [What the Handler Receives](./index.md#handler-data).

The handler loads in the field only after the application installation is complete. You can check this by the `INSTALLED` value in the response of the [app.info](../../common/system/app-info.md) method.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_TYPE_ID***
[`string`](../../data-types.md) | Type code. The method converts it to lowercase.

The code must be unique among the types of all applications in Bitrix24: if it is already taken, the method returns the `Handler already binded` error.

The full type code `rest_<APP_ID>_<USER_TYPE_ID>` must fit within 50 characters. The method also accepts a longer code, but the type code of the created field is truncated to 50 characters, and Bitrix24 will not find the type ||
|| **HANDLER***
[`string`](../../data-types.md) | Handler address that Bitrix24 loads into the field. It must start with `http://` or `https://`, and the host name must contain a dot.

Each type of the application must have its own address: if the address is already taken, the method returns the `Handler already binded` error.

For Bitrix24 over HTTPS, use an HTTPS address, otherwise the browser will not load the field content ||
|| **TITLE**
[`string`](../../data-types.md) | Type name, up to 255 characters. Displayed in the administrative interface for configuring custom fields. If not passed, the type code becomes the name ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Type description, up to 255 characters. Displayed in the administrative interface for configuring custom fields ||
|| **OPTIONS**
[`object`](../../data-types.md) | Additional settings. Currently, one key is available: `height` — the field height in pixels, an integer.

Default — `0`: the field gets the standard height, which is 200 pixels in the CRM card ||
|| **LANG_ALL**
[`object`](../../data-types.md) | Type name and description for different languages. The object key is a two-letter language code, for example `ru` or `en`. The value is an object with the string fields `TITLE` and `DESCRIPTION`, up to 255 characters each. If a non-empty `LANG_ALL` is passed, the method ignores the `TITLE` and `DESCRIPTION` parameters.

The method retains all supplied translations. [userfieldtype.list](./userfieldtype-list.md) returns one version of the name and description, selected during registration: the version in the Bitrix24 language if supplied, or another supplied version otherwise ||
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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    options = {
        "height": 60,
    }

    try:
        bitrix_response = client.userfieldtype.add(
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

        echo 'Success: ' . var_export($result[0], true);

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
    res, err := client.Core().Call(ctx, "userfieldtype.add", b24.Params{
    	"USER_TYPE_ID": "test_type",
    	"HANDLER":      "https://www.myapplication.com/handler/",
    	"TITLE":        "Updated test type",
    	"DESCRIPTION":  "Test userfield type for documentation with updated description",
    	"OPTIONS": b24.Params{
    		"height": 60,
    	},
    })
    if err != nil {
    	return fmt.Errorf("userfieldtype.add: %w", err)
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
|| **Status** | **Code** | **Description** | **Meaning** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | The method was called outside an application, for example via a webhook ||
|| `403` | `ACCESS_DENIED` | Access denied! | The method was called by a user who is not an administrator ||
|| `400` | `ERROR_CORE` | Unable to set placement handler: Handler already binded | `HANDLER` is already occupied by another type of this application, or this `USER_TYPE_ID` is already registered ||
|| `400` | `ERROR_CORE` | Error: The length of "TITLE" should not exceed 255 characters | The name, description, or language code is longer than allowed. The method creates a type record before the error occurs, and the record appears in [userfieldtype.list](./userfieldtype-list.md), but the registration is not complete — a field with this type cannot be created. Delete the type using the [userfieldtype.delete](./userfieldtype-delete.md) method and register it again ||
|| `400` | `ERROR_ARGUMENT` | Argument 'USER_TYPE_ID' is null or empty | `USER_TYPE_ID` is not passed ||
|| `400` | `ERROR_ARGUMENT` | Argument 'HANDLER' is null or empty | `HANDLER` is not passed ||
|| `400` | `ERROR_WRONG_HANDLER_URL` | Wrong handler URL | `HANDLER` does not specify a host name, or the host name contains no dot. For example, the address is written without `https://` or points to `localhost` ||
|| `400` | `ERROR_UNSUPPORTED_PROTOCOL` | Unsupported handler protocol | The host name in `HANDLER` is correct, but the protocol is neither `http` nor `https`, for example `ftp://` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldtype-update.md)
- [{#T}](./userfieldtype-list.md)
- [{#T}](./userfieldtype-delete.md)
- [{#T}](../../crm/universal/userfieldconfig/userfieldconfig-add.md)
- [{#T}](../../crm/universal/user-defined-fields/userfield-type.md)
