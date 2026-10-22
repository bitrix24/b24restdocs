# Update Price Type v 3.0 catalog.pricetype.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Manage price types" access permission

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.pricetype.update` method changes the fields of a price type and returns its fields after the change.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Price type identifier.

The identifier can be obtained using the [catalog.pricetype.list](./catalog-price-type-list-rest-v3.md) method or when creating a price type with the [catalog.pricetype.add](./catalog-price-type-add-rest-v3.md) method ||
|| **fields***
[`object`](../../data-types.md) | Object with the price type fields to change. Pass only the fields being changed. [Field description](#fields)

An empty `{}` object causes a validation error ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Price type name. An empty string and `null` cause a validation error ||
|| **base**
[`boolean`](../../data-types.md) | Indicates whether the price type is base.

Possible values:

- `true` — the price type becomes the base one. The flag is automatically removed from the previous base type
- `false` — remove the base type flag

The strings `Y` and `N` are not accepted, and `null` causes a validation error ||
|| **sort**
[`integer`](../../data-types.md) | Sort order. Zero and negative values are replaced with `100`, and `null` causes a validation error ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the price type ||
|#

The `id`, `timestampX`, `modifiedBy`, `dateCreate`, and `createdBy` fields cannot be changed. If you pass them in `fields`, the method returns an error.

{% note warning "" %}

If you pass `base: false` for the base price type, the flag is removed, and no other type becomes the base one. Bitrix24 is left without a base price type. To change the base type, pass `base: true` for the new type.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

A call to the new API differs by the `/api/` parameter added to the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.pricetype.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":31,"fields":{"name":"Partner wholesale price","sort":150}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.pricetype.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":31,"fields":{"name":"Partner wholesale price","sort":150},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.pricetype.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PriceTypeUpdateResult = {
      item: {
        id: number
        name: string
        base: boolean
        sort: number
        xmlId: string
        timestampX: ISODate
        modifiedBy: number
        dateCreate: ISODate
        createdBy: number
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<PriceTypeUpdateResult>({
        method: 'catalog.pricetype.update',
        params: {
          id: 31,
          fields: {
            name: 'Partner wholesale price',
            sort: 150,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Price type updated:', result.item.id, result.item.name)
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
      async function updatePriceType() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.pricetype.update',
            params: {
              id: 31,
              fields: {
                name: 'Partner wholesale price',
                sort: 150,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Price type updated:', result.item.id, result.item.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updatePriceType)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.pricetype.update(
            bitrix_id=31,
            fields={
                "name": "Partner wholesale price",
                "sort": 150,
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
                'catalog.pricetype.update',
                [
                    'id' => 31,
                    'fields' => [
                        'name' => 'Partner wholesale price',
                        'sort' => 150,
                    ],
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
        'catalog.pricetype.update',
        {
            id: 31,
            fields: {
                name: 'Partner wholesale price',
                sort: 150,
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
        'catalog.pricetype.update',
        [
            'id' => 31,
            'fields' => [
                'name' => 'Partner wholesale price',
                'sort' => 150,
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
    res, err := client.Core().Call(ctx, "catalog.pricetype.update", b24.Params{
    	"id": 31,
    	"fields": b24.Params{
    		"name": "Partner wholesale price",
    		"sort": 150,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.pricetype.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID   int    `json:"id"`
    	Name string `json:"name"`
    	Base bool   `json:"base"`
    	Sort int    `json:"sort"`
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
            "id": 31,
            "name": "Partner wholesale price",
            "base": false,
            "sort": 150,
            "xmlId": "wholesale-opt",
            "timestampX": "2026-10-22T14:23:13+02:00",
            "modifiedBy": 1,
            "dateCreate": "2026-10-22T14:23:07+02:00",
            "createdBy": 1
        }
    },
    "time": {
        "start": 1792668193,
        "finish": 1792668193.714955,
        "duration": 0.7149550914764404,
        "processing": 0,
        "date_start": "2026-10-22T14:23:13+02:00",
        "date_finish": "2026-10-22T14:23:13+02:00",
        "operating_reset_at": 1792668780,
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
[`object`](../../data-types.md) | Price type fields after the change. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Price type identifier ||
|| **name**
[`string`](../../data-types.md) | Price type name ||
|| **base**
[`boolean`](../../data-types.md) | Indicates whether the price type is base: `true` — base, `false` — regular ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the price type. An empty string if the code is not set ||
|| **timestampX**
[`datetime`](../../data-types.md) | Date and time of the last change ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who last changed the price type ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date and time of creation ||
|| **createdBy**
[`integer`](../../data-types.md) | Identifier of the user who created the price type ||
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
|| `-` | At least one of the required fields must be not empty: id, filter | Pass a positive number in `id`. A second message comes with this one: "Only one of the following values must be provided: id, filter" ||
|| `fields` | At least one field to update must be provided | Pass at least one field in `fields` ||
|| `name` | The field name must not be null | Pass a non-empty string in `name` ||
|| `-` | The price type ID is missing`<br>` | Pass a non-empty string in `name` ||
|| `base` | This request requires data type `bool` in field `base` | Pass `true` or `false`, not `Y` or `N` ||
|| `base` | The field base must be true or false | Do not pass `null` in `base` ||
|| `sort` | The field sort must be an integer | Do not pass `null` in `sort` ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | Field `#FIELD#` is read-only | Remove read-only fields from `fields`: `id`, `timestampX`, `modifiedBy`, `dateCreate`, `createdBy` ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `filter` | The method does not support `filter`. Pass the price type `id` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | Check the identifier using the [catalog.pricetype.list](./catalog-price-type-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `PriceTypeDto` | Pass only `name`, `base`, `sort`, `xmlId` in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "Manage price types" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-add-rest-v3.md)
- [{#T}](./catalog-price-type-get-rest-v3.md)
- [{#T}](./catalog-price-type-list-rest-v3.md)
- [{#T}](./catalog-price-type-delete-rest-v3.md)
- [{#T}](./catalog-price-type-field-list.md)
