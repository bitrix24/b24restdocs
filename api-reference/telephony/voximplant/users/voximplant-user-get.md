# Get User Settings voximplant.user.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: user with User settings — Update access permission

The method `voximplant.user.get` returns user settings.

The method works only in the application context. Calling the method requires [administrator confirmation](../../../scopes/confirmation.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID***
[```integer | integer[]```](../../../data-types.md#compound-types) | User ID or an array of user IDs.

IDs of users you cannot access under the User settings — Update permission are excluded from the request without an error. The `ACCESS_DENIED` error occurs only if all IDs are excluded. No records are returned for non-existent users, and no error occurs either.

You can obtain the ID using the [user.get](../../../user/user-get.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":[1269, 1271],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.user.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each user settings entry returned in result[]
    type VoximplantUserResult = {
      ID: string
      DEFAULT_LINE: string | null
      PHONE_ENABLED: string
      SIP_SERVER: string
      SIP_LOGIN: string
      SIP_PASSWORD: string | null
      INNER_NUMBER: string | null
    }

    try {
      const response = await $b24.actions.v2.call.make<VoximplantUserResult[]>({
        method: 'voximplant.user.get',
        params: {
          USER_ID: [1269, 1271],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User settings count:', result.length, result)
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
      async function getUserSettings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'voximplant.user.get',
            params: {
              USER_ID: [1269, 1271],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('User settings count:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getUserSettings)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.voximplant.user.get(
            user_id=[
                1269,
                1271,
            ],
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
                'voximplant.user.get',
                [
                    'USER_ID' => [1269, 1271],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'voximplant.user.get',
        {
            USER_ID: [1269, 1271]
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'voximplant.user.get',
        [
            'USER_ID' => [1269, 1271],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "voximplant.user.get", b24.Params{
    	"USER_ID": []int{1269, 1271},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("voximplant.user.get: %w", err)
    }

    var items []struct {
    	ID           b24.ID  `json:"ID"`
    	DefaultLine  *string `json:"DEFAULT_LINE"`
    	PhoneEnabled string  `json:"PHONE_ENABLED"`
    	SipServer    string  `json:"SIP_SERVER"`
    	SipLogin     string  `json:"SIP_LOGIN"`
    	SipPassword  *string `json:"SIP_PASSWORD"`
    	InnerNumber  *string `json:"INNER_NUMBER"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.PhoneEnabled)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
        "ID": "1269",
        "DEFAULT_LINE": null,
        "PHONE_ENABLED": "Y",
        "SIP_SERVER": "ip.b24-6059-1584795982.bitrixphone.com",
        "SIP_LOGIN": "phone1269",
        "SIP_PASSWORD": "28fec473a88a",
        "INNER_NUMBER": null
        },
        {
        "ID": "1271",
        "DEFAULT_LINE": null,
        "PHONE_ENABLED": "Y",
        "SIP_SERVER": "ip.b24-6059-158475982.bitrixphone.com",
        "SIP_LOGIN": "phone1271",
        "SIP_PASSWORD": null,
        "INNER_NUMBER": null
        }
    ],
    "time": {
        "start": 1773667386,
        "finish": 1773667386.23801,
        "duration": 0.23800992965698242,
        "processing": 0,
        "date_start": "2026-03-16T16:23:06+01:00",
        "date_finish": "2026-03-16T16:23:06+01:00",
        "operating_reset_at": 1773667986,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | Array of user settings [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../../data-types.md) | User ID ||
|| **DEFAULT_LINE**
[`string`](../../../data-types.md) | The user's outgoing line. Usually this is a line ID in the format of the [voximplant.line.get](../lines/voximplant-line-get.md) keys or `LINK_BASE_NUMBER`.

The method returns the value exactly as it is stored in the user settings, so older settings may contain text here, such as `Default number`. `null` if no line is selected ||
|| **PHONE_ENABLED**
[`string`](../../../data-types.md) | Indicator of SIP device availability.

Possible values:

- `Y` — SIP device is activated for the user
- `N` — SIP device is not activated for the user ||
|| **SIP_SERVER**
[`string`](../../../data-types.md) | Address of the server for connecting the SIP device ||
|| **SIP_LOGIN**
[`string`](../../../data-types.md) | Login for connecting the SIP device ||
|| **SIP_PASSWORD**
[`string`](../../../data-types.md) | Password for connecting the SIP device. `null` if no password is set ||
|| **INNER_NUMBER**
[`string`](../../../data-types.md) | User's internal number. May be `null` ||
|#

## Error Handling

HTTP Status: **401**

```json
{
    "error": "METHOD_CONFIRM_WAITING",
    "error_description": "Waiting for confirmation"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Parameter USER_ID is not set` | Required parameter `USER_ID` is not specified ||
|| `401` | `METHOD_CONFIRM_WAITING` | `Waiting for confirmation` | Confirmation from a Bitrix24 administrator is pending for the method call ||
|| `403` | `METHOD_CONFIRM_DENIED` | `Method call denied` | Administrator has denied the method call for the current token ||
|| `403` | `ACCESS_DENIED` | `Access denied! You have no permission to query selected users` | Insufficient permissions to retrieve user settings ||
|| `403` | `ACCESS_DENIED` | `Access denied! Application context required` | The method is called outside an application, for example, via a webhook ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./voximplant-user-activate-phone.md)
- [{#T}](./voximplant-user-deactivate-phone.md)
- [{#T}](../../../scopes/confirmation.md)
