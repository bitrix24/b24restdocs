# Get List of Information Block Elements iblock.element.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`iblock`](../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

The method `iblock.element.list` returns a list of information block elements with filtering, sorting, and pagination.

{% note warning "" %}

The method works only with information blocks that have an "API symbolic code" set and the "Access via REST is enabled" option selected in the admin panel. Cloud Bitrix24 has no admin panel, so these settings cannot be set and the method returns an error. For details, see the [section limitations and specifics](./index.md#restrictions).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../data-types.md) | List filter in the REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`.

The filter is required and must contain an equality condition on `iblockId` with a positive information block identifier, for example `[["iblockId",14]]`. The method returns elements of this information block only.

You can filter by the `id`, `iblockId`, `name`, and `iblockSectionId` fields. Filtering by `properties` and other element fields is not supported.

The filter syntax is described in the [REST 3.0 overview](../rest-v3.md#filter) ||
|| **select**
[`array`](../data-types.md) | Array of element fields to return in the response.

Available fields:

- `id` — element identifier
- `iblockId` — information block identifier
- `name` — element name
- `iblockSectionId` — information block section identifier
- `properties` — element property values

Default: `id`, `iblockId`, `name`, `iblockSectionId`. Without `select`, the `properties` field is returned only if the element has filled properties ||
|| **order**
[`object`](../data-types.md) | Sorting in the `{"field": "direction"}` format, for example `{"id":"ASC"}`.

You can sort by the `id`, `name`, and `iblockSectionId` fields.

Possible direction values:

- `ASC` — ascending
- `DESC` — descending ||
|| **pagination**
[`object`](../data-types.md) | Pagination parameters. [Object structure description](#pagination) ||
|#

### Parameter pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../data-types.md) | Number of elements per page. Values greater than `1000` are reduced to `1000`. Zero, negative, and non-numeric values cause an error.

Default: `50` ||
|| **page**
[`integer`](../data-types.md) | Page number starting from `1`. The offset is calculated as `(page - 1) * limit`. If `page` is passed, the `offset` value is ignored ||
|| **offset**
[`integer`](../data-types.md) | Number of records to skip from the beginning of the list.

Default: `0` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/iblock.element.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["iblockId",14],["iblockSectionId",8]],"select":["id","name","properties"],"order":{"id":"ASC"},"pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/iblock.element.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["iblockId",14],["iblockSectionId",8]],"select":["id","name","properties"],"order":{"id":"ASC"},"pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/iblock.element.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PropertyValue = {
      id: number
      value: string
      description: string | null
    }

    type ElementListResult = {
      items: Array<{
        id: number
        name: string
        properties: Record<string, PropertyValue | PropertyValue[]> | []
      }>
    }

    try {
      const response = await $b24.actions.v3.call.make<ElementListResult>({
        method: 'iblock.element.list',
        params: {
          filter: [
            ['iblockId', 14],
            ['iblockSectionId', 8],
          ],
          select: ['id', 'name', 'properties'],
          order: { id: 'ASC' },
          pagination: { limit: 50 },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        result.items.forEach(item => console.info(item.id, item.name))
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
      async function listElements() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'iblock.element.list',
            params: {
              filter: [
                ['iblockId', 14],
                ['iblockSectionId', 8],
              ],
              select: ['id', 'name', 'properties'],
              order: { id: 'ASC' },
              pagination: { limit: 50 },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          result.items.forEach(item => console.info(item.id, item.name))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listElements)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.iblock.element.list(
            filter=[
                ["iblockId", 14],
                ["iblockSectionId", 8],
            ],
            select=["id", "name", "properties"],
            order={"id": "ASC"},
            pagination={"limit": 50},
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
                'iblock.element.list',
                [
                    'filter' => [
                        ['iblockId', 14],
                        ['iblockSectionId', 8],
                    ],
                    'select' => ['id', 'name', 'properties'],
                    'order' => ['id' => 'ASC'],
                    'pagination' => ['limit' => 50],
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
        'iblock.element.list',
        {
            filter: [
                ['iblockId', 14],
                ['iblockSectionId', 8],
            ],
            select: ['id', 'name', 'properties'],
            order: { id: 'ASC' },
            pagination: { limit: 50 },
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
        'iblock.element.list',
        [
            'filter' => [
                ['iblockId', 14],
                ['iblockSectionId', 8],
            ],
            'select' => ['id', 'name', 'properties'],
            'order' => ['id' => 'ASC'],
            'pagination' => ['limit' => 50],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "iblock.element.list", b24.Params{
    	"filter": []any{
    		[]any{"iblockId", 14},
    		[]any{"iblockSectionId", 8},
    	},
    	"select": []string{"id", "name", "properties"},
    	"order": b24.Params{
    		"id": "ASC",
    	},
    	"pagination": b24.Params{
    		"limit": 50,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("iblock.element.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID         int             `json:"id"`
    		Name       string          `json:"name"`
    		Properties json.RawMessage `json:"properties"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Name)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 215,
                "name": "Office chair",
                "properties": {
                    "COLOR": {
                        "id": 1043,
                        "value": "Black",
                        "description": null
                    },
                    "SIZE": [
                        {
                            "id": 1044,
                            "value": "M",
                            "description": null
                        },
                        {
                            "id": 1045,
                            "value": "L",
                            "description": null
                        }
                    ]
                }
            },
            {
                "id": 216,
                "name": "Writing desk",
                "properties": []
            }
        ]
    },
    "time": {
        "start": 1792670785,
        "finish": 1792670785.424705,
        "duration": 0.42470502853393555,
        "processing": 0,
        "date_start": "2026-10-22T15:06:25+02:00",
        "date_finish": "2026-10-22T15:06:25+02:00",
        "operating_reset_at": 1792671360,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with the response data ||
|| **result.items**
[`array`](../data-types.md) | Array of information block elements on the page. The set of fields depends on `select`. [Field description](#items) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

The method does not return the total number of records. If the response contains fewer records than `limit`, the page is the last one.

#### Items Element {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Element identifier ||
|| **iblockId**
[`integer`](../data-types.md) | Information block identifier ||
|| **name**
[`string`](../data-types.md) | Element name ||
|| **iblockSectionId**
[`integer`](../data-types.md) | Information block section identifier. `null` if the element is not linked to a section ||
|| **properties**
[`object`](../data-types.md) | Element property values. The key is the property symbolic code, the value is a [value object](#property-value) for a single property or an array of such objects for a multiple property.

If `properties` is specified in `select` but the element has no properties, an empty array `[]` is returned ||
|#

#### Property Value Object {#property-value}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Property value identifier ||
|| **value**
[`string`](../data-types.md) | Property value ||
|| **description**
[`string`](../data-types.md) | Value description. `null` if no description is set ||
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
                "message": "Unable to compile iblock entity.",
                "field": "iblockId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Field value must not be empty | Pass `filter` with a condition on `iblockId` ||
|| `filter` | iblockId must be a positive integer. | Pass a positive information block identifier in the `iblockId` condition ||
|| `iblockId` | Iblock not found. | There is no information block with this identifier. Check the `iblockId` value in the filter ||
|| `iblockId` | Unable to compile iblock entity. | The information block has no API code. Set it in the admin panel. This is not possible in cloud Bitrix24 ||
|| `iblockId` | Restricted iblock ID:`#ID#` (`#API_CODE#`) | REST access is disabled for the information block. Enable it in the admin panel ||
|| `#FIELD#` | DTO `ElementDto` in field `#FIELD#` requires attribute `Sortable` to perform this request | Sort only by the `id`, `name`, and `iblockSectionId` fields ||
|#

#### Required Filter Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter.iblockId` | Filter for required field `iblockId` is required | Add the `["iblockId", N]` condition to `filter` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select`, `filter` | Unknown field `#FIELD#` found in entity `ElementDto` | Specify only the fields from the [items element description](#items) ||
|#

#### Sorting Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Invalid sort order `#ORDER#` | Pass the `ASC` or `DESC` direction ||
|#

#### Pagination Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `#PAGINATION#` | Pass `limit` as a positive number, `offset` as a non-negative number, and `page` as a number ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./iblock-element-get.md)
- [{#T}](./iblock-element-field-list.md)
- [{#T}](./index.md)
- [{#T}](../lists/index.md)
