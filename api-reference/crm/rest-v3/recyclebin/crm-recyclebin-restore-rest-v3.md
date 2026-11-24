# Restore an Item from the CRM Recycle Bin v 3.0 crm.recyclebin.restore

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: the user who deleted the item, or an administrator

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `crm.recyclebin.restore` method restores a deleted CRM item from the Recycle Bin by the Recycle Bin record ID.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **recordId***
[`integer`](../../../data-types.md) | Recycle Bin record ID. Do not confuse it with the item ID before deletion: these are different numbers.

The record ID can be retrieved with the [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md) method in the `recordId` field ||
|#

{% note warning "" %}

A restored item gets a new ID. Take it from `result.item.id` in the response, not from the `id` field of the Recycle Bin record.

{% endnote %}

{% note info "" %}

You can restore only a record that [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md) returns to this user: a user other than an administrator sees only the items they deleted themselves. Records older than the retention period — 30 days in cloud Bitrix24 — cannot be restored. After restoration, the record is removed from the Recycle Bin.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/crm.recyclebin.restore`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"recordId":14533}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/crm.recyclebin.restore
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"recordId":14533,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/crm.recyclebin.restore
    ```

- JS (TS)

    CRM methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RecyclebinRestoreResult = {
      item: {
        entityTypeId: number
        id: number
        url: string
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<RecyclebinRestoreResult>(
        'crm.recyclebin.restore',
        {
          recordId: 14533,
        },
        Text.getUuidRfc4122()
      )

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Restored item ID:', result.item.id)
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
      async function restoreItem() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'crm.recyclebin.restore',
            {
              recordId: 14533,
            },
            B24Js.Text.getUuidRfc4122()
          )

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Restored item ID:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', restoreItem)
    </script>
    ```

- Python

    `b24pysdk` has no ready-made wrappers for CRM methods, and the SDK sends to REST 3.0 only the methods from the `Config().api_v3_methods` list. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "crm.recyclebin.restore"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "recordId": 14533,
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["item"])
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
                'crm.recyclebin.restore',
                [
                    'recordId' => 14533,
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
        'crm.recyclebin.restore',
        {
            recordId: 14533,
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
        'crm.recyclebin.restore',
        [
            'recordId' => 14533,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.recyclebin.restore", b24.Params{
    	"recordId": 14533,
    })
    if err != nil {
    	return fmt.Errorf("crm.recyclebin.restore: %w", err)
    }

    var out struct {
    	Item struct {
    		EntityTypeID int    `json:"entityTypeId"`
    		ID           int    `json:"id"`
    		URL          string `json:"url"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(out.Item.EntityTypeID, out.Item.ID, out.Item.URL)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "entityTypeId": 2,
            "id": 9205,
            "url": "/crm/deal/details/9205/"
        }
    },
    "time": {
        "start": 1795431190,
        "finish": 1795431191.112406,
        "duration": 1.1124060153961182,
        "processing": 1,
        "date_start": "2026-11-23T11:53:10+01:00",
        "date_finish": "2026-11-23T11:53:11+01:00",
        "operating_reset_at": 1795431780,
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
[`object`](../../../data-types.md) | Restored item. [Object description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### The item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ID: `1` — lead, `2` — deal, `3` — contact, `4` — company, or an SPA type ID ||
|| **id**
[`integer`](../../../data-types.md) | ID of the restored item. It differs from the item ID before deletion. The item can be retrieved with the [crm.&lt;type&gt;.get](../items/crm-entity-get-rest-v3.md) method ||
|| **url**
[`string`](../../../data-types.md) | Relative URL of the item card, for example, `/crm/deal/details/9205/`. For an SPA, the URL contains the `categoryId` pipeline parameter ||
|#

{% note info "" %}

The change history and stage history recorded before deletion remain with the previous ID and are not returned by the [crm.&lt;type&gt;.history.list](../history/crm-entity-history-list-rest-v3.md) and [crm.&lt;type&gt;.stagehistory.list](../history/crm-entity-stage-history-list-rest-v3.md) methods. The stage history of the restored item starts from its current stage.

{% endnote %}

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Recycle bin item is unavailable.",
                "field": "RECYCLEBIN_ITEM_UNAVAILABLE"
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
|| `recordId` | Required field `recordId` is missing | Pass `recordId` ||
|| `recordId` | This request requires data type `int` in field `recordId` | Pass the record ID as a number ||
|| `recordId` | Field value must be 1 or greater | Pass a positive record ID ||
|#

#### Restoration Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`. The reason code comes in `validation.field`.

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `RECYCLEBIN_ITEM_UNAVAILABLE` | Recycle bin item is unavailable. | The record is not in the Recycle Bin, it has already been restored, it is older than the retention period, it was deleted by another user, the Recycle Bin is disabled for the object type, or the Recycle Bin is not available on the plan. Retrieve the available records with the [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md) method ||
|| `RECYCLEBIN_RESTORE_FAILED` | Could not restore recycle bin item. | The restoration failed. Some item data may have been restored: check the Recycle Bin with the [crm.recyclebin.list](./crm-recyclebin-list-rest-v3.md) method and the item by its title with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method before retrying the call ||
|| `RECYCLEBIN_INVALID_RESTORE_RESULT` | Core restore did not return an item ID. | The Recycle Bin did not return the ID of the restored item. Find the item by its title with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| `RECYCLEBIN_INVALID_RESTORE_RESULT` | The restored item is unavailable. | The restored item is not available. Find the item by its title with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|| `RECYCLEBIN_RESTORE_URL_UNAVAILABLE` | The restored item URL is unavailable. | The item was restored, but the card URL was not retrieved. Find the item by its title with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`, HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | REST 3.0 CRM Recycle Bin methods are not available on this Bitrix24, or the call was made without an authorized user ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-recyclebin-list-rest-v3.md)
- [{#T}](../items/crm-entity-get-rest-v3.md)
- [{#T}](../items/crm-entity-delete-rest-v3.md)
