# Get Section Property Settings v 3.0 catalog.product.property.section.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.property.section.get` method returns the smart filter settings of a property for the entire catalog or for a single section.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **propertyId***
[`integer`](../../data-types.md) | Property identifier.

The identifier can be retrieved using the [catalog.product.property.list](../product-property/catalog-product-property-list-rest-v3.md) method ||
|| **sectionId**
[`integer`](../../data-types.md) | Catalog section identifier. `0` — settings for the entire catalog.

The section identifier can be retrieved using the [catalog.section.list](../section/catalog-section-list-rest-v3.md) method.

Default: `0` ||
|| **select**
[`array`](../../data-types.md) | Array of fields to return in the response.

Available fields:

- `propertyId` — property identifier
- `sectionId` — section identifier
- `iblockId` — catalog information block identifier
- `smartFilter` — whether to show the property in the smart filter
- `displayType` — property display type in the smart filter
- `displayExpanded` — whether to show the property expanded in the smart filter
- `filterHint` — hint in the smart filter

Default: all fields ||
|#

If the section has no settings, the method returns the `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` error. To find out which sections have settings for the property, use the [catalog.product.property.section.list](./catalog-product-property-section-list-rest-v3.md) method with a filter by `propertyId`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.section.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"propertyId":1285,"sectionId":31,"select":["propertyId","sectionId","smartFilter","displayType","displayExpanded","filterHint"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.section.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"propertyId":1285,"sectionId":31,"select":["propertyId","sectionId","smartFilter","displayType","displayExpanded","filterHint"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.section.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SectionGetResult = {
      item: {
        propertyId: number
        sectionId: number
        smartFilter: boolean
        displayType: string | null
        displayExpanded: boolean
        filterHint: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<SectionGetResult>({
        method: 'catalog.product.property.section.get',
        params: {
            propertyId: 1285,
            sectionId: 31,
            select: ['propertyId', 'sectionId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
          },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Section settings:', result.item)
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
      async function getPropertySection() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.section.get',
            params: {
                  propertyId: 1285,
                  sectionId: 31,
                  select: ['propertyId', 'sectionId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
                },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Section settings:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPropertySection)
    </script>
    ```

- Python

    The `catalog.product.property.section.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.section.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "propertyId": 1285,
                "sectionId": 31,
                "select": ["propertyId", "sectionId", "smartFilter", "displayType", "displayExpanded", "filterHint"],
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
                    'catalog.product.property.section.get',
                    [
                        'propertyId' => 1285,
                        'sectionId' => 31,
                        'select' => ['propertyId', 'sectionId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
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
        'catalog.product.property.section.get',
        {
            propertyId: 1285,
            sectionId: 31,
            select: ['propertyId', 'sectionId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
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
        'catalog.product.property.section.get',
        [
            'propertyId' => 1285,
            'sectionId' => 31,
            'select' => ['propertyId', 'sectionId', 'smartFilter', 'displayType', 'displayExpanded', 'filterHint'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.property.section.get", b24.Params{
    	"propertyId": 1285,
    	"sectionId":  31,
    	"select":     []string{"propertyId", "sectionId", "smartFilter", "displayType", "displayExpanded", "filterHint"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.property.section.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	PropertyID      int     `json:"propertyId"`
    	SectionID       int     `json:"sectionId"`
    	SmartFilter     bool    `json:"smartFilter"`
    	DisplayType     *string `json:"displayType"`
    	DisplayExpanded bool    `json:"displayExpanded"`
    	FilterHint      string  `json:"filterHint"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.SectionID, item.SmartFilter, item.FilterHint)
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
            "smartFilter": true,
            "displayType": "F",
            "displayExpanded": true,
            "filterHint": "Select a color"
        }
    },
    "time": {
        "start": 1794923726,
        "finish": 1794923726.182027,
        "duration": 0.18202710151672363,
        "processing": 0,
        "date_start": "2026-11-17T16:55:26+02:00",
        "date_finish": "2026-11-17T16:55:26+02:00",
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
[`object`](../../data-types.md) | Property settings. The set of fields depends on `select`. [Field description](#item) ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 1285 was not found."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `propertyId` | Required field `propertyId` is missing | Pass a positive number in `propertyId`. The method has no `id` parameter ||
|| `sectionId` | Field value must be 0 or greater | Pass a section identifier or `0` in `sectionId` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `propertyId` | Entity with ID `#PROPERTY_ID#` was not found | The property does not exist, does not belong to the trade catalog, or the section has no settings. In each of these cases, the message contains the property identifier. To check whether the property and the section have settings, use the [catalog.product.property.section.list](./catalog-product-property-section-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `ProductPropertySectionDto` | Pass only the fields from the `select` parameter list in `select` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check that the user has the "View product catalog" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-section-set-rest-v3.md)
- [{#T}](./catalog-product-property-section-list-rest-v3.md)
- [{#T}](./catalog-product-property-section-field-list.md)
