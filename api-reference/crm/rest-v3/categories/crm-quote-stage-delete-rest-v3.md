# Delete an Estimate Stage in CRM v3.0 crm.quote.stage.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with the "Manage stages" permission in a CRM access role

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

`crm.quote.stage.delete` deletes an estimate stage.

Unlike the [crm.&lt;type&gt;.category.stage.delete](./crm-entity-category-stage-delete-rest-v3.md) methods for deals and SPAs, this method deletes a stage from the single set of estimate stages without pipelines.

Only a stage without estimates can be deleted. If a stage has estimates, move them to another stage: specify it in the `stageId` field with [crm.quote.update](../items/crm-entity-update-rest-v3.md). Alternatively, delete the estimates with [crm.quote.delete](../items/crm-entity-delete-rest-v3.md).

## Method Parameters

{% include [Required Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | The stage ID, for example, `UC_9AD50Y`. [ID Format](#stage-id).

System stages with `isSystem: true` cannot be deleted: `DRAFT`, `APPROVED`, and `DECLAINED`. Failed stages without the `isSystem` flag can be deleted ||
|#

### Stage ID {#stage-id}

Estimates have no pipelines, so a stage ID is a code without a pipeline prefix. The stages that Bitrix24 creates with CRM have the codes `DRAFT`, `SENT`, `APPROVED`, `DECLAINED`, and `APOLOGY`. Bitrix24 assigns new stages a code such as `UC_9AD50Y`.

Stage codes are returned by [crm.quote.stage.list](./crm-quote-stage-list-rest-v3.md).

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% note info "" %}

Calls to the new API include the `/api/` segment in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.quote.stage.delete`

{% endnote %}

The examples delete the `UC_9AD50Y` stage.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"UC_9AD50Y"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.stage.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"UC_9AD50Y","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.quote.stage.delete
    ```

- JS (TS)

    CRM methods are not yet included in the REST 3.0 method list recognized by `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageDeleteResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageDeleteResult>(
        'crm.quote.stage.delete',
        {
          id: 'UC_9AD50Y',
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Stage deleted:', result.result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the REST 3.0 method list recognized by `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function deleteStage() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.quote.stage.delete',
            {
              id: 'UC_9AD50Y',
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Stage deleted:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteStage)
    </script>
    ```

- Python

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.quote.stage.delete"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": "UC_9AD50Y",
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.stage.delete',
                [
                    'id' => 'UC_9AD50Y',
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.quote.stage.delete',
        {
            id: 'UC_9AD50Y',
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.stage.delete',
        [
            'id' => 'UC_9AD50Y',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.quote.stage.delete", b24.Params{
    	"id": "UC_9AD50Y",
    })
    if err != nil {
    	return fmt.Errorf("crm.quote.stage.delete: %w", err)
    }

    var deleted struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &deleted); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(deleted.Result)
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
        "start": 1795772513,
        "finish": 1795772513.359252,
        "duration": 0.3592519760131836,
        "processing": 0,
        "date_start": "2026-11-27T12:41:53+03:00",
        "date_finish": "2026-11-27T12:41:53+03:00",
        "operating_reset_at": 1795773060,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object containing the response data ||
|| **result.result**
[`boolean`](../../../data-types.md) | `true` if the stage is deleted ||
|| **time**
[`time`](../../../data-types.md#time) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Move or delete the related items first.",
                "field": "DEPENDENT_ITEMS_EXIST"
            }
        ]
    }
}
```

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | — | Pass a non-empty stage `id` ||
|| `-` | — | Pass a non-empty stage `id`. The method does not support the `filter` parameter ||
|| `STAGE_NOT_FOUND` | Stage was not found | Check `id` with [crm.quote.stage.list](./crm-quote-stage-list-rest-v3.md). The stage with this code does not exist or has already been deleted ||
|| `DEPENDENT_ITEMS_EXIST` | Move or delete the related items first | The stage has estimates. Move them to another stage or delete them ||
|| `OPERATION_NOT_ALLOWED_FOR_STAGE` | This action is not available for this stage | This is a system stage. Only a stage with `isSystem: false` can be deleted ||
|#

If `id` is omitted or empty, the method returns both errors without `field` in a single response. For errors with the codes `STAGE_NOT_FOUND`, `DEPENDENT_ITEMS_EXIST`, and `OPERATION_NOT_ALLOWED_FOR_STAGE`, `field` contains the error code, not a field name. If the stage does not exist, the method returns a request validation error rather than the `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` code.

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a CRM administrator or a user with the "Manage stages" permission in a CRM access role ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase — `crm.quote.stage.delete` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-quote-stage-add-rest-v3.md)
- [{#T}](./crm-quote-stage-list-rest-v3.md)
- [{#T}](./crm-quote-stage-replace-rest-v3.md)
