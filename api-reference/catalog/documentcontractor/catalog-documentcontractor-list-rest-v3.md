# Get List of Vendor Bindings to Documents v 3.0 catalog.document.contractor.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View" permission for documents of the "Incoming" type and the "View and select warehouses" and "View product catalog" permissions

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.document.contractor.list` returns the list of CRM vendor bindings to incoming documents.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | Array of binding fields to return in the response.

Available fields:

- `id` — binding identifier
- `documentId` — document identifier
- `entityTypeId` — CRM object type
- `entityId` — CRM object identifier

The `id` field is returned only if it is specified in `select`. The value `["*"]` is not supported.

By default: all fields ||
|| **filter**
[`array`](../../data-types.md) | Filter in REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`, for example `[["documentId", "=", 325]]`. The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter).

You can filter by all fields from the `select` parameter. Condition groups with `or` logic and negation are not supported. An `in` condition with an empty list returns an empty list.

The method returns bindings only for documents available to the user ||
|| **order**
[`object`](../../data-types.md) | Sorting in the format `{"field": "direction"}`, for example `{"id": "asc"}`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

You can sort by all fields from the `select` parameter. Without `order`, the order of records is not guaranteed ||
|| **pagination**
[`object`](../../data-types.md) | Pagination. [Object description](#pagination) ||
|#

### Parameter Pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of records per page. Values greater than `1000` are reduced to `1000`, the value `0` causes an error.

By default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. If passed, it replaces `offset`. A page beyond the end of the list returns an empty array ||
|| **offset**
[`integer`](../../data-types.md) | Number of records to skip ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.contractor.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["documentId","=",325]],"select":["id","documentId","entityTypeId","entityId"],"order":{"id":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.contractor.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["documentId","=",325]],"select":["id","documentId","entityTypeId","entityId"],"order":{"id":"asc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.contractor.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentContractorListResult = {
      items: Array<{
        id?: number
        documentId?: number
        entityTypeId?: number
        entityId?: number
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentContractorListResult>({
        method: 'catalog.document.contractor.list',
        params: {
          filter: [
            ['documentId', '=', 325],
          ],
          select: ['id', 'documentId', 'entityTypeId', 'entityId'],
          order: {
            id: 'asc',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Contractors:', result.items)
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
      async function documentContractorList() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.contractor.list',
            params: {
              filter: [
                ['documentId', '=', 325],
              ],
              select: ['id', 'documentId', 'entityTypeId', 'entityId'],
              order: {
                id: 'asc',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Contractors:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', documentContractorList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.contractor.list(
            filter=[
                ["documentId", "=", 325],
            ],
            select=["id", "documentId", "entityTypeId", "entityId"],
            order={
                "id": "asc",
            },
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
                'catalog.document.contractor.list',
                [
                    'filter' => [
                        ['documentId', '=', 325],
                    ],
                    'select' => ['id', 'documentId', 'entityTypeId', 'entityId'],
                    'order' => [
                        'id' => 'asc',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.document.contractor.list',
        {
            filter: [
                ['documentId', '=', 325],
            ],
            select: ['id', 'documentId', 'entityTypeId', 'entityId'],
            order: {
                id: 'asc',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.contractor.list',
        [
            'filter' => [
                ['documentId', '=', 325],
            ],
            'select' => ['id', 'documentId', 'entityTypeId', 'entityId'],
            'order' => [
                'id' => 'asc',
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
    res, err := client.Core().Call(ctx, "catalog.document.contractor.list", b24.Params{
    	"filter": []any{
    		[]any{"documentId", "=", 325},
    	},
    	"select": []string{"id", "documentId", "entityTypeId", "entityId"},
    	"order":  b24.Params{
    		"id": "asc",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.document.contractor.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID           int `json:"id"`
    		EntityTypeID int `json:"entityTypeId"`
    		EntityID     int `json:"entityId"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.EntityTypeID, item.EntityID)
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
                "id": 25,
                "documentId": 325,
                "entityTypeId": 4,
                "entityId": 1
            },
            {
                "id": 29,
                "documentId": 325,
                "entityTypeId": 3,
                "entityId": 1
            }
        ]
    },
    "time": {
        "start": 1792665377,
        "finish": 1792665377.15777,
        "duration": 0.15776991844177246,
        "processing": 0,
        "date_start": "2026-10-22T13:36:17+02:00",
        "date_finish": "2026-10-22T13:36:17+02:00",
        "operating_reset_at": 1792665960,
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
|| **result.items**
[`array`](../../data-types.md) | Array of bindings. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Binding identifier ||
|| **documentId**
[`integer`](../../data-types.md) | Inventory document identifier ||
|| **entityTypeId**
[`integer`](../../data-types.md) | CRM object type: `3` — contact, `4` — company ||
|| **entityId**
[`integer`](../../data-types.md) | Identifier of the contact or company ||
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
                "message": "Filter OR groups are not supported"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Filter OR groups are not supported | Remove groups with `or` logic from `filter` ||
|| `-` | Contractors should be provided by CRM | Document vendors are not taken from CRM. The methods work only if inventory document vendors are managed in CRM ||
|| `-` | CRM module is required for document contractors | The methods work only if the CRM module is installed ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `#FIELD#` found in entity `DocumentContractorDto` | Specify only fields from the `select` parameter description ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass a number from `1` to `1000` in `limit` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for incoming documents, warehouses, and the catalog ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-documentcontractor-add-rest-v3.md)
- [{#T}](./catalog-documentcontractor-delete-rest-v3.md)
- [{#T}](./catalog-documentcontractor-field-list.md)
- [{#T}](./index.md)
