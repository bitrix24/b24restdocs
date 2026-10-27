# Update VAT Rate v 3.0 catalog.vat.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Manage tax rates" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.vat.update` method updates a VAT rate by its identifier and returns its fields after the update.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | VAT rate identifier.

The identifier can be retrieved using the [catalog.vat.list](./catalog-vat-list-rest-v3.md) method ||
|| **fields***
[`object`](../../data-types.md) | An object with the VAT rate fields to update. Pass only the fields you are changing. [Field description](#fields)

An empty object `{}` causes a validation error ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | VAT rate name. An empty string and `null` cause a validation error ||
|| **rate**
[`double`](../../data-types.md) | VAT rate value in percent, for example `10` or `7.5`.

Values from `0` and above are allowed. A negative value and `null` cause a validation error.

The value of the system "No VAT" rate does not change and stays `null` ||
|| **active**
[`boolean`](../../data-types.md) | VAT rate activity.

Possible values:

- `true` — the rate is active
- `false` — the rate is inactive

The strings `Y` and `N` are not accepted, `null` causes a validation error ||
|| **sort**
[`integer`](../../data-types.md) | Sort order. If you pass zero or a negative value, the method keeps the previous sort order and does not return an error. `null` causes a validation error ||
|#

The `id` and `timestampX` fields cannot be changed. If you pass them in `fields`, the method returns an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.vat.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"fields":{"name":"VAT 10%","rate":10,"active":false}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.vat.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"fields":{"name":"VAT 10%","rate":10,"active":false},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.vat.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type VatUpdateResult = {
      item: {
        id: number
        timestampX: ISODate
        active: boolean
        sort: number
        name: string
        rate: number | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<VatUpdateResult>({
        method: 'catalog.vat.update',
        params: {
          id: 27,
          fields: {
            name: 'VAT 10%',
            rate: 10,
            active: false,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('VAT rate updated:', result.item)
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
      async function updateVat() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.vat.update',
            params: {
              id: 27,
              fields: {
                name: 'VAT 10%',
                rate: 10,
                active: false,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('VAT rate updated:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateVat)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.vat.update(
            bitrix_id=27,
            fields={
                "name": "VAT 10%",
                "rate": 10,
                "active": False,
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
                'catalog.vat.update',
                [
                    'id' => 27,
                    'fields' => [
                        'name' => 'VAT 10%',
                        'rate' => 10,
                        'active' => false,
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
        'catalog.vat.update',
        {
            id: 27,
            fields: {
                name: 'VAT 10%',
                rate: 10,
                active: false,
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
        'catalog.vat.update',
        [
            'id' => 27,
            'fields' => [
                'name' => 'VAT 10%',
                'rate' => 10,
                'active' => false,
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
    res, err := client.Core().Call(ctx, "catalog.vat.update", b24.Params{
    	"id": 27,
    	"fields": b24.Params{
    		"name":   "VAT 10%",
    		"rate":   10,
    		"active": false,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.vat.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID     int      `json:"id"`
    	Name   string   `json:"name"`
    	Rate   *float64 `json:"rate"`
    	Active bool     `json:"active"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name, item.Active)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 27,
            "timestampX": "2026-10-22T13:39:38+02:00",
            "active": false,
            "sort": 10,
            "name": "VAT 10%",
            "rate": 10
        }
    },
    "time": {
        "start": 1792665578,
        "finish": 1792665578.735093,
        "duration": 0.7350931167602539,
        "processing": 0,
        "date_start": "2026-10-22T13:39:38+02:00",
        "date_finish": "2026-10-22T13:39:38+02:00",
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
[`object`](../../data-types.md) | An object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | All VAT rate fields after the update. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | VAT rate identifier ||
|| **timestampX**
[`datetime`](../../data-types.md) | Date and time of the last rate change ||
|| **active**
[`boolean`](../../data-types.md) | Rate activity: `true` — active, `false` — inactive ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **name**
[`string`](../../data-types.md) | VAT rate name ||
|| **rate**
[`double`](../../data-types.md) or `null` | VAT rate value in percent. `null` for the system "No VAT" rate ||
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
|| `-` | At least one of the required fields must be not empty: id, filter | Pass `id` — a positive integer ||
|| `id` | Required field `id` is missing | Pass `id` as a number, not a string ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `fields` | At least one field to update must be provided | Pass at least one field in `fields` ||
|| `rate` | This request requires data type `float` in field `rate` | Pass a number ||
|| `active` | This request requires data type `bool` in field `active` | Pass `true` or `false`, not `Y` or `N` ||
|| `active` | The field active must be true or false | Do not pass `null` in `active` ||
|| `sort` | This request requires data type `int` in field `sort` | Pass an integer ||
|| `sort` | The field sort must be an integer | Do not pass `null` in `sort` ||
|| `-` | Tax rate value is incorrect`<br>` | Pass a number from `0` and above in `rate`. The `null` value is not supported ||
|| `-` | No tax rate name`<br>` | Pass a non-empty string in `name` ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Field `id` is read-only | Remove `id` from `fields` ||
|| `timestampX` | Field `timestampX` is read-only | Remove `timestampX` from `fields` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | Check the identifier using the [catalog.vat.list](./catalog-vat-list-rest-v3.md) method ||
|#

#### Filter Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `filter` | Pass `id` instead of `filter`: the method updates one rate ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `VatDto` | Pass only `name`, `rate`, `active`, `sort` in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "Manage tax rates" permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-vat-add-rest-v3.md)
- [{#T}](./catalog-vat-get-rest-v3.md)
- [{#T}](./catalog-vat-list-rest-v3.md)
- [{#T}](./catalog-vat-delete-rest-v3.md)
- [{#T}](./catalog-vat-field-list.md)
