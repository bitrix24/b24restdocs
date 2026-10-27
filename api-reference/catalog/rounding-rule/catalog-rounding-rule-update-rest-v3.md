# Update Price Rounding Rule v 3.0 catalog.roundingrule.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Manage price types" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.roundingrule.update` updates a price rounding rule and returns its fields after the update.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Rounding rule identifier.

The identifier can be retrieved using the [catalog.roundingrule.list](./catalog-rounding-rule-list-rest-v3.md) method ||
|| **fields***
[`object`](../../data-types.md) | Object with the rounding rule fields to update. Pass only the fields you change. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **catalogGroupId**
[`integer`](../../data-types.md) | Identifier of the price type the rule applies to. The price type must exist.

The identifier can be retrieved using the [catalog.pricetype.list](../price-type/catalog-price-type-list-rest-v3.md) method ||
|| **price**
[`double`](../../data-types.md) | Minimum price from which the rule applies. `0` — the rule applies to all prices of the type.

A negative value causes an error. A price type cannot have two rules with the same `price` ||
|| **roundType**
[`integer`](../../data-types.md) | Rounding type.

Possible values:

- `1` — mathematical rounding
- `2` — rounding up, in favor of the store
- `4` — rounding down, in favor of the customer ||
|| **roundPrecision**
[`double`](../../data-types.md) | Rounding precision, for example, `10` — rounding to tens.

Possible values: `0.0001`, `0.001`, `0.005`, `0.01`, `0.02`, `0.05`, `0.1`, `0.2`, `0.5`, `1`, `2`, `5`, `10`, `20`, `50`, `100`, `200`, `500`, `1000`, `5000` ||
|#

The `id`, `createdBy`, `dateCreate`, `modifiedBy`, and `dateModify` fields are filled in automatically. If you pass them in `fields`, the method returns an error. An empty `fields` object does not change the rule fields but updates `modifiedBy` and `dateModify`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.roundingrule.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3,"fields":{"roundType":4,"roundPrecision":100}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.roundingrule.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3,"fields":{"roundType":4,"roundPrecision":100},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.roundingrule.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type RoundingRule = {
      id: number
      catalogGroupId: number
      price: number
      roundType: 1 | 2 | 4
      roundPrecision: number
      createdBy: number
      dateCreate: ISODate
      modifiedBy: number
      dateModify: ISODate
    }

    type RoundingRuleUpdateResult = {
      item: RoundingRule
    }

    try {
      const response = await $b24.actions.v3.call.make<RoundingRuleUpdateResult>({
        method: 'catalog.roundingrule.update',
        params: {
          id: 3,
          fields: {
            roundType: 4,
            roundPrecision: 100,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Rounding rule updated:', result.item.id)
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
      async function updateRoundingRule() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.roundingrule.update',
            params: {
              id: 3,
              fields: {
                roundType: 4,
                roundPrecision: 100,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Rounding rule updated:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateRoundingRule)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.roundingrule.update(
            bitrix_id=3,
            fields={
                "roundType": 4,
                "roundPrecision": 100,
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
                'catalog.roundingrule.update',
                [
                    'id' => 3,
                    'fields' => [
                        'roundType' => 4,
                        'roundPrecision' => 100,
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
        'catalog.roundingrule.update',
        {
            id: 3,
            fields: {
                roundType: 4,
                roundPrecision: 100,
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
        'catalog.roundingrule.update',
        [
            'id' => 3,
            'fields' => [
                'roundType' => 4,
                'roundPrecision' => 100,
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
    res, err := client.Core().Call(ctx, "catalog.roundingrule.update", b24.Params{
    	"id":     3,
    	"fields": b24.Params{
    		"roundType":      4,
    		"roundPrecision": 100,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.roundingrule.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID             int     `json:"id"`
    	CatalogGroupID int     `json:"catalogGroupId"`
    	Price          float64 `json:"price"`
    	RoundType      int     `json:"roundType"`
    	RoundPrecision float64 `json:"roundPrecision"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.RoundType, item.RoundPrecision)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 3,
            "catalogGroupId": 25,
            "price": 1000,
            "roundType": 4,
            "roundPrecision": 100,
            "createdBy": 1,
            "dateCreate": "2026-10-22T14:20:16+02:00",
            "modifiedBy": 1,
            "dateModify": "2026-10-22T14:20:36+02:00"
        }
    },
    "time": {
        "start": 1792668036,
        "finish": 1792668036.056195,
        "duration": 0.05619502067565918,
        "processing": 0,
        "date_start": "2026-10-22T14:20:36+02:00",
        "date_finish": "2026-10-22T14:20:36+02:00",
        "operating_reset_at": 1792668600,
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
[`object`](../../data-types.md) | Rounding rule fields after the update. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Rounding rule identifier ||
|| **catalogGroupId**
[`integer`](../../data-types.md) | Identifier of the price type the rule applies to ||
|| **price**
[`double`](../../data-types.md) | Minimum price from which the rule applies. `0` — the rule applies to all prices of the type ||
|| **roundType**
[`integer`](../../data-types.md) | Rounding type: `1` — mathematical, `2` — up, `4` — down ||
|| **roundPrecision**
[`double`](../../data-types.md) | Rounding precision, for example, `10` — to tens ||
|| **createdBy**
[`integer`](../../data-types.md) | Identifier of the user who created the rule ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date and time the rule was created ||
|| **modifiedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the rule ||
|| **dateModify**
[`datetime`](../../data-types.md) | Date and time the rule was last modified ||
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
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the rounding rule `id`. This message comes together with a second one: "Only one of the following values must be provided: id, filter" ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `catalogGroupId` | The specified price type does not exist | Pass the identifier of an existing price type. It can be retrieved using the [catalog.pricetype.list](../price-type/catalog-price-type-list-rest-v3.md) method ||
|| `catalogGroupId` | This request requires data type `int` in field `catalogGroupId` | Pass an integer ||
|| `price` | The rounding rule price threshold cannot be negative | Pass a number of `0` or greater in `price` ||
|| `price` | A rounding rule with the same price threshold already exists for this price type | Change `price` or update the existing rule using the [catalog.roundingrule.update](./catalog-rounding-rule-update-rest-v3.md) method ||
|| `roundType` | Invalid rounding type provided. The available values are: 1, 2, 4 | Pass `1`, `2`, or `4` ||
|| `roundPrecision` | Invalid rounding precision provided. The available values are: 0.0001, 0.001, 0.005, 0.01, 0.02, 0.05, 0.1, 0.2, 0.5, 1, 2, 5, 10, 20, 50, 100, 200, 500, 1000, 5000 | Pass one of the values from the list ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | Field "#FIELD#" is read-only | Remove the read-only fields from `fields`: `id`, `createdBy`, `dateCreate`, `modifiedBy`, `dateModify` ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `filter` | The method does not support `filter`. Pass the rounding rule `id` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no rounding rule with this identifier. Check the identifier using the [catalog.roundingrule.list](./catalog-rounding-rule-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `RoundingRuleDto` | Pass only `catalogGroupId`, `price`, `roundType`, `roundPrecision` in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "Manage price types" permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-rounding-rule-add-rest-v3.md)
- [{#T}](./catalog-rounding-rule-get-rest-v3.md)
- [{#T}](./catalog-rounding-rule-list-rest-v3.md)
- [{#T}](./catalog-rounding-rule-delete-rest-v3.md)
- [{#T}](./catalog-rounding-rule-field-list.md)
