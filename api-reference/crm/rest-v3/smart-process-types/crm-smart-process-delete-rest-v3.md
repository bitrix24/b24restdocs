# Delete an SPA Type v 3.0 crm.smartprocess.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with permission to manage this SPA in CRM access permissions

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 Overview](../../../rest-v3.md).

{% endnote %}

Deletes an SPA type by its `entityTypeId` using `crm.smartprocess.delete`.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | SPA type ID, for example, `1038`. The value must be greater than `0`.

Type IDs are returned by the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) method ||
|#

The method accepts no parameters other than `entityTypeId`: any extra parameter causes a [Validation Error](#request-validation).

A type can be deleted only if it has no items. Delete items with the [`crm.smartprocess<entityTypeId>.delete`](../items/crm-entity-delete-rest-v3.md) method, for example, `crm.smartprocess1038.delete`, or the previous-version method [crm.item.delete](../../universal/crm-item-delete.md). Items in the Recycle Bin do not prevent type deletion.

The `relations` links to other types, `linkedUserFields` bindings, and placement in digital workplaces do not prevent deletion: Bitrix24 removes them along with the type. If the type is connected to a CRM binding or serves as a source of binding objects, the method returns the `TYPE_USED_IN_BINDING` [Error](#request-validation): first disconnect the type from the binding in CRM settings.

The method name is written in lowercase: `crm.smartprocess.delete`. For the name `crm.smartProcess.delete`, the method returns the ["Method Not Found"](#method-not-found) error.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess.delete`

{% endnote %}

The examples delete the SPA type with `entityTypeId = 1038`. Substitute your type ID from the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) response.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1038}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1038,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess.delete
    ```

- JS (TS)

    CRM methods are not yet included in the REST 3.0 method list known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SmartProcessDeleteResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<SmartProcessDeleteResult>(
        'crm.smartprocess.delete',
        {
          entityTypeId: 1038,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info('Deleted:', response.getData()!.result.result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    CRM methods are not yet included in the REST 3.0 method list known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function deleteSmartProcessType() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess.delete',
            {
              entityTypeId: 1038,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info('Deleted:', response.getData().result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteSmartProcessType)
    </script>
    ```

- Python

    CRM methods have no ready-made wrappers in `b24pysdk`, and the SDK sends only methods from the `Config().api_v3_methods` list to REST 3.0. Add the method to this list and call it through `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess.delete"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "entityTypeId": 1038,
            },
            prefer_version=3,
        )
        print("Deleted:", response["result"]["result"])
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess.delete',
                [
                    'entityTypeId' => 1038,
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```js
    BX24.callMethod(
        'crm.smartprocess.delete',
        {
            entityTypeId: 1038,
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

    The SDK does not yet support calls to `/rest/api/`. Use a direct HTTP request, for example, through cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.smartprocess.delete',
        [
            'entityTypeId' => 1038,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx have already been created — see the "SDK for Go" section
    res, err := client.Core().Call(ctx, "crm.smartprocess.delete", b24.Params{
    	"entityTypeId": 1038,
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess.delete: %w", err)
    }

    var deleted struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &deleted); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("Deleted:", deleted.Result)
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
        "start": 1795772819,
        "finish": 1795772819.819501,
        "duration": 0.8195009231567383,
        "processing": 0,
        "date_start": "2026-11-27T12:46:59+03:00",
        "date_finish": "2026-11-27T12:46:59+03:00",
        "operating_reset_at": 1795772820,
        "operating": 1.2481939792633057
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **result.result**
[`boolean`](../../../data-types.md) | `true` if the type was deleted ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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
                "message": "Smart process type contains items. Delete the items before deleting the type.",
                "field": "TYPE_HAS_ITEMS"
            }
        ]
    }
}
```

{% include notitle [Error Handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Type Not Found {#not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Entity with ID `0` was not found | There is no type with this `entityTypeId`, it has already been deleted, it is a system type, for example, `2` for a deal or `31` for an invoice, or the user has no permission to manage this SPA. Check the ID with the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) method. The error message always contains `0`, not the passed ID ||
|#

#### Request Validation Errors {#request-validation}

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `entityTypeId` | Required field `entityTypeId` is not specified | Pass `entityTypeId` ||
|| `entityTypeId` | Field value must be 1 or greater | Pass a positive type ID ||
|| `entityTypeId` | This request requires data type `int` in field `entityTypeId` | Pass the ID as an integer, for example, `1038`, not a string ||
|| Extra parameter name, for example, `foo` | Delete accepts only entityTypeId. | Keep only `entityTypeId` in the request ||
|| `TYPE_HAS_ITEMS` | Smart process type contains items. Delete the items before deleting the type. | Delete the items of the type with the [`crm.smartprocess<entityTypeId>.delete`](../items/crm-entity-delete-rest-v3.md) method and repeat the request ||
|| `TYPE_USED_IN_BINDING` | Smart process type is used in bindings. Remove the bindings before deleting it. | The type is connected to a CRM binding or serves as a source of binding objects. Disconnect the type from all bindings in CRM settings and repeat the request ||
|| `RESTRICTION` | Your plan does not allow deleting smart process types. | The Bitrix24 plan does not allow changes to this SPA's settings ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status **404**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Method `crm.smartProcess.delete` was not found | Write the method name in lowercase: `crm.smartprocess.delete` ||
|#

#### Internal Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INTERNAL_INTERNALEXCEPTION`, HTTP status **500**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Something's gone wrong | Bitrix24 did not finish deleting the type. Check whether the type still exists with the [crm.smartprocess.get](./crm-smart-process-get-rest-v3.md) method, and repeat the request ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-smart-process-add-rest-v3.md)
- [{#T}](./crm-smart-process-update-rest-v3.md)
- [{#T}](./crm-smart-process-list-rest-v3.md)
- [{#T}](../items/crm-entity-delete-rest-v3.md)
