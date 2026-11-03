# Get an Activity v 3.0 crm.[type].activity.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the activity

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.<type>.activity.get` method returns an activity by ID. The object type is set by the `<type>` segment of the method name: the method finds the activity only if it is bound to at least one item of this type. The `crm.smartprocess1038.activity.get` method will not find a deal activity.

To retrieve an activity regardless of the type, use the [crm.activity.get](./crm-activity-get-rest-v3.md) method.

## Method Name for the Object Type

#|
|| **Object type** | **Method** ||
|| Deal | `crm.deal.activity.get` ||
|| Lead | `crm.lead.activity.get` ||
|| Contact | `crm.contact.activity.get` ||
|| Company | `crm.company.activity.get` ||
|| Estimate | `crm.quote.activity.get` ||
|| Invoice | `crm.smartinvoice.activity.get` ||
|| CRM document | `crm.smartdocument.activity.get` ||
|| SPA | `crm.smartprocess<entityTypeId>.activity.get`, for example, `crm.smartprocess1038.activity.get` ||
|#

For an SPA, the method name includes the type ID `entityTypeId`. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

The method name is written in lowercase: `crm.smartinvoice.activity.get`, not `crm.smartInvoice.activity.get`. If the name is written in a different case, the method returns the ["Method Not Found"](#method-not-found) error.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Activity ID.

The ID can be retrieved with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method ||
|| **select**
[`array`](../../../data-types.md) | List of activity fields to return, for example, `["id","subject","deadlineTime"]`. The available fields are described in the [overview of activity methods](./index.md#fields). For `bindings` and `communications`, you can select nested fields using dot notation: `bindings.entityId`.

Default: all activity fields ||
|#

Document and call list activities cannot be read: for their IDs, the method returns a "not found" error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.smartprocess1038.activity.get`

{% endnote %}

The examples retrieve an activity of an SPA item with the type ID `1038`. For a different object type, replace the method name according to the table above.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8463,"select":["id","type","providerId","subject","isCompleted","assignedById","deadlineTime","bindings"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.smartprocess1038.activity.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8463,"select":["id","type","providerId","subject","isCompleted","assignedById","deadlineTime","bindings"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.smartprocess1038.activity.get
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ActivityGetResult = {
      item: {
        id: number
        type: string
        providerId: string
        subject: string
        isCompleted: boolean
        assignedById: number
        deadlineTime: string | null
        bindings: Array<{ entityTypeId: number; entityId: number }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<ActivityGetResult>(
        'crm.smartprocess1038.activity.get',
        {
          id: 8463,
          select: ['id', 'type', 'providerId', 'subject', 'isCompleted', 'assignedById', 'deadlineTime', 'bindings'],
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activity:', result.item)
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
      async function getActivity() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.smartprocess1038.activity.get',
            {
              id: 8463,
              select: ['id', 'type', 'providerId', 'subject', 'isCompleted', 'assignedById', 'deadlineTime', 'bindings'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Activity:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getActivity)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.smartprocess1038.activity.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 8463,
                "select": ["id", "type", "providerId", "subject", "isCompleted", "assignedById", "deadlineTime", "bindings"],
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
                'crm.smartprocess1038.activity.get',
                [
                    'id' => 8463,
                    'select' => ['id', 'type', 'providerId', 'subject', 'isCompleted', 'assignedById', 'deadlineTime', 'bindings'],
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
        'crm.smartprocess1038.activity.get',
        {
            id: 8463,
            select: ['id', 'type', 'providerId', 'subject', 'isCompleted', 'assignedById', 'deadlineTime', 'bindings'],
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
        'crm.smartprocess1038.activity.get',
        [
            'id' => 8463,
            'select' => ['id', 'type', 'providerId', 'subject', 'isCompleted', 'assignedById', 'deadlineTime', 'bindings'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.smartprocess1038.activity.get", b24.Params{
    	"id":     8463,
    	"select": []string{"id", "type", "providerId", "subject", "isCompleted", "assignedById", "deadlineTime", "bindings"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.smartprocess1038.activity.get: %w", err)
    }

    var result struct {
    	Item struct {
    		ID           int     `json:"id"`
    		Type         string  `json:"type"`
    		ProviderID   string  `json:"providerId"`
    		Subject      string  `json:"subject"`
    		IsCompleted  bool    `json:"isCompleted"`
    		AssignedByID int     `json:"assignedById"`
    		DeadlineTime *string `json:"deadlineTime"`
    		Bindings     []struct {
    			EntityTypeID int `json:"entityTypeId"`
    			EntityID     int `json:"entityId"`
    		} `json:"bindings"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.Subject, result.Item.IsCompleted)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 8463,
            "type": "provider",
            "providerId": "CRM_TODO",
            "subject": "Call the client back",
            "isCompleted": false,
            "assignedById": 1269,
            "deadlineTime": "2026-11-20T18:00:00+03:00",
            "bindings": [
                {
                    "entityTypeId": 1038,
                    "entityId": 41
                }
            ]
        }
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
|| **result.item**
[`object`](../../../data-types.md) | Activity with the fields from `select`; without `select`, with all fields. [Activity field description](./index.md#fields) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 8467 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the activity `id` ||
|| `id` | This request requires data type `int` in field `id` | Pass the ID as a number ||
|#

#### Activity Not Found

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | The activity does not exist, is not bound to an item of this object type, or you have no permission to view it. Check `id` with the [crm.&lt;type&gt;.activity.list](./crm-entity-activity-list-rest-v3.md) method and the method name ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ActivityDto` | Check the field names in `select` against the [activity field table](./index.md#fields) ||
|| `-` | Unknown field `#FIELD#` found in entity `EmployeeDto` | Check the employee field name in `select`, for example, `assignedBy.firstName` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | REST 3.0 activity methods are not available on this Bitrix24 ||
|#

#### Method Not Found {#method-not-found}

Error Code: `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION`, HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Method `#METHOD#` was not found | Check the method name: it is written in lowercase, for example, `crm.deal.activity.get`. For an SPA, check `entityTypeId` with the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. My company and B2E documents have no activity methods ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-entity-activity-list-rest-v3.md)
- [{#T}](./crm-entity-activity-complete-rest-v3.md)
- [{#T}](./crm-activity-binding-list-rest-v3.md)
