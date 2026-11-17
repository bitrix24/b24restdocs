# Add Value to List Property v 3.0 catalog.product.property.enum.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission and permission to edit the property's information block

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.property.enum.add` method adds a value to a product or variation property of the "List" type and returns the fields of the new value. A "List" property stores a predefined set of values: for example, the "Color" property has "Red" and "Yellow".

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object with the fields of the new value. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId***
[`integer`](../../data-types.md) | Identifier of a trade catalog property of the "List" type.

The identifier can be retrieved using the [catalog.product.property.list](../product-property/catalog-product-property-list-rest-v3.md) method. To retrieve only list properties, pass `propertyType` with the value `L` in the filter ||
|| **value***
[`string`](../../data-types.md) | Value text, up to 255 characters. An empty string causes an error ||
|| **xmlId***
[`string`](../../data-types.md) | External ID of the value, up to 200 characters. The ID must be unique among the values of all list properties in Bitrix24: if a value of another property already has the ID `Y`, you cannot use it ||
|| **def**
[`boolean`](../../data-types.md) | Default value flag.

Possible values:

- `true` — default value
- `false` — regular value

The strings `Y` and `N` are not accepted. The method does not clear the flag on the other values of the property, so a property can end up with several default values. To keep only one, pass `def: false` for the previous default value using the [catalog.product.property.enum.update](./catalog-product-property-enum-update-rest-v3.md) method.

Default: `false` ||
|| **sort**
[`integer`](../../data-types.md) | Sort index.

Default: `500` ||
|#

Bitrix24 fills in the `id` field automatically. If you pass it in `fields`, the method returns an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.enum.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"propertyId":1285,"value":"Yellow","xmlId":"color-yellow","sort":300}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.enum.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"propertyId":1285,"value":"Yellow","xmlId":"color-yellow","sort":300},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.enum.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type EnumAddResult = {
      item: {
        id: number
        propertyId: number
        value: string
        def: boolean
        sort: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<EnumAddResult>({
        method: 'catalog.product.property.enum.add',
        params: {
            fields: {
              propertyId: 1285,
              value: 'Yellow',
              xmlId: 'color-yellow',
              sort: 300,
            },
          },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Value created:', result.item.id)
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
      async function addEnumValue() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.enum.add',
            params: {
                  fields: {
                    propertyId: 1285,
                    value: 'Yellow',
                    xmlId: 'color-yellow',
                    sort: 300,
                  },
                },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Value created:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addEnumValue)
    </script>
    ```

- Python

    The `catalog.product.property.enum.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.enum.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "propertyId": 1285,
                    "value": "Yellow",
                    "xmlId": "color-yellow",
                    "sort": 300,
                },
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
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
                    'catalog.product.property.enum.add',
                    [
                        'fields' => [
                            'propertyId' => 1285,
                            'value' => 'Yellow',
                            'xmlId' => 'color-yellow',
                            'sort' => 300,
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
        'catalog.product.property.enum.add',
        {
            fields: {
                propertyId: 1285,
                value: 'Yellow',
                xmlId: 'color-yellow',
                sort: 300,
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
        'catalog.product.property.enum.add',
        [
            'fields' => [
                'propertyId' => 1285,
                'value' => 'Yellow',
                'xmlId' => 'color-yellow',
                'sort' => 300,
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
    res, err := client.Core().Call(ctx, "catalog.product.property.enum.add", b24.Params{
    	"fields": b24.Params{
    		"propertyId": 1285,
    		"value":      "Yellow",
    		"xmlId":      "color-yellow",
    		"sort":       300,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.property.enum.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID         int    `json:"id"`
    	PropertyID int    `json:"propertyId"`
    	Value      string `json:"value"`
    	Def        bool   `json:"def"`
    	Sort       int    `json:"sort"`
    	XMLID      string `json:"xmlId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Value)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 1789,
            "propertyId": 1285,
            "value": "Yellow",
            "def": false,
            "sort": 300,
            "xmlId": "color-yellow"
        }
    },
    "time": {
        "start": 1794922609,
        "finish": 1794922609.767199,
        "duration": 0.7671990394592285,
        "processing": 0,
        "date_start": "2026-11-17T16:36:49+02:00",
        "date_finish": "2026-11-17T16:36:49+02:00",
        "operating_reset_at": 1794922860,
        "operating": 0.11009812355041504
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
[`object`](../../data-types.md) | Fields of the created value. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Value identifier ||
|| **propertyId**
[`integer`](../../data-types.md) | Identifier of the property the value belongs to ||
|| **value**
[`string`](../../data-types.md) | Value text ||
|| **def**
[`boolean`](../../data-types.md) | Default value flag: `true` — default value, `false` — regular value ||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|| **xmlId**
[`string`](../../data-types.md) | External ID of the value ||
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
                "message": "A record \"External ID\" set to \"blue\" already exists in the database.",
                "field": "BX_INVALID_VALUE"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `propertyId` | Required field "propertyId" is empty | Pass `propertyId` in `fields` ||
|| `value` | Required field "value" is empty | Pass `value` in `fields` ||
|| `xmlId` | Required field "xmlId" is empty | Pass `xmlId` in `fields` ||
|| `id` | Field "id" is read-only | Remove `id` from `fields` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `-` | Property #ID# does not exist | Check `propertyId` using the [catalog.product.property.list](../product-property/catalog-product-property-list-rest-v3.md) method ||
|| `-` | The specified property does not belong to a product catalog | Specify a trade catalog property ||
|| `-` | Only list properties are supported | Specify a property of the "List" type ||
|| `BX_INVALID_VALUE` | A record "External ID" set to "#XML_ID#" already exists in the database | Pass a different `xmlId`. The ID must be unique among the values of all list properties ||
|| `BX_EMPTY_REQUIRED` | The field Value is required | Pass a non-empty string in `value` ||
|| `BX_INVALID_VALUE` | The length of "Value" should not exceed 255 characters | Shorten `value` to 255 characters ||
|| `def` | This request requires data type `bool` in field `def` | Pass `true` or `false`, not `Y` or `N` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductPropertyEnumDto` | Pass in `fields` only the fields from the `fields` parameter table ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check that the user has the "View product catalog" access permission and permission to edit the property's information block ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-enum-update-rest-v3.md)
- [{#T}](./catalog-product-property-enum-get-rest-v3.md)
- [{#T}](./catalog-product-property-enum-list-rest-v3.md)
- [{#T}](./catalog-product-property-enum-delete-rest-v3.md)
- [{#T}](./catalog-product-property-enum-field-list.md)
