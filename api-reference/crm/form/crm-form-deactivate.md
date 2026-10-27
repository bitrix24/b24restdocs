# Deactivate a CRM Form crm.form.deactivate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit CRM forms

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `crm.form.deactivate` method deactivates a CRM form: the form stops accepting submissions on all pages where it is placed.

{% note info "" %}

Deactivation does not delete the form or its settings. You can activate the form again using the [crm.form.activate](./crm-form-activate.md) method.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Form identifier.

The identifier can be obtained using the [crm.form.list](./crm-form-list.md) method ||
|| **updatedTime**
[`string`](../../data-types.md) | Form version — the `updatedTime` value from the [crm.form.get](./crm-form-get.md) response. If passed, the form is deactivated only if it has not been changed since it was read. If not passed, the version is not checked ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.deactivate`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":47}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.deactivate
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":47,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.deactivate
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormDeactivateResult = {
      active: boolean
      updatedTime: string
    }

    try {
      const response = await $b24.actions.v3.call.make<FormDeactivateResult>({
        method: 'crm.form.deactivate',
        params: {
          id: 47,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.active, result.updatedTime)
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
      async function deactivateForm() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.deactivate',
            params: {
              id: 47,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.active, result.updatedTime)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deactivateForm)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # client = Client(token, prefer_version=3)

    try:
        bitrix_response = client.crm.form.deactivate(
            id=47,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.form.deactivate',
                [
                    'id' => 47,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.form.deactivate',
        {
          id: 47,
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.form.deactivate',
        [
            'id' => 47,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "crm.form.deactivate", b24.Params{
    	"id": 47,
    })
    if err != nil {
    	return fmt.Errorf("crm.form.deactivate: %w", err)
    }

    var data struct {
    	Active      bool   `json:"active"`
    	UpdatedTime string `json:"updatedTime"`
    }
    if err := json.Unmarshal(res.Result, &data); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(data.Active, data.UpdatedTime)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "active": false,
        "updatedTime": "2026-10-21T17:44:56+02:00"
    },
    "time": {
        "start": 1792593896,
        "finish": 1792593898.890856,
        "duration": 2.8908560276031494,
        "processing": 2,
        "date_start": "2026-10-21T17:44:56+02:00",
        "date_finish": "2026-10-21T17:44:58+02:00",
        "operating_reset_at": 1792594440,
        "operating": 2.0196049213409424
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Form state after the call ||
|| **active**
[`boolean`](../../data-types.md) | Whether the form is active. After a successful call — `false` ||
|| **updatedTime**
[`string`](../../data-types.md) | New form version. Pass it to the next update method ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": {
        "code": "CRM_FORM_SYSTEM_IMMUTABLE",
        "message": "Cannot edit or delete this CRM form."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the form identifier ||
|| `updatedTime` | This request requires data type `DateTime` in field `updatedTime` | Pass the date in ISO 8601 format ||
|| `filter` | crm.form.deactivate turns off one form addressed by id and takes no filter. | Deactivate forms one at a time by passing `id` without `filter` ||
|#

#### Outdated Form Version Error

Error Code: `CRM_FORM_VERSION_MISMATCH`

HTTP status: **409**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `updatedTime` | Could not save changes because the CRM form version is outdated. Retrieve the form again and retry the request | Retrieve the form using the [crm.form.get](./crm-form-get.md) method and repeat the request. The current version is in the `currentUpdatedTime` key of the response ||
|#

#### System Form Error

Error Code: `CRM_FORM_SYSTEM_IMMUTABLE`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Cannot edit or delete this CRM form | The activity of a system form cannot be changed ||
|#

#### Form Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | Check the identifier using the [crm.form.list](./crm-form-list.md) method ||
|#

#### Plan Restrictions

Error Code: `CRM_FORM_WRITE_RESTRICTED`

HTTP status: **402**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | You cannot modify CRM forms on your plan | Change your Bitrix24 plan ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check that the user has permission to edit CRM forms ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-activate.md)
- [{#T}](./crm-form-delete.md)
- [{#T}](./index.md)
