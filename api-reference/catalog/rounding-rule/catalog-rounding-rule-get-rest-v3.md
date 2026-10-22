# Get Price Rounding Rule Field Values v 3.0 catalog.roundingrule.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "Manage price types" permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.roundingrule.get` returns the fields of a price rounding rule by its identifier.

{% note warning "" %}

In REST 3.0, the method group name is written in lowercase: `catalog.roundingrule.*`. Calling `catalog.roundingRule.*` at the `/rest/api/` address returns the `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION` error.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Rounding rule identifier.

The identifier can be retrieved using the [catalog.roundingrule.list](./catalog-rounding-rule-list-rest-v3.md) method ||
|| **select**
[`array`](../../data-types.md) | Array of rule fields to return in the response.

Available fields:

- `id` — rounding rule identifier
- `catalogGroupId` — price type identifier
- `price` — minimum price from which the rule applies
- `roundType` — rounding type
- `roundPrecision` — rounding precision
- `createdBy` — identifier of the rule author
- `dateCreate` — creation date and time
- `modifiedBy` — identifier of the user who last modified the rule
- `dateModify` — date and time of the last modification

The `["*"]` value is not supported.

By default, all fields are returned ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.roundingrule.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.roundingrule.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":3,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.roundingrule.get
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

    type RoundingRuleGetResult = {
      item: RoundingRule
    }

    try {
      const response = await $b24.actions.v3.call.make<RoundingRuleGetResult>({
        method: 'catalog.roundingrule.get',
        params: {
          id: 3,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Rounding rule:', result.item)
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
      async function getRoundingRule() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.roundingrule.get',
            params: {
              id: 3,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Rounding rule:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getRoundingRule)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.roundingrule.get(
            bitrix_id=3,
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
                'catalog.roundingrule.get',
                [
                    'id' => 3,
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
        'catalog.roundingrule.get',
        {
            id: 3,
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
        'catalog.roundingrule.get',
        [
            'id' => 3,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.roundingrule.get", b24.Params{
    	"id": 3,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.roundingrule.get: %w", err)
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
            "dateModify": "2026-10-22T14:20:37+02:00"
        }
    },
    "time": {
        "start": 1792668042,
        "finish": 1792668042.04575,
        "duration": 0.04574990272521973,
        "processing": 0,
        "date_start": "2026-10-22T14:20:42+02:00",
        "date_finish": "2026-10-22T14:20:42+02:00",
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
[`object`](../../data-types.md) | Rounding rule fields. [Field description](#item) ||
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
|| `id` | Required field `id` is missing | Pass a positive integer in `id`. The same error is returned for `0`, a negative number, and a string ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no rounding rule with this identifier. Check the identifier using the [catalog.roundingrule.list](./catalog-rounding-rule-list-rest-v3.md) method ||
|#

#### Unknown Field Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `RoundingRuleDto` | Pass only the `id`, `catalogGroupId`, `price`, `roundType`, `roundPrecision`, `createdBy`, `dateCreate`, `modifiedBy`, `dateModify` fields in `select` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example, `["id"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" or "Manage price types" permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-rounding-rule-add-rest-v3.md)
- [{#T}](./catalog-rounding-rule-update-rest-v3.md)
- [{#T}](./catalog-rounding-rule-list-rest-v3.md)
- [{#T}](./catalog-rounding-rule-delete-rest-v3.md)
- [{#T}](./catalog-rounding-rule-field-list.md)
