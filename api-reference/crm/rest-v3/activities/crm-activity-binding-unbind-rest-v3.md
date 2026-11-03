# Unbind an Activity from a CRM Item v 3.0 crm.activity.binding.unbind

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the activity and the CRM item the activity is unbound from

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.activity.binding.unbind` method removes an activity binding to a CRM item and returns the remaining bindings. If the activity is not bound to the item, the method changes nothing and returns the current bindings.

The last binding cannot be removed: an activity always has at least one item. To bind the activity to another item, use the [crm.activity.binding.move](./crm-activity-binding-move-rest-v3.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **activityId***
[`integer`](../../../data-types.md) | Activity ID.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method ||
|| **parent***
[`object`](../../../data-types.md) | CRM item in the `{"entityTypeId": 2, "entityId": 9001}` format: `entityTypeId` is the object type ID, `entityId` is the item ID. The activity will be unbound from this item.

Object type IDs are returned by the [crm.dictionary.entitytype.list](../dictionaries/crm-dictionary-list-rest-v3.md) method, and for an SPA by [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.activity.binding.unbind`

{% endnote %}

The examples unbind the activity from the deal with the ID `9001`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":8463,"parent":{"entityTypeId":2,"entityId":9001}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.binding.unbind
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"activityId":8463,"parent":{"entityTypeId":2,"entityId":9001},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.activity.binding.unbind
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ActivityUnbindResult = {
      items: Array<{
        entityTypeId: number
        entityId: number
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ActivityUnbindResult>(
        'crm.activity.binding.unbind',
        {
          activityId: 8463,
          parent: {
            entityTypeId: 2,
            entityId: 9001,
          },
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Bindings:', result.items)
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
      async function unbindActivity() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.activity.binding.unbind',
            {
              activityId: 8463,
              parent: {
                entityTypeId: 2,
                entityId: 9001,
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Bindings:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', unbindActivity)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.activity.binding.unbind"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "activityId": 8463,
                "parent": {
                    "entityTypeId": 2,
                    "entityId": 9001,
                },
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.binding.unbind',
                [
                    'activityId' => 8463,
                    'parent' => [
                        'entityTypeId' => 2,
                        'entityId' => 9001,
                    ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'crm.activity.binding.unbind',
        {
            activityId: 8463,
            parent: {
                entityTypeId: 2,
                entityId: 9001,
            },
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.binding.unbind',
        [
            'activityId' => 8463,
            'parent' => [
                'entityTypeId' => 2,
                'entityId' => 9001,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.binding.unbind", b24.Params{
    	"activityId": 8463,
    	"parent": b24.Params{
    		"entityTypeId": 2,
    		"entityId":     9001,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.binding.unbind: %w", err)
    }

    var result struct {
    	Items []struct {
    		EntityTypeID int `json:"entityTypeId"`
    		EntityID     int `json:"entityId"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.EntityTypeID, item.EntityID)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "entityTypeId": 1038,
                "entityId": 41
            }
        ]
    },
    "time": {
        "start": 1793711360,
        "finish": 1793711360.764732,
        "duration": 0.7647318840026855,
        "processing": 0,
        "date_start": "2026-11-03T16:09:20+03:00",
        "date_finish": "2026-11-03T16:09:20+03:00",
        "operating_reset_at": 1793711940,
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
|| **result.items**
[`object[]`](../../../data-types.md) | Activity bindings after the operation: `entityTypeId` is the object type, `entityId` is the item ID. Bindings to items you cannot view are not included in the response but are retained ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "CRM_ACTIVITY_WITHOUT_BINDINGS",
        "message": "Cannot remove the activity's only association. The association can only be moved to another CRM item."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `activityId` | Required field `activityId` is missing | Pass `activityId` ||
|| `activityId` | Field value must be 1 or greater | Pass a positive activity ID ||
|| `parent` | Required field `parent` is missing | Pass `parent` as an object ||
|| `parent` | The parent must be an object of entityTypeId and entityId. | Pass `parent` as an object with the `entityTypeId` and `entityId` fields, not as an array ||
|| `parent.entityId` | Required field `parent.entityId` is missing | Pass both fields: `entityTypeId` and `entityId` ||
|| `parent.entityTypeId` | This request requires data type `integer` in field `parent.entityTypeId` | Pass the IDs as numbers, not strings ||
|| `parent.entityTypeId` | The type of the parent is not defined. | Pass an existing object type ID ||
|| `parent.entityId` | The identifier of the parent must be positive. | Pass a positive item ID ||
|#

#### Last Binding

Error Code: `CRM_ACTIVITY_WITHOUT_BINDINGS`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Cannot remove the activity's only association. The association can only be moved to another CRM item | Move the binding with the [crm.activity.binding.move](./crm-activity-binding-move-rest-v3.md) method or delete the activity with the [crm.activity.delete](./crm-activity-delete-rest-v3.md) method ||
|#

#### Activity Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | The activity does not exist or you have no permission to edit it. Check `activityId` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The item does not exist or you have no permission to edit it. Check the item ID and permissions ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name, for example, `crm.activity.binding.unbind` ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-binding-bind-rest-v3.md)
- [{#T}](./crm-activity-binding-move-rest-v3.md)
- [{#T}](./crm-activity-binding-list-rest-v3.md)
