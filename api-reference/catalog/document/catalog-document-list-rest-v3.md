# Get List of Inventory Documents v 3.0 catalog.document.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View and select warehouses" or "View product catalog" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.document.list` method returns a list of inventory documents by filter.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of document fields to return in the response.

Available fields:

- `id` — document identifier
- `docType` — document type
- `siteId` — site identifier
- `responsibleId` — identifier of the responsible person
- `dateModify` — modification date
- `dateCreate` — creation date
- `createdBy` — created by
- `modifiedBy` — modified by
- `currency` — currency
- `status` — processing status
- `dateStatus` — status change date
- `dateDocument` — document date
- `statusBy` — status changed by
- `total` — total
- `commentary` — comment
- `title` — title
- `docNumber` — incoming document number
- `contractorId` — vendor identifier

The `id` field is always returned, even if it is not in `select`. The value `["*"]` and uppercase names, for example `ID`, cause an unknown field error.

Default: all fields ||
|| **filter**
[`array`](../../data-types.md) | A list filter in REST 3.0 format: an array of conditions like `["field", "operator", "value"]`, for example `[["docType", "=", "A"]]`. The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter).

You can filter by any field listed in the `select` parameter description.

Supported operators: `=`, `!=`, `>`, `>=`, `<`, `<=`, `in`, `between`. The `like` operator is not supported.

Pass the processing status `status` in the filter as a boolean: `true` — processed, `false` — not processed. The strings `Y` and `N` cause an error.

If vendors are managed in CRM, the filter by `contractorId` causes an error — use the [catalog.document.contractor.list](../documentcontractor/catalog-documentcontractor-list-rest-v3.md) method.

The method returns only documents of the types for which the user has the "View" permission ||
|| **order**
[`object`](../../data-types.md) | Sort order in the format `{"field": "direction"}`, for example `{"id": "desc"}`.

Possible direction values:

- `asc` — ascending
- `desc` — descending

You can sort by any field listed in the `select` parameter description ||
|| **pagination**
[`object`](../../data-types.md) | Pagination. [Parameter description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of documents per page. The value `0` causes an error.

Default: `50` ||
|| **page**
[`integer`](../../data-types.md) | Page number, starting from `1`. A page beyond the list returns an empty array ||
|| **offset**
[`integer`](../../data-types.md) | Number of documents to skip from the beginning of the list ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","docType","title","status","total"],"filter":[["docType","=","A"],["status","=",false]],"order":{"id":"desc"},"pagination":{"limit":2}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","docType","title","status","total"],"filter":[["docType","=","A"],["status","=",false]],"order":{"id":"desc"},"pagination":{"limit":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentListResult = {
      items: Array<{
        id: number
        docType: string
        status: boolean
        total: number
        title: string
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentListResult>({
        method: 'catalog.document.list',
        params: {
          select: ['id', 'docType', 'title', 'status', 'total'],
          filter: [
            ['docType', '=', 'A'],
            ['status', '=', false],
          ],
          order: {
            id: 'desc',
          },
          pagination: {
            limit: 2,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Documents:', result.items.length)
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
      async function callDocumentList() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.list',
            params: {
              select: ['id', 'docType', 'title', 'status', 'total'],
              filter: [
                ['docType', '=', 'A'],
                ['status', '=', false],
              ],
              order: {
                id: 'desc',
              },
              pagination: {
                limit: 2,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Documents:', result.items.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', callDocumentList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.list(
            select=["id", "docType", "title", "status", "total"],
            filter=[
                ["docType", "=", "A"],
                ["status", "=", False],
            ],
            order={
                "id": "desc",
            },
            pagination={
                "limit": 2,
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
                'catalog.document.list',
                [
                    'select' => ['id', 'docType', 'title', 'status', 'total'],
                    'filter' => [
                        ['docType', '=', 'A'],
                        ['status', '=', false],
                    ],
                    'order' => [
                        'id' => 'desc',
                    ],
                    'pagination' => [
                        'limit' => 2,
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
        'catalog.document.list',
        {
            select: ['id', 'docType', 'title', 'status', 'total'],
            filter: [
                ['docType', '=', 'A'],
                ['status', '=', false],
            ],
            order: {
                id: 'desc',
            },
            pagination: {
                limit: 2,
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
        'catalog.document.list',
        [
            'select' => ['id', 'docType', 'title', 'status', 'total'],
            'filter' => [
                ['docType', '=', 'A'],
                ['status', '=', false],
            ],
            'order' => [
                'id' => 'desc',
            ],
            'pagination' => [
                'limit' => 2,
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
    res, err := client.Core().Call(ctx, "catalog.document.list", b24.Params{
    	"select":     []string{"id", "docType", "title", "status", "total"},
    	"filter":     []any{
    		[]string{"docType", "=", "A"},
    		[]any{"status", "=", false},
    	},
    	"order":      b24.Params{
    		"id": "desc",
    	},
    	"pagination": b24.Params{
    		"limit": 2,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.document.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID      int     `json:"id"`
    		DocType string  `json:"docType"`
    		Title   string  `json:"title"`
    		Status  bool    `json:"status"`
    		Total   float64 `json:"total"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Title, item.Status)
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
                "id": 343,
                "docType": "A",
                "status": false,
                "total": 15000,
                "title": "Receipt from vendor Alpha GmbH"
            },
            {
                "id": 319,
                "docType": "A",
                "status": false,
                "total": 8200,
                "title": "Receipt from vendor Beta GmbH"
            }
        ]
    },
    "time": {
        "start": 1792665268,
        "finish": 1792665268.074479,
        "duration": 0.0744791030883789,
        "processing": 0,
        "date_start": "2026-10-22T13:34:28+02:00",
        "date_finish": "2026-10-22T13:34:28+02:00",
        "operating_reset_at": 1792665840,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object with the response data ||
|| **result.items**
[`array`](../../data-types.md) | An array of documents. The set of fields of each document depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Items Element {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Document identifier ||
|| **docType**
[`string`](../../data-types.md) | Document type: `A` — receipt, `S` — stock adjustment, `M` — transfer, `R` — return, `D` — write-off ||
|| **siteId**
[`string`](../../data-types.md) | Site identifier, for example `s1` ||
|| **responsibleId**
[`integer`](../../data-types.md) | Identifier of the responsible person ||
|| **dateModify**
[`datetime`](../../data-types.md) | Date and time of the last change ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date and time of creation ||
|| **createdBy**
[`integer`](../../data-types.md) | Identifier of the user who created the document ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the document ||
|| **currency**
[`string`](../../data-types.md) | Document currency code, for example `EUR` ||
|| **status**
[`boolean`](../../data-types.md) | Processing status: `true` — the document is processed, `false` — not processed ||
|| **dateStatus**
[`datetime`](../../data-types.md) | Date and time of the last status change ||
|| **dateDocument**
[`datetime`](../../data-types.md) or `null` | Document date. `null` if the date is not set ||
|| **statusBy**
[`integer`](../../data-types.md) or `null` | Identifier of the user who last changed the status. `null` if the status has not changed ||
|| **total**
[`double`](../../data-types.md) | Document total. `0` if the total is not set ||
|| **commentary**
[`string`](../../data-types.md) or `null` | Comment. `null` if the comment is not set ||
|| **title**
[`string`](../../data-types.md) | Document title ||
|| **docNumber**
[`string`](../../data-types.md) or `null` | Incoming document number. `null` or an empty string if the number is not set ||
|| **contractorId**
[`integer`](../../data-types.md) or `null` | Vendor ID from the catalog directory. `null` if the vendor is not specified or vendors are managed in CRM ||
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
                "message": "This request requires data type \"bool\" in field \"status\".",
                "field": "status"
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
|| `status` | This request requires data type `bool` in field `status` | Pass the status as `true` or `false`. The same error with a different type is returned if the condition value does not fit the field, for example, the string `"abc"` for `id` ||
|| — | Field contractorId is unavailable while a contractor provider is active; use catalog.document.contractor.list | Vendors are managed in CRM, so the filter by `contractorId` is unavailable. Use the [catalog.document.contractor.list](../documentcontractor/catalog-documentcontractor-list-rest-v3.md) method ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `Unknown filter condition` | Pass `filter` as an array of conditions, for example `[["docType","=","A"]]` ||
|#

#### Filter Operator Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Unknown filter operator `like` | Use the operators from the [REST 3.0 overview](../../rest-v3.md#operators) ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass a positive number in `limit` ||
|#

#### Sorting Error

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `bad` | Pass the direction `asc` or `desc`. Case does not matter ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter`, `order` | Unknown field `foo` found in entity `DocumentDto` | Specify only the fields from the `select` parameter description. Write field names in camelCase: `DOC_TYPE` and `*` also cause this error ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `["title"]` | `select` contains a nested array. Pass `select` as an array of strings, for example `["id","title"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View and select warehouses" or "View product catalog" permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-add-rest-v3.md)
- [{#T}](./catalog-document-update-rest-v3.md)
- [{#T}](./catalog-document-get.md)
- [{#T}](./catalog-document-delete-rest-v3.md)
- [{#T}](./catalog-document-delete-list-rest-v3.md)
- [{#T}](./catalog-document-field-list.md)
- [{#T}](./catalog-document-field-get.md)
