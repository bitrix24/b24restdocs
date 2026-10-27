# Add VAT Rate v 3.0 catalog.vat.add

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

The `catalog.vat.add` method creates a VAT rate and returns its fields.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object with the fields of the new VAT rate. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | VAT rate name. An empty string causes a validation error ||
|| **rate***
[`double`](../../data-types.md) | VAT rate value in percent, for example `13` or `7.5`.

Values from `0` and above are allowed. A negative value and `null` cause a validation error ||
|| **active**
[`boolean`](../../data-types.md) | VAT rate activity.

Possible values:

- `true` — the rate is active
- `false` — the rate is inactive

The strings `Y` and `N` are not accepted, `null` causes a validation error.

Default: `true` ||
|| **sort**
[`integer`](../../data-types.md) | Sort order. Zero and negative values are replaced with `100`, `null` causes a validation error.

Default: `100` ||
|#

The `id` and `timestampX` fields are filled in automatically. If you pass them in `fields`, the method returns an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.vat.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"VAT 13%","rate":13,"sort":10,"active":true}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.vat.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"VAT 13%","rate":13,"sort":10,"active":true},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.vat.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type VatAddResult = {
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
      const response = await $b24.actions.v3.call.make<VatAddResult>({
        method: 'catalog.vat.add',
        params: {
          fields: {
            name: 'VAT 13%',
            rate: 13,
            sort: 10,
            active: true,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('VAT rate created:', result.item.id)
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
      async function addVat() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.vat.add',
            params: {
              fields: {
                name: 'VAT 13%',
                rate: 13,
                sort: 10,
                active: true,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('VAT rate created:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addVat)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.vat.add(
            fields={
                "name": "VAT 13%",
                "rate": 13,
                "sort": 10,
                "active": True,
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
                'catalog.vat.add',
                [
                    'fields' => [
                        'name' => 'VAT 13%',
                        'rate' => 13,
                        'sort' => 10,
                        'active' => true,
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
        'catalog.vat.add',
        {
            fields: {
                name: 'VAT 13%',
                rate: 13,
                sort: 10,
                active: true,
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
        'catalog.vat.add',
        [
            'fields' => [
                'name' => 'VAT 13%',
                'rate' => 13,
                'sort' => 10,
                'active' => true,
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
    res, err := client.Core().Call(ctx, "catalog.vat.add", b24.Params{
    	"fields": b24.Params{
    		"name":   "VAT 13%",
    		"rate":   13,
    		"sort":   10,
    		"active": true,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.vat.add: %w", err)
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
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 27,
            "timestampX": "2026-10-22T13:39:08+02:00",
            "active": true,
            "sort": 10,
            "name": "VAT 13%",
            "rate": 13
        }
    },
    "time": {
        "start": 1792665548,
        "finish": 1792665548.462168,
        "duration": 0.46216797828674316,
        "processing": 0,
        "date_start": "2026-10-22T13:39:08+02:00",
        "date_finish": "2026-10-22T13:39:08+02:00",
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
[`object`](../../data-types.md) | Fields of the created VAT rate. [Field description](#item) ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Required field \"rate\" is empty.",
                "field": "rate"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is empty | Pass `name` in `fields` ||
|| `rate` | Required field `rate` is empty | Pass `rate` in `fields` ||
|| `id` | Field `id` is read-only | Remove `id` from `fields` ||
|| `timestampX` | Field `timestampX` is read-only | Remove `timestampX` from `fields` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `rate` | This request requires data type `float` in field `rate` | Pass a number ||
|| `active` | This request requires data type `bool` in field `active` | Pass `true` or `false`, not `Y` or `N` ||
|| `active` | The field active must be true or false | Do not pass `null` in `active` ||
|| `sort` | This request requires data type `int` in field `sort` | Pass an integer ||
|| `sort` | The field sort must be an integer | Do not pass `null` in `sort` ||
|| `-` | Tax rate value is incorrect`<br>` | Pass a number from `0` and above in `rate`. The `null` value is not supported ||
|| `-` | No tax rate name`<br>` | Pass a non-empty string in `name` ||
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

- [{#T}](./catalog-vat-update-rest-v3.md)
- [{#T}](./catalog-vat-get-rest-v3.md)
- [{#T}](./catalog-vat-list-rest-v3.md)
- [{#T}](./catalog-vat-delete-rest-v3.md)
- [{#T}](./catalog-vat-field-list.md)
