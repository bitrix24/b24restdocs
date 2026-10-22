# Delete a CRM Form crm.form.delete

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

The `crm.form.delete` method permanently deletes a CRM form.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Form identifier.

The identifier can be obtained using the [crm.form.list](./crm-form-list.md) method ||
|| **updatedTime**
[`string`](../../data-types.md) | Form version — the `updatedTime` value from the [crm.form.get](./crm-form-get.md) response. If passed, the form is deleted only if it has not been changed since it was read. If not passed, the version is not checked ||
|| **confirmed**
[`boolean`](../../data-types.md) | Confirmation of deleting an active form.

Possible values:

- `true` — delete the form even if it is active
- `false` — do not delete an active form

Default: `false`.

A deactivated form can be deleted without confirmation

Only a `boolean` value is accepted: the strings `"Y"` or `"true"` return an error ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.form.delete`

{% endnote %}

The example deletes an active form with a version check.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":47,"updatedTime":"2026-10-21T17:48:05+02:00","confirmed":true}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.form.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":47,"updatedTime":"2026-10-21T17:48:05+02:00","confirmed":true,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.form.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FormDeleteResult = {
      result: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<FormDeleteResult>({
        method: 'crm.form.delete',
        params: {
          id: 47,
          updatedTime: '2026-10-21T17:48:05+02:00',
          confirmed: true,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.result)
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
      async function deleteForm() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'crm.form.delete',
            params: {
              id: 47,
              updatedTime: '2026-10-21T17:48:05+02:00',
              confirmed: true,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteForm)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # client = Client(token, prefer_version=3)

    try:
        bitrix_response = client.crm.form.delete(
            id=47,
            updatedTime="2026-10-21T17:48:05+02:00",
            confirmed=True,
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
                'crm.form.delete',
                [
                    'id' => 47,
                    'updatedTime' => '2026-10-21T17:48:05+02:00',
                    'confirmed' => true,
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
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.form.delete',
        {
          id: 47,
          updatedTime: '2026-10-21T17:48:05+02:00',
          confirmed: true,
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
        'crm.form.delete',
        [
            'id' => 47,
            'updatedTime' => '2026-10-21T17:48:05+02:00',
            'confirmed' => true,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "crm.form.delete", b24.Params{
    	"id": 47,
    	"updatedTime": "2026-10-21T17:48:05+02:00",
    	"confirmed": true,
    })
    if err != nil {
    	return fmt.Errorf("crm.form.delete: %w", err)
    }

    var data struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &data); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(data.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true
    },
    "time": {
        "start": 1792594087,
        "finish": 1792594087.940726,
        "duration": 0.9407260417938232,
        "processing": 0,
        "date_start": "2026-10-21T17:48:07+02:00",
        "date_finish": "2026-10-21T17:48:07+02:00",
        "operating_reset_at": 1792594680,
        "operating": 0.2279191017150879
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response.

Contains an object with the `result` key set to `true` if the form is deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **409**

```json
{
    "error": {
        "code": "CRM_FORM_DELETE_CONFIRMATION_REQUIRED",
        "message": "Could not delete the CRM form because it is active. Confirm the deletion and retry the request."
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
|| `confirmed` | This request requires data type `boolean` in field `confirmed` | Pass `true` or `false` without quotes ||
|| `updatedTime` | This request requires data type `DateTime` in field `updatedTime` | Pass the date in ISO 8601 format ||
|| `filter` | crm.form.delete removes one form addressed by id and takes no filter. | Pass the identifier of a single form in `id` without `filter` ||
|#

#### Deletion Confirmation Required Error

Error Code: `CRM_FORM_DELETE_CONFIRMATION_REQUIRED`

HTTP status: **409**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `confirmed` | Could not delete the CRM form because it is active. Confirm the deletion and retry the request | Pass `confirmed: true`, or deactivate the form first using the [crm.form.deactivate](./crm-form-deactivate.md) method ||
|#

#### Outdated Form Version Error

Error Code: `CRM_FORM_VERSION_MISMATCH`

HTTP status: **409**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `updatedTime` | Could not save changes because the CRM form version is outdated. Retrieve the form again and retry the request | Retrieve the form using the [crm.form.get](./crm-form-get.md) method and make sure it still needs to be deleted. The current version is in the `currentUpdatedTime` key of the response ||
|#

#### System Form Error

Error Code: `CRM_FORM_SYSTEM_IMMUTABLE`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Cannot edit or delete this CRM form | A system form cannot be deleted. Check the `system` field using the [crm.form.get](./crm-form-get.md) method ||
|#

#### Form Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | The form does not exist or has already been deleted. Check the identifier using the [crm.form.list](./crm-form-list.md) method ||
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

#### Post-Save Error

Error Code: `CRM_FORM_SIDE_EFFECT_FAILED`

HTTP status: **500**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Operation failed but CRM form data was modified. Check the form before trying again | Check whether the form is deleted using the [crm.form.get](./crm-form-get.md) method ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-form-deactivate.md)
- [{#T}](./crm-form-list.md)
- [{#T}](./index.md)
