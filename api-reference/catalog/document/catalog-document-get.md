# Get Inventory Document by ID catalog.document.get

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

The `catalog.document.get` method returns the fields of an inventory document by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Document identifier.

The identifier can be retrieved using the [catalog.document.list](./catalog-document-list-rest-v3.md) method or from the [catalog.document.add](./catalog-document-add-rest-v3.md) response.

The method returns the document only if the user has the "View" permission for its type. Otherwise, the method returns an access error ||
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

The `id` field is always returned, even if it is not in `select`. The `["*"]` value is not supported.

Default: all fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":343,"select":["id","docType","title","status","total","currency","dateDocument"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":343,"select":["id","docType","title","status","total","currency","dateDocument"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentGetResult = {
      item: {
        id: number
        docType: string
        currency: string
        status: string
        dateDocument: ISODate | null
        total: number
        title: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentGetResult>({
        method: 'catalog.document.get',
        params: {
          id: 343,
          select: ['id', 'docType', 'title', 'status', 'total', 'currency', 'dateDocument'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Document:', result.item.id, result.item.title, result.item.status)
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
      async function callDocumentGet() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.get',
            params: {
              id: 343,
              select: ['id', 'docType', 'title', 'status', 'total', 'currency', 'dateDocument'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Document:', result.item.id, result.item.title, result.item.status)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', callDocumentGet)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.get(
            id=343,
            select=["id", "docType", "title", "status", "total", "currency", "dateDocument"],
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
                'catalog.document.get',
                [
                    'id' => 343,
                    'select' => ['id', 'docType', 'title', 'status', 'total', 'currency', 'dateDocument'],
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
        'catalog.document.get',
        {
            id: 343,
            select: ['id', 'docType', 'title', 'status', 'total', 'currency', 'dateDocument'],
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
        'catalog.document.get',
        [
            'id' => 343,
            'select' => ['id', 'docType', 'title', 'status', 'total', 'currency', 'dateDocument'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.get", b24.Params{
    	"id":     343,
    	"select": []string{"id", "docType", "title", "status", "total", "currency", "dateDocument"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.document.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID       int     `json:"id"`
    	DocType  string  `json:"docType"`
    	Title    string  `json:"title"`
    	Status   string  `json:"status"`
    	Total    float64 `json:"total"`
    	Currency string  `json:"currency"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Title, item.Status)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 343,
            "docType": "A",
            "currency": "EUR",
            "status": "",
            "dateDocument": "2026-10-22T10:00:00+02:00",
            "total": 15000,
            "title": "Receipt from vendor Alpha GmbH"
        }
    },
    "time": {
        "start": 1792665249,
        "finish": 1792665249.695711,
        "duration": 0.6957108974456787,
        "processing": 0,
        "date_start": "2026-10-22T13:34:09+02:00",
        "date_finish": "2026-10-22T13:34:09+02:00",
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
|| **result.item**
[`object`](../../data-types.md) | Document fields. The set depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

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
[`string`](../../data-types.md) | Processing status: `"1"` — the document is processed, an empty string — not processed ||
|| **dateStatus**
[`datetime`](../../data-types.md) | Date and time of the last status change ||
|| **dateDocument**
[`datetime`](../../data-types.md) or `null` | Document date. `null` if the date is not set ||
|| **statusBy**
[`integer`](../../data-types.md) | Identifier of the user who last changed the status. `0` if the status has not changed ||
|| **total**
[`double`](../../data-types.md) | Document total ||
|| **commentary**
[`string`](../../data-types.md) | Comment ||
|| **title**
[`string`](../../data-types.md) | Document title ||
|| **docNumber**
[`string`](../../data-types.md) | Incoming document number ||
|| **contractorId**
[`integer`](../../data-types.md) | Identifier of the vendor from the catalog directory. `0` if no vendor is specified ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 999999 was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass a positive integer in `id` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | Check the identifier using the [catalog.document.list](./catalog-document-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DocumentDto` | Specify only the fields from the `select` parameter description in `select` ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["id","title"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View" permission for the document type ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-add-rest-v3.md)
- [{#T}](./catalog-document-update-rest-v3.md)
- [{#T}](./catalog-document-list-rest-v3.md)
- [{#T}](./catalog-document-delete-rest-v3.md)
- [{#T}](./catalog-document-delete-list-rest-v3.md)
- [{#T}](./catalog-document-field-list.md)
- [{#T}](./catalog-document-field-get.md)
