# Create Inventory Document catalog.document.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View and select warehouses" permission and the "Create and edit" permission for the required document type

The method `catalog.document.add` creates an inventory document in draft status.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating the document ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **docType***
[`string`](../../data-types.md) | Document type. Possible values:
- `A` — stock receipt
- `S` — stock adjustment
- `M` — stock transfer between warehouses
- `R` — stock return
- `D` — write-off

The value is case-sensitive: the method rejects `a` and other values with the error `DOC_TYPE isn't available`.

Type names can be retrieved with the [catalog.enum.getStoreDocumentTypes](../enum/catalog-enum-get-store-document-types.md) method ||
|| **currency***
[`string`](../../data-types.md) | Document currency code, for example `RUB`. The method does not check that the currency exists. The currency cannot be changed after creation.

The list of currencies can be retrieved with the [crm.currency.list](../../crm/currency/crm-currency-list.md) method ||
|| **responsibleId***
[`user.id`](../../data-types.md) | Responsible user ID. The method does not check that the user exists. The method saves a non-numeric value as `null`.

User IDs can be retrieved with the [user.get](../../user/user-get.md) method ||
|| **title**
[`string`](../../data-types.md) | Document name. If not passed, `title` in the response is `null` ||
|| **docNumber**
[`string`](../../data-types.md) | Document number. If not passed, `docNumber` in the response is `null`: the number is not generated automatically ||
|| **dateDocument**
[`datetime`](../../data-types.md) | Document date, for example `2026-11-05T10:00:00+03:00`. You can pass a date without time, for example `2026-11-05`.

The method skips a string that cannot be parsed as a date without an error ||
|| **commentary**
[`string`](../../data-types.md) | Document comment ||
|| **total**
[`double`](../../data-types.md) | Document total. The method does not recalculate it from the products, including on processing. The method saves a non-numeric value as `null` ||
|| **siteId**
[`string`](../../data-types.md) | Site code. Default is `s1`. The method does not check that the site exists ||
|| **statusBy**
[`user.id`](../../data-types.md) | ID of the user who changed the document status. On processing and on canceling processing, the field gets the ID of the current user ||
|#

The method fills in the `id`, `status`, `createdBy`, `modifiedBy`, `dateCreate`, `dateModify`, and `dateStatus` fields itself: `status` is `N`, `createdBy` and `modifiedBy` are the ID of the current user, the dates are the creation time. The method skips passed values of these fields and unknown fields without an error.

Add products to the document with the [catalog.document.element.add](./document-element/catalog-document-element-add.md) method, and the vendor with the [catalog.documentcontractor.add](../documentcontractor/catalog-documentcontractor-add.md) method.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docType":"S","currency":"RUB","responsibleId":1269,"docNumber":"OP-00042","title":"Surplus stock adjustment","commentary":"Surplus found during stocktaking","dateDocument":"2026-11-05T10:00:00+03:00"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"docType":"S","currency":"RUB","responsibleId":1269,"docNumber":"OP-00042","title":"Surplus stock adjustment","commentary":"Surplus found during stocktaking","dateDocument":"2026-11-05T10:00:00+03:00"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.document.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentAddResult = {
      document: {
        commentary: string | null
        createdBy: number
        currency: string
        dateCreate: ISODate
        dateDocument: ISODate | null
        dateModify: ISODate
        dateStatus: ISODate
        docNumber: string | null
        docType: string
        id: number
        modifiedBy: number
        responsibleId: number | null
        siteId: string
        status: string
        statusBy: number | null
        title: string | null
        total: number | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<DocumentAddResult>({
        method: 'catalog.document.add',
        params: {
          fields: {
            docType: 'S',
            currency: 'USD',
            responsibleId: 1269,
            docNumber: 'OP-00042',
            title: 'Surplus stock adjustment',
            commentary: 'Surplus found during stocktaking',
            dateDocument: '2026-11-05T10:00:00+03:00',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created document:', result.document.id, result.document.status)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addDocument() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.document.add',
            params: {
              fields: {
                docType: 'S',
                currency: 'USD',
                responsibleId: 1269,
                docNumber: 'OP-00042',
                title: 'Surplus stock adjustment',
                commentary: 'Surplus found during stocktaking',
                dateDocument: '2026-11-05T10:00:00+03:00',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created document:', result.document.id, result.document.status)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addDocument)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.document.add(
            fields={
                "docType": "S",
                "currency": "USD",
                "responsibleId": 1269,
                "docNumber": "OP-00042",
                "title": "Surplus stock adjustment",
                "commentary": "Surplus found during stocktaking",
                "dateDocument": "2026-11-05T10:00:00+03:00",
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.document.add',
                [
                    'fields' => [
                        'docType'       => 'S',
                        'currency'      => 'RUB',
                        'responsibleId' => 1269,
                        'docNumber'     => 'OP-00042',
                        'title'         => 'Surplus stock adjustment',
                        'commentary'    => 'Surplus found during stocktaking',
                        'dateDocument'  => '2026-11-05T10:00:00+03:00',
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

    ```js
    BX24.callMethod(
        'catalog.document.add',
        {
            fields: {
                docType: 'S',
                currency: 'USD',
                responsibleId: 1269,
                docNumber: 'OP-00042',
                title: 'Surplus stock adjustment',
                commentary: 'Surplus found during stocktaking',
                dateDocument: '2026-11-05T10:00:00+03:00',
            },
        },
        function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.add',
        [
            'fields' => [
                'docType' => 'S',
                'currency' => 'USD',
                'responsibleId' => 1269,
                'docNumber' => 'OP-00042',
                'title' => 'Surplus stock adjustment',
                'commentary' => 'Surplus found during stocktaking',
                'dateDocument' => '2026-11-05T10:00:00+03:00',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.document.add", b24.Params{
    	"fields": b24.Params{
    		"docType":       "S",
    		"currency":      "EUR",
    		"responsibleId": 1269,
    		"docNumber":     "OP-00042",
    		"title":         "Surplus stock adjustment",
    		"commentary":    "Surplus found during stocktaking",
    		"dateDocument":  "2026-11-05T10:00:00+03:00",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.document.add: %w", err)
    }

    // The method wraps the response in an object with the "document" key.
    raw, ok := b24.Unwrap(res.Result, "document")
    if !ok {
    	return fmt.Errorf("no document key in the response")
    }

    // Optional fields can be null, so pointers are used for them.
    var item struct {
    	ID            b24.ID   `json:"id"`
    	DocType       string   `json:"docType"`
    	Status        string   `json:"status"`
    	Currency      string   `json:"currency"`
    	Title         *string  `json:"title"`
    	DocNumber     *string  `json:"docNumber"`
    	Commentary    *string  `json:"commentary"`
    	ResponsibleID *int     `json:"responsibleId"`
    	DateDocument  *string  `json:"dateDocument"`
    	DateCreate    string   `json:"dateCreate"`
    	Total         *float64 `json:"total"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Status)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "document": {
            "commentary": "Surplus found during stocktaking",
            "createdBy": 1269,
            "currency": "USD",
            "dateCreate": "2026-11-05T15:58:42+03:00",
            "dateDocument": "2026-11-05T10:00:00+03:00",
            "dateModify": "2026-11-05T15:58:42+03:00",
            "dateStatus": "2026-11-05T15:58:42+03:00",
            "docNumber": "OP-00042",
            "docType": "S",
            "id": 381,
            "modifiedBy": 1269,
            "responsibleId": 1269,
            "siteId": "s1",
            "status": "N",
            "statusBy": null,
            "title": "Surplus stock adjustment",
            "total": null
        }
    },
    "time": {
        "start": 1793883522,
        "finish": 1793883522.271001,
        "duration": 0.27100110054016113,
        "processing": 0,
        "date_start": "2026-11-05T15:58:42+03:00",
        "date_finish": "2026-11-05T15:58:42+03:00",
        "operating_reset_at": 1793884080,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.document**
[`catalog_document`](../data-types.md#catalog_document) | Object with the fields of the created document. The method returns all document fields, including empty ones with the value `null`.

The document is created in status `N` — draft. You can process the document with the [catalog.document.conduct](./catalog-document-conduct.md) method ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: docType, responsibleId, currency"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Cannot complete action because you don't have sufficient permissions to view and select warehouses. | The user does not have the "View and select warehouses" permission ||
|| `400` | `0` | Insufficient permissions to save the inventory object. | The user does not have the "Create and edit" permission for documents of this type or has no catalog permissions ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not passed ||
|| `400` | `100` | Invalid value {abc} to match with parameter {fields}. Should be value of type array. | `fields` is not an object ||
|| `400` | `0` | Required fields: docType, responsibleId, currency | Required fields are not passed. The text lists only the missing fields ||
|| `400` | `0` | DOC_TYPE isn't available | `docType` contains a type that is not in the list, for example `U` or `a` ||
|| `400` | `0` | Wrong format of field `title`. | The field contains an array or an object instead of a value. The text contains the field name ||
|| `400` | `0` | Inventory Management is not available on your plan. | Inventory Management is not available on the account plan ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-update.md)
- [{#T}](./catalog-document-list.md)
- [{#T}](./catalog-document-conduct.md)
- [{#T}](./catalog-document-get-fields.md)
- [{#T}](./document-element/catalog-document-element-add.md)
- [{#T}](../documentcontractor/catalog-documentcontractor-add.md)