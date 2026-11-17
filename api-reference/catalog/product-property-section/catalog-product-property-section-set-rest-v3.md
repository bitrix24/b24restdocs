# Set Section Property Settings v 3.0 catalog.product.property.section.set

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

The `catalog.product.property.section.set` method sets how a product or variation property is displayed in the smart filter of the catalog or one of its sections, and returns the stored settings. In the smart filter, customers select products by property values. For example, the "Color" property can be shown as checkboxes in the "Clothing" section and hidden in the "Footwear" section.

If there are no settings for the property–section pair yet, the method creates them; if there are, it updates them. Settings for the entire catalog, `sectionId: 0`, are created together with the property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId***
[`integer`](../../data-types.md) | Trade catalog property identifier.

The identifier can be retrieved using the [catalog.product.property.list](../product-property/catalog-product-property-list-rest-v3.md) method ||
|| **sectionId**
[`integer`](../../data-types.md) | Catalog section identifier. `0` — settings for the entire catalog.

The section must belong to the property's information block, so a product section does not fit a variation property. The section identifier can be retrieved using the [catalog.section.list](../section/catalog-section-list-rest-v3.md) method.

Default: `0` ||
|| **fields***
[`object`](../../data-types.md) | Object with the settings. Pass only the fields you want to change: the rest retain their current values. An empty object `{}` changes nothing. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **smartFilter**
[`boolean`](../../data-types.md) | Whether to show the property in the smart filter.

Possible values:

- `true` — show
- `false` — do not show

The values `Y`, `N`, and `null` are not accepted ||
|| **displayType**
[`string`](../../data-types.md) | Property display type in the smart filter. The available display types depend on the property type:

- `F` — checkboxes
- `K` — radio buttons
- `P` — dropdown list
- `G` — checkboxes with images, for a directory property
- `H` — checkboxes with names and images, for a directory property
- `R` — dropdown list with names and images, for a directory property
- `U` — calendar, for a "Date/Time" property
- `A` — number range with a slider, for a "Number" property
- `B` — number range, for a "Number" property

Pass a letter from the list: the method does not validate the value and stores any other letter without an error ||
|| **displayExpanded**
[`boolean`](../../data-types.md) | Whether to show the property expanded in the filter.

Possible values:

- `true` — expanded
- `false` — collapsed

The values `Y`, `N`, and `null` are not accepted ||
|| **filterHint**
[`string`](../../data-types.md) | Hint for the property in the smart filter ||
|#

You cannot pass the `propertyId`, `sectionId`, and `iblockId` fields in `fields`: the property and the section are set by the method parameters, and the information block is determined by the property.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.section.set`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"propertyId":1285,"sectionId":31,"fields":{"smartFilter":true,"displayType":"F","displayExpanded":true,"filterHint":"Select a color"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.section.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"propertyId":1285,"sectionId":31,"fields":{"smartFilter":true,"displayType":"F","displayExpanded":true,"filterHint":"Select a color"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.section.set
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SectionSetResult = {
      item: {
        propertyId: number
        sectionId: number
        iblockId: number
        smartFilter: boolean
        displayType: string | null
        displayExpanded: boolean
        filterHint: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<SectionSetResult>({
        method: 'catalog.product.property.section.set',
        params: {
            propertyId: 1285,
            sectionId: 31,
            fields: {
              smartFilter: true,
              displayType: 'F',
              displayExpanded: true,
              filterHint: 'Select a color',
            },
          },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Smart filter:', result.item.smartFilter)
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
      async function setPropertySection() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.section.set',
            params: {
                  propertyId: 1285,
                  sectionId: 31,
                  fields: {
                    smartFilter: true,
                    displayType: 'F',
                    displayExpanded: true,
                    filterHint: 'Select a color',
                  },
                },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Smart filter:', result.item.smartFilter)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setPropertySection)
    </script>
    ```

- Python

    The `catalog.product.property.section.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.section.set"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "propertyId": 1285,
                "sectionId": 31,
                "fields": {
                    "smartFilter": True,
                    "displayType": "F",
                    "displayExpanded": True,
                    "filterHint": "Select a color",
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
                    'catalog.product.property.section.set',
                    [
                        'propertyId' => 1285,
                        'sectionId' => 31,
                        'fields' => [
                            'smartFilter' => true,
                            'displayType' => 'F',
                            'displayExpanded' => true,
                            'filterHint' => 'Select a color',
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
        'catalog.product.property.section.set',
        {
            propertyId: 1285,
            sectionId: 31,
            fields: {
                smartFilter: true,
                displayType: 'F',
                displayExpanded: true,
                filterHint: 'Select a color',
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
        'catalog.product.property.section.set',
        [
            'propertyId' => 1285,
            'sectionId' => 31,
            'fields' => [
                'smartFilter' => true,
                'displayType' => 'F',
                'displayExpanded' => true,
                'filterHint' => 'Select a color',
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
    res, err := client.Core().Call(ctx, "catalog.product.property.section.set", b24.Params{
    	"propertyId": 1285,
    	"sectionId":  31,
    	"fields": b24.Params{
    		"smartFilter":     true,
    		"displayType":     "F",
    		"displayExpanded": true,
    		"filterHint":      "Select a color",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.property.section.set: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	PropertyID      int     `json:"propertyId"`
    	SectionID       int     `json:"sectionId"`
    	IblockID        int     `json:"iblockId"`
    	SmartFilter     bool    `json:"smartFilter"`
    	DisplayType     *string `json:"displayType"`
    	DisplayExpanded bool    `json:"displayExpanded"`
    	FilterHint      string  `json:"filterHint"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.PropertyID, item.SectionID, item.SmartFilter)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "propertyId": 1285,
            "sectionId": 31,
            "iblockId": 25,
            "smartFilter": true,
            "displayType": "F",
            "displayExpanded": true,
            "filterHint": "Select a color"
        }
    },
    "time": {
        "start": 1794923724,
        "finish": 1794923724.832547,
        "duration": 0.8325469493865967,
        "processing": 0,
        "date_start": "2026-11-17T16:55:24+02:00",
        "date_finish": "2026-11-17T16:55:24+02:00",
        "operating_reset_at": 1794924300,
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
[`object`](../../data-types.md) | Stored settings. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId**
[`integer`](../../data-types.md) | Property identifier ||
|| **sectionId**
[`integer`](../../data-types.md) | Catalog section identifier. `0` — settings for the entire catalog ||
|| **iblockId**
[`integer`](../../data-types.md) | Identifier of the catalog information block the settings belong to. This is usually the property's information block, but for variation properties it can also be the product information block ||
|| **smartFilter**
[`boolean`](../../data-types.md) | Whether to show the property in the smart filter: `true` — show, `false` — do not show ||
|| **displayType**
[`string`](../../data-types.md) or `null` | Property display type in the smart filter, for example `F` — checkboxes. `null` if the display type is not set ||
|| **displayExpanded**
[`boolean`](../../data-types.md) | Whether to show the property expanded in the filter: `true` — expanded, `false` — collapsed ||
|| **filterHint**
[`string`](../../data-types.md) | Hint for the property in the smart filter. An empty string if the hint is not set ||
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
                "message": "Section 999999 does not belong to the iblock of property 1285"
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
|| `propertyId` | Required field `propertyId` is missing | Pass a positive number in `propertyId` ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `sectionId` | Field value must be 0 or greater | Pass a section identifier or `0` in `sectionId` ||
|| `-` | Section #SECTION_ID# does not belong to the iblock of property #PROPERTY_ID# | Specify a section from the property's information block. Sections can be retrieved using the [catalog.section.list](../section/catalog-section-list-rest-v3.md) method ||
|| `#FIELD#` | This request requires data type `bool` in field `#FIELD#` | Pass `true` or `false` in `smartFilter` and `displayExpanded` ||
|| `-` | Field `#FIELD#` must be a boolean | Do not pass `null` in `smartFilter` and `displayExpanded` ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | Field "#FIELD#" is read-only | Remove the `propertyId`, `sectionId`, and `iblockId` fields from `fields` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `propertyId` | Entity with ID `#ID#` was not found | The property does not exist or does not belong to the trade catalog. Check the identifier using the [catalog.product.property.list](../product-property/catalog-product-property-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductPropertySectionDto` | Pass only the fields from the `fields` parameter table in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check that the user has the "View product catalog" access permission and permission to edit the property's information block ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-section-get-rest-v3.md)
- [{#T}](./catalog-product-property-section-list-rest-v3.md)
- [{#T}](./catalog-product-property-section-field-list.md)
