# Update Inventory Document v 3.0 catalog.document.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Create and edit" permission for the required document type and one of the permissions: "View and select warehouses" or "View product catalog"

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.document.update` method updates the fields of an inventory document and returns the document after the update.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Document identifier.

The identifier can be retrieved using the [catalog.document.list](./catalog-document-list-rest-v3.md) method or from the [catalog.document.add](./catalog-document-add-rest-v3.md) response ||
|| **fields***
[`object`](../../data-types.md) | An object with the inventory document fields to update. Pass only the fields you change. [Field description](#fields)

An empty object `{}` returns the document unchanged ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **responsibleId**
[`integer`](../../data-types.md) | Identifier of the responsible person.

The user identifier can be retrieved using the [user.get](../../user/user-get.md) method ||
|| **currency**
[`string`](../../data-types.md) | Document currency code, for example `EUR` ||
|| **title**
[`string`](../../data-types.md) | Document title. If not passed, Bitrix24 generates the title from the type and number, for example "Stock receipt #37" ||
|| **docNumber**
[`string`](../../data-types.md) | Incoming document number ||
|| **dateDocument**
[`datetime`](../../data-types.md) | Document date in ISO 8601 format with time, for example `2026-10-22T10:00:00+02:00`. A date without time causes a validation error ||
|| **total**
[`double`](../../data-types.md) | Document total ||
|| **commentary**
[`string`](../../data-types.md) | Comment ||
|| **statusBy**
[`integer`](../../data-types.md) | Identifier of the user who last changed the status ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the document. The value is saved only if the method is called by an administrator. Otherwise, the current user is set ||
|| **contractorId**
[`integer`](../../data-types.md) | Identifier of the vendor from the catalog directory. If vendors are managed in CRM in Bitrix24, the field is not saved. To bind a CRM vendor to the document, use the [catalog.document.contractor.add](../documentcontractor/catalog-documentcontractor-add-rest-v3.md) method ||
|| **siteId**
[`string`](../../data-types.md) | Site identifier ||
|#

The `docType` document type cannot be changed: the method does not save the value from `fields` and does not return an error. The `id`, `dateCreate`, `dateModify`, `createdBy`, `status`, and `dateStatus` fields are filled in automatically. If you pass them in `fields`, the method returns an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.document.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":343,"fields":{"title":"Receipt from vendor Alpha GmbH","dateDocument":"2026-10-22T10:00:00+02:00","total":15000}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":343,"fields":{"title":"Receipt from vendor Alpha GmbH","dateDocument":"2026-10-22T10:00:00+02:00","total":15000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.document.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentUpdateResult = {
      item: {
        id: number
        docType: string
        siteId: string
        responsibleId: number
        dateModify: ISODate
        dateCreate: ISODate
        createdBy: number
        modifiedBy: number
        currency: string
        status: string
        dateStatus: ISODate
        dateDocument: ISODate | null
        statusBy: number
        total: number
        commentary: string
        title: string
        docNumber: string
        contractorId: number
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentUpdateResult>({
        method: 'catalog.document.update',
        params: {
          id: 343,
          fields: {
            title: 'Receipt from vendor Alpha GmbH',
            dateDocument: '2026-10-22T10:00:00+02:00',
            total: 15000,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Document updated:', result.item.id, result.item.title)
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
      async function callDocumentUpdate() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.document.update',
            params: {
              id: 343,
              fields: {
                title: 'Receipt from vendor Alpha GmbH',
                dateDocument: '2026-10-22T10:00:00+02:00',
                total: 15000,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Document updated:', result.item.id, result.item.title)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', callDocumentUpdate)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.update(
            id=343,
            fields={
                "title": "Receipt from vendor Alpha GmbH",
                "dateDocument": "2026-10-22T10:00:00+02:00",
                "total": 15000,
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
                'catalog.document.update',
                [
                    'id' => 343,
                    'fields' => [
                        'title' => 'Receipt from vendor Alpha GmbH',
                        'dateDocument' => '2026-10-22T10:00:00+02:00',
                        'total' => 15000,
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
        'catalog.document.update',
        {
            id: 343,
            fields: {
                title: 'Receipt from vendor Alpha GmbH',
                dateDocument: '2026-10-22T10:00:00+02:00',
                total: 15000,
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
        'catalog.document.update',
        [
            'id' => 343,
            'fields' => [
                'title' => 'Receipt from vendor Alpha GmbH',
                'dateDocument' => '2026-10-22T10:00:00+02:00',
                'total' => 15000,
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
    res, err := client.Core().Call(ctx, "catalog.document.update", b24.Params{
    	"id":     343,
    	"fields": b24.Params{
    		"title":        "Receipt from vendor Alpha GmbH",
    		"dateDocument": "2026-10-22T10:00:00+02:00",
    		"total":        15000,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.update: %w", err)
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
            "siteId": "s1",
            "responsibleId": 1,
            "dateModify": "2026-10-22T13:34:08+02:00",
            "dateCreate": "2026-10-22T13:34:07+02:00",
            "createdBy": 1,
            "modifiedBy": 1,
            "currency": "EUR",
            "status": "",
            "dateStatus": "2026-10-22T13:34:07+02:00",
            "dateDocument": "2026-10-22T10:00:00+02:00",
            "statusBy": 0,
            "total": 15000,
            "commentary": "Delivery under invoice 15",
            "title": "Receipt from vendor Alpha GmbH",
            "docNumber": "IN-0015",
            "contractorId": 0
        }
    },
    "time": {
        "start": 1792665248,
        "finish": 1792665248.332424,
        "duration": 0.3324239253997803,
        "processing": 0,
        "date_start": "2026-10-22T13:34:08+02:00",
        "date_finish": "2026-10-22T13:34:08+02:00",
        "operating_reset_at": 1792665840,
        "operating": 0.10437798500061035
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
[`object`](../../data-types.md) | Document fields after the update. [Field description](#item) ||
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
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the document `id`. A second message comes with this one: "Only one of the following values must be provided: id, filter" ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `dateDocument` | This request requires data type `DateTime` in field `dateDocument` | Pass a date with time in ISO 8601 format, for example `2026-10-22T10:00:00+02:00` ||
|| `-` | Inventory Management is not available on your plan | The method works only on plans with inventory management ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | Field `#FIELD#` is read-only | Remove the `id`, `dateCreate`, `dateModify`, `createdBy`, `status`, and `dateStatus` fields from `fields` ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `filter` | The method does not support `filter`. Pass the document `id` ||
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
|| `-` | Unknown field `#FIELD#` found in entity `DocumentDto` | Pass only the fields from the [Parameter fields](#fields) table in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user permissions for this document type ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-add-rest-v3.md)
- [{#T}](./catalog-document-get.md)
- [{#T}](./catalog-document-list-rest-v3.md)
- [{#T}](./catalog-document-delete-rest-v3.md)
- [{#T}](./catalog-document-delete-list-rest-v3.md)
- [{#T}](./catalog-document-field-list.md)
- [{#T}](./catalog-document-field-get.md)
