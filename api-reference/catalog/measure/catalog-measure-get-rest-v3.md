# Get Unit of Measurement Information by ID v 3.0 catalog.measure.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the catalog or view warehouses

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.measure.get` method returns the data of a unit of measurement by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Unit of measurement identifier.

The identifier can be retrieved using the [catalog.measure.list](./catalog-measure-list-rest-v3.md) method ||
|| **select**
[`array`](../../data-types.md) | Array of unit of measurement fields to return in the response.

Available fields:

- `id` — unit of measurement identifier
- `code` — unit of measurement code
- `measureTitle` — name
- `symbol` — symbol
- `symbolIntl` — international symbol
- `symbolLetterIntl` — international letter code symbol
- `isDefault` — default unit flag

Default: all fields. An empty array `[]` also returns all fields; the value `["*"]` is not supported ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.measure.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9,"select":["id","code","measureTitle","symbol","isDefault"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.measure.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":9,"select":["id","code","measureTitle","symbol","isDefault"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.measure.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type MeasureGetResult = {
      item: {
        id: number
        code: number
        measureTitle: string
        symbol: string
        isDefault: boolean
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<MeasureGetResult>({
        method: 'catalog.measure.get',
        params: {
          id: 9,
          select: [
            'id',
            'code',
            'measureTitle',
            'symbol',
            'isDefault',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Measure:', result.item.code, result.item.isDefault)
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
      async function getMeasure() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.measure.get',
            params: {
              id: 9,
              select: [
                'id',
                'code',
                'measureTitle',
                'symbol',
                'isDefault',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Measure:', result.item.code, result.item.isDefault)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMeasure)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.measure.get(
            bitrix_id=9,
            select=["id", "code", "measureTitle", "symbol", "isDefault"],
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
                'catalog.measure.get',
                [
                    'id' => 9,
                    'select' => ['id', 'code', 'measureTitle', 'symbol', 'isDefault'],
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
        'catalog.measure.get',
        {
            id: 9,
            select: ['id', 'code', 'measureTitle', 'symbol', 'isDefault'],
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
        'catalog.measure.get',
        [
            'id' => 9,
            'select' => ['id', 'code', 'measureTitle', 'symbol', 'isDefault'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.measure.get", b24.Params{
    	"id":     9,
    	"select": []string{"id", "code", "measureTitle", "symbol", "isDefault"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.measure.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID           int    `json:"id"`
    	Code         int    `json:"code"`
    	MeasureTitle string `json:"measureTitle"`
    	Symbol       string `json:"symbol"`
    	IsDefault    bool   `json:"isDefault"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Code, item.IsDefault)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 9,
            "code": 796,
            "measureTitle": "",
            "symbol": "",
            "isDefault": true
        }
    },
    "time": {
        "start": 1792665512,
        "finish": 1792665513.004024,
        "duration": 1.0040240287780762,
        "processing": 1,
        "date_start": "2026-10-22T13:38:32+02:00",
        "date_finish": "2026-10-22T13:38:33+02:00",
        "operating_reset_at": 1792666080,
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
[`object`](../../data-types.md) | Unit of measurement data. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Unit of measurement identifier ||
|| **code**
[`integer`](../../data-types.md) | Unit of measurement code ||
|| **measureTitle**
[`string`](../../data-types.md) | Name of the unit of measurement. An empty string if the value is not set ||
|| **symbol**
[`string`](../../data-types.md) | Symbol. An empty string if the value is not set ||
|| **symbolIntl**
[`string`](../../data-types.md) | International symbol. An empty string if the value is not set ||
|| **symbolLetterIntl**
[`string`](../../data-types.md) | International letter code symbol. An empty string if the value is not set ||
|| **isDefault**
[`boolean`](../../data-types.md) | `true` — default unit of measurement, `false` — regular unit ||
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

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass `id` as a positive integer. The same error is returned for `0`, a negative value, and a non-numeric value ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | Check the identifier using the [catalog.measure.list](./catalog-measure-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `MeasureDto` | Specify only the fields from the [item object description](#item) in `select` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have permission to read the catalog or view warehouses ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-measure-add-rest-v3.md)
- [{#T}](./catalog-measure-update-rest-v3.md)
- [{#T}](./catalog-measure-list-rest-v3.md)
- [{#T}](./catalog-measure-delete-rest-v3.md)
- [{#T}](./catalog-measure-field-list.md)
