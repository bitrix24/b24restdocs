# Update Unit of Measurement v 3.0 catalog.measure.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the catalog or view warehouses and to modify measurement units

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.measure.update` method updates a unit of measurement and returns its data after the update.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Unit of measurement identifier.

The identifier can be retrieved using the [catalog.measure.list](./catalog-measure-list-rest-v3.md) method or from the [catalog.measure.add](./catalog-measure-add-rest-v3.md) response ||
|| **fields***
[`object`](../../data-types.md) | Object with the unit of measurement fields to update. Pass only the fields being changed. [Field description](#fields)

An empty object `{}` returns the unit data without changes ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`integer`](../../data-types.md) | Unit of measurement code. The value must be unique: if another unit already uses the code, the method returns an error ||
|| **measureTitle**
[`string`](../../data-types.md) | Name of the unit of measurement ||
|| **symbol**
[`string`](../../data-types.md) | Symbol ||
|| **symbolIntl**
[`string`](../../data-types.md) | International symbol ||
|| **symbolLetterIntl**
[`string`](../../data-types.md) | International letter code symbol ||
|| **isDefault**
[`boolean`](../../data-types.md) | Default unit of measurement flag. The default unit is selected automatically in a new product form.

Possible values:

- `true` — default unit
- `false` — regular unit

There can be only one default unit, and the method does not clear the flag on the current unit automatically. To make another unit the default one, make two calls: pass `false` for the current unit, then `true` for the new one. You can find the current default unit using the [catalog.measure.list](./catalog-measure-list-rest-v3.md) method with the `[["isDefault","=",true]]` filter ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.measure.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":21,"fields":{"measureTitle":"Package of 10 pcs.","symbol":"pk"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.measure.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":21,"fields":{"measureTitle":"Package of 10 pcs.","symbol":"pk"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.measure.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type MeasureUpdateResult = {
      item: {
        id: number
        code: number
        measureTitle: string
        symbol: string
        symbolIntl: string
        symbolLetterIntl: string
        isDefault: boolean
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<MeasureUpdateResult>({
        method: 'catalog.measure.update',
        params: {
          id: 21,
          fields: {
            measureTitle: 'Package of 10 pcs.',
            symbol: 'pk',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Measure updated:', result.item)
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
      async function updateMeasure() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.measure.update',
            params: {
              id: 21,
              fields: {
                measureTitle: 'Package of 10 pcs.',
                symbol: 'pk',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Measure updated:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateMeasure)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.measure.update(
            bitrix_id=21,
            fields={
                "measureTitle": "Package of 10 pcs.",
                "symbol": "pk",
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
                'catalog.measure.update',
                [
                    'id' => 21,
                    'fields' => [
                        'measureTitle' => 'Package of 10 pcs.',
                        'symbol' => 'pk',
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
        'catalog.measure.update',
        {
            id: 21,
            fields: {
                measureTitle: 'Package of 10 pcs.',
                symbol: 'pk',
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
        'catalog.measure.update',
        [
            'id' => 21,
            'fields' => [
                'measureTitle' => 'Package of 10 pcs.',
                'symbol' => 'pk',
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
    res, err := client.Core().Call(ctx, "catalog.measure.update", b24.Params{
    	"id": 21,
    	"fields": b24.Params{
    		"measureTitle": "Package of 10 pcs.",
    		"symbol":       "pk",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.measure.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID           int    `json:"id"`
    	MeasureTitle string `json:"measureTitle"`
    	Symbol       string `json:"symbol"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.MeasureTitle, item.Symbol)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 21,
            "code": 778,
            "measureTitle": "Package of 10 pcs.",
            "symbol": "pk",
            "symbolIntl": "pack",
            "symbolLetterIntl": "NMP",
            "isDefault": false
        }
    },
    "time": {
        "start": 1792665567,
        "finish": 1792665567.517781,
        "duration": 0.5177810192108154,
        "processing": 0,
        "date_start": "2026-10-22T13:39:27+02:00",
        "date_finish": "2026-10-22T13:39:27+02:00",
        "operating_reset_at": 1792666140,
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
[`object`](../../data-types.md) | The unit of measurement after the update. [Field description](#item) ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "default value can be set once [isDefault]"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the `id` of the unit of measurement ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object with the fields to update ||
|| `code` | This request requires data type `int` in field `code` | Pass the code as a number ||
|| `isDefault` | This request requires data type `bool` in field `isDefault` | Pass `true` or `false`, not `Y` or `N` ||
|| `-` | Duplicate entry for key [code] | Another unit already uses this code. Specify a different `code` ||
|| `-` | default value can be set once [isDefault] | First pass `false` in `isDefault` for the current default unit ||
|#

#### Field Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Field `id` is read-only | Remove `id` from `fields` and pass it at the top level of the request ||
|#

#### Filter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `filter` | The method does not accept `filter`. Pass the `id` of the unit of measurement ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Unknown field `#FIELD#` found in entity `MeasureDto` | Pass only the fields from the [fields parameter table](#fields) in `fields` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | Check the identifier using the [catalog.measure.list](./catalog-measure-list-rest-v3.md) method ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have permission to read the catalog or view warehouses, or permission to modify measurement units ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-measure-add-rest-v3.md)
- [{#T}](./catalog-measure-get-rest-v3.md)
- [{#T}](./catalog-measure-list-rest-v3.md)
- [{#T}](./catalog-measure-delete-rest-v3.md)
- [{#T}](./catalog-measure-field-list.md)
