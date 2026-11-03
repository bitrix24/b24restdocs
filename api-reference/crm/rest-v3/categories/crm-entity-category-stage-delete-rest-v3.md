# Delete a CRM Pipeline Stage v 3.0 crm.[type].category.stage.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a CRM administrator or a user with the "Manage stages" permission in a CRM access role. For an SPA of a digital workplace — an administrator of this digital workplace or a user with the "Manage stages" permission in a digital workplace access role. The user must also have permission to read the items of this pipeline

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.category.stage.delete` method deletes a stage from a pipeline. The object type is set by the `<type>` segment of the method name.

Only a stage that has no items can be deleted. First move the items to another stage by setting `stageId` with the [crm.&lt;type&gt;.update](../items/crm-entity-update-rest-v3.md) method, or delete them with the [crm.&lt;type&gt;.delete](../items/crm-entity-delete-rest-v3.md) method. System stages with `isSystem: true` — the first stage, the successful stage, and the main failed stage — cannot be deleted.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.category.stage.delete` ||
|| SPA | `crm.smartprocess<entityTypeId>.category.stage.delete`, for example, `crm.smartprocess1038.category.stage.delete` ||
|#

For an SPA, the method name includes its type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

Deals have pipeline stages, and so does an SPA if both pipelines and stages are enabled in its type settings. Leads, contacts, companies, estimates, invoices, and my company have no pipeline stage methods.

The method name is written in lowercase: `crm.smartprocess1038.category.stage.delete`, not `crm.smartProcess1038.category.stage.delete`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | Stage ID, for example, `DT1038_127:UC_WUHOJE`. [ID format](#stage-id).

Stage IDs are returned by the [crm.&lt;type&gt;.category.stage.list](./crm-entity-category-stage-list-rest-v3.md) method ||
|#

### Stage ID {#stage-id}

The stage ID format depends on the pipeline:

#|
|| **Pipeline** | **Format** | **Example** ||
|| Default deal pipeline | Stage code without a prefix | `NEW` ||
|| Other deal pipeline | `C<categoryId>:<code>` | `C1:NEW` ||
|| SPA pipeline | `DT<entityTypeId>_<categoryId>:<code>` | `DT1038_127:UC_WUHOJE` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.category.stage.delete`

{% endnote %}

The examples delete the `DT1038_127:UC_WUHOJE` stage of an SPA with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"DT1038_127:UC_WUHOJE"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.category.stage.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"DT1038_127:UC_WUHOJE","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.category.stage.delete
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StageDeleteResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<StageDeleteResult>(
        'crm.smartprocess1038.category.stage.delete',
        {
          id: 'DT1038_127:UC_WUHOJE',
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

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function deleteStage() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.category.stage.delete',
            {
              id: 'DT1038_127:UC_WUHOJE',
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

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.category.stage.delete"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": "DT1038_127:UC_WUHOJE",
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.smartprocess1038.category.stage.delete',
                [
                    'id' => 'DT1038_127:UC_WUHOJE',
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
        'crm.smartprocess1038.category.stage.delete',
        {
            id: 'DT1038_127:UC_WUHOJE',
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
        'crm.smartprocess1038.category.stage.delete',
        [
            'id' => 'DT1038_127:UC_WUHOJE',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.category.stage.delete", b24.Params{
    	"id": "DT1038_127:UC_WUHOJE",
    })
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.category.stage.delete: %w", err)
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
        "start": 1793715167,
        "finish": 1793715167.650605,
        "duration": 0.6506049633026123,
        "processing": 0,
        "date_start": "2026-11-03T17:12:47+03:00",
        "date_finish": "2026-11-03T17:12:47+03:00",
        "operating_reset_at": 1793715720,
        "operating": 0
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
[`boolean`](../../../data-types.md) | `true` if the stage is deleted ||
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
                "message": "Move or delete the related items first.",
                "field": "DEPENDENT_ITEMS_EXIST"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the stage `id` ||
|| `id` | Field value must not be empty | Pass a non-empty `id` ||
|| `STAGE_NOT_FOUND` | Stage was not found | Check `id` with the [crm.&lt;type&gt;.category.stage.list](./crm-entity-category-stage-list-rest-v3.md) method. This object type has no such stage, the stage has been deleted, or the user has no permission to read the items of its pipeline ||
|| `DEPENDENT_ITEMS_EXIST` | Move or delete the related items first | The stage has items. Move them to another stage or delete them ||
|| `OPERATION_NOT_ALLOWED_FOR_STAGE` | This action is not available for this stage | This is a system stage. Only a stage with `isSystem: false` can be deleted ||
|#

For errors with the `STAGE_NOT_FOUND`, `DEPENDENT_ITEMS_EXIST`, and `OPERATION_NOT_ALLOWED_FOR_STAGE` codes, `field` contains the error code, not a field name. If the stage does not exist, the method returns a request validation error with the `STAGE_NOT_FOUND` code in `field` instead of the `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` code.

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Call the method on behalf of a CRM administrator or a user with the "Manage stages" permission in an access role ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.smartprocess1038.category.stage.delete`. Only deals and SPAs have stage methods. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method and whether pipelines and stages are enabled in the type ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-category-stage-list-rest-v3.md)
- [{#T}](./crm-entity-category-stage-replace-rest-v3.md)
- [{#T}](./crm-entity-category-stage-add-rest-v3.md)
