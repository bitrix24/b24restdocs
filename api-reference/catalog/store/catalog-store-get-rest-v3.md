# Get Warehouse Field Values v 3.0 catalog.store.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View and select warehouses" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.store.get` method returns the data of a warehouse by its identifier.

The method returns the warehouse if it is available to the user through the "View and select warehouses" access permission. A user with the "Edit warehouses" access permission also receives the warehouses they created.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Warehouse identifier.

The identifier can be retrieved using the [catalog.store.list](./catalog-store-list-rest-v3.md) method ||
|| **select**
[`array`](../../data-types.md) | Array of warehouse fields to return in the response.

Available fields:

- `id` — warehouse identifier
- `title` — warehouse name
- `active` — warehouse activity
- `address` — warehouse address
- `description` — warehouse description
- `gpsN` — warehouse latitude
- `gpsS` — warehouse longitude
- `imageId` — warehouse image file identifier
- `imageUrl` — link to the warehouse image
- `dateModify` — date the warehouse was last modified
- `dateCreate` — warehouse creation date
- `userId` — identifier of the user who created the warehouse
- `modifiedBy` — identifier of the user who last modified the warehouse
- `phone` — warehouse phone number
- `schedule` — warehouse working hours
- `xmlId` — warehouse external ID
- `sort` — warehouse sort order in lists
- `email` — warehouse email
- `issuingCenter` — pickup point flag
- `code` — warehouse symbolic code

The `id` identifier is always returned. The value `["*"]` is not supported.

Default: all fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.store.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"select":["id","title","active","address","phone","schedule","issuingCenter","imageUrl"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.store.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":51,"select":["id","title","active","address","phone","schedule","issuingCenter","imageUrl"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.store.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type StoreGetResult = {
      item: {
        id: number
        title: string
        active: boolean
        address: string
        phone: string
        schedule: string
        issuingCenter: boolean
        imageUrl: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<StoreGetResult>({
        method: 'catalog.store.get',
        params: {
          id: 51,
          select: ['id', 'title', 'active', 'address', 'phone', 'schedule', 'issuingCenter', 'imageUrl'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.title, result.item.address)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getStore() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.store.get',
            params: {
              id: 51,
              select: ['id', 'title', 'active', 'address', 'phone', 'schedule', 'issuingCenter', 'imageUrl'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.title, result.item.address)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getStore)
    </script>
    ```

- Python

    The `catalog.store.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.store.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 51,
                "select": ["id", "title", "active", "address", "phone", "schedule", "issuingCenter", "imageUrl"],
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
                'catalog.store.get',
                [
                    'id' => 51,
                    'select' => ['id', 'title', 'active', 'address', 'phone', 'schedule', 'issuingCenter', 'imageUrl'],
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
        'catalog.store.get',
        {
            id: 51,
            select: ['id', 'title', 'active', 'address', 'phone', 'schedule', 'issuingCenter', 'imageUrl'],
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
        'catalog.store.get',
        [
            'id' => 51,
            'select' => ['id', 'title', 'active', 'address', 'phone', 'schedule', 'issuingCenter', 'imageUrl'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.store.get", b24.Params{
    	"id":     51,
    	"select": []string{"id", "title", "active", "address", "phone", "schedule", "issuingCenter", "imageUrl"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.store.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID            int    `json:"id"`
    	Title         string `json:"title"`
    	Active        bool   `json:"active"`
    	Address       string `json:"address"`
    	IssuingCenter bool   `json:"issuingCenter"`
    	ImageURL      string `json:"imageUrl"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Title, item.Address)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 51,
            "title": "Friedrichstrasse 51 Warehouse",
            "active": true,
            "address": "Friedrichstraße 51, 10117 Berlin, Germany",
            "imageUrl": "https://cdn-com.bitrix24.com/b1234567/catalog/fee/fee4295e698466f04b617988b6f415ff/508cb2bfea465e513a642ec8849f1023.png",
            "phone": "+49 30 1111111",
            "schedule": "Mon–Sat 9:00–20:00",
            "issuingCenter": true
        }
    },
    "time": {
        "start": 1794922633,
        "finish": 1794922633.508727,
        "duration": 0.5087270736694336,
        "processing": 0,
        "date_start": "2026-11-17T16:37:13+02:00",
        "date_finish": "2026-11-17T16:37:13+02:00",
        "operating_reset_at": 1794923220,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Warehouse data. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Warehouse identifier ||
|| **title**
[`string`](../../data-types.md) | Warehouse name. An empty string if the name is not set ||
|| **active**
[`boolean`](../../data-types.md) | Warehouse activity: `true` — the warehouse is active, `false` — inactive ||
|| **address**
[`string`](../../data-types.md) | Warehouse address ||
|| **description**
[`string`](../../data-types.md) | Warehouse description. An empty string if the description is not set ||
|| **gpsN**
[`double`](../../data-types.md) | Warehouse latitude. If the coordinate is not set, `0` is returned. If Bitrix24 stores a non-numeric value, such as an empty string, `null` is returned ||
|| **gpsS**
[`double`](../../data-types.md) | Warehouse longitude. If the coordinate is not set, `0` is returned. If Bitrix24 stores a non-numeric value, such as an empty string, `null` is returned ||
|| **imageId**
[`integer`](../../data-types.md) | Warehouse image file identifier. `0` if there is no image ||
|| **imageUrl**
[`string`](../../data-types.md) | Link to the warehouse image. An empty string if there is no image ||
|| **dateModify**
[`datetime`](../../data-types.md) | Date the warehouse was last modified ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Warehouse creation date. Can be `null` for warehouses that Bitrix24 created itself ||
|| **userId**
[`integer`](../../data-types.md) | Identifier of the user who created the warehouse. `0` for warehouses that Bitrix24 created itself ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the warehouse ||
|| **phone**
[`string`](../../data-types.md) | Warehouse phone number. An empty string if the phone number is not set ||
|| **schedule**
[`string`](../../data-types.md) | Warehouse working hours. An empty string if the working hours are not set ||
|| **xmlId**
[`string`](../../data-types.md) | Warehouse external ID. An empty string if the ID is not set ||
|| **sort**
[`integer`](../../data-types.md) | Warehouse sort order in lists ||
|| **email**
[`string`](../../data-types.md) | Warehouse email. An empty string if the address is not set ||
|| **issuingCenter**
[`boolean`](../../data-types.md) | Pickup point flag: `true` — the warehouse operates as a pickup point, `false` — a regular warehouse ||
|| **code**
[`string`](../../data-types.md) | Warehouse symbolic code. An empty string if the code is not set ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 999 was not found."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass the warehouse `id` as a positive integer ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no warehouse with this `id`, or it has been deleted. Check the identifier using the [catalog.store.list](./catalog-store-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `StoreDto` | Specify only the fields from the [item object description](#item) in `select` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have the "View product catalog" or "View and select warehouses" access permission, or the warehouse is not available to them ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-store-add-rest-v3.md)
- [{#T}](./catalog-store-update-rest-v3.md)
- [{#T}](./catalog-store-list-rest-v3.md)
- [{#T}](./catalog-store-delete-rest-v3.md)
- [{#T}](./catalog-store-field-list.md)
