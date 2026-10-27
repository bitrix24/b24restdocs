# Get Information Block Element iblock.element.get

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

The method `iblock.element.get` returns an information block element by identifier.

{% note warning "" %}

The method works only with information blocks that have an "API symbolic code" set and the "Access via REST is enabled" option selected in the admin panel. Cloud Bitrix24 has no admin panel, so these settings cannot be set and the method returns an error. For details, see the [section limitations and specifics](./index.md#restrictions).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblock***
[`integer`](../data-types.md) | Identifier of the information block that contains the element. The value must be greater than `0`.

Note that the parameter is named `iblock`, not `iblockId`. In information block validation errors, the field is named `iblockId` ||
|| **id***
[`integer`](../data-types.md) | Element identifier.

The identifier can be retrieved using the [iblock.element.list](./iblock-element-list.md) method ||
|| **select**
[`array`](../data-types.md) | Array of element fields to return in the response.

Available fields:

- `id` — element identifier
- `iblockId` — information block identifier
- `name` — element name
- `iblockSectionId` — information block section identifier
- `properties` — element property values

Default: `id`, `iblockId`, `name`, `iblockSectionId`. Without `select`, the `properties` field is returned only if the element has filled properties ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/iblock.element.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"iblock":14,"id":215,"select":["id","name","iblockSectionId","properties"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/iblock.element.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"iblock":14,"id":215,"select":["id","name","iblockSectionId","properties"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/iblock.element.get
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

    type ElementGetResult = {
      item: {
        id: number
        name: string
        iblockSectionId: number | null
        properties: Record<string, PropertyValue | PropertyValue[]>
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ElementGetResult>({
        method: 'iblock.element.get',
        params: {
          iblock: 14,
          id: 215,
          select: ['id', 'name', 'iblockSectionId', 'properties'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.name, result.item.properties)
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
      async function getElement() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'iblock.element.get',
            params: {
              iblock: 14,
              id: 215,
              select: ['id', 'name', 'iblockSectionId', 'properties'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.item.name, result.item.properties)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getElement)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.iblock.element.get(
            iblock=14,
            id=215,
            select=["id", "name", "iblockSectionId", "properties"],
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
                'iblock.element.get',
                [
                    'iblock' => 14,
                    'id' => 215,
                    'select' => ['id', 'name', 'iblockSectionId', 'properties'],
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
        'iblock.element.get',
        {
            iblock: 14,
            id: 215,
            select: ['id', 'name', 'iblockSectionId', 'properties'],
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
        'iblock.element.get',
        [
            'iblock' => 14,
            'id' => 215,
            'select' => ['id', 'name', 'iblockSectionId', 'properties'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "iblock.element.get", b24.Params{
    	"iblock": 14,
    	"id":     215,
    	"select": []string{"id", "name", "iblockSectionId", "properties"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("iblock.element.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID              int                        `json:"id"`
    	Name            string                     `json:"name"`
    	IblockSectionID *int                       `json:"iblockSectionId"`
    	Properties      map[string]json.RawMessage `json:"properties"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name, len(item.Properties))
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 215,
            "name": "Office chair",
            "iblockSectionId": 8,
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
        }
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
|| **result.item**
[`object`](../data-types.md) | Information block element. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

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
|| `iblock` | Required field `iblock` is missing | Pass the information block identifier in the `iblock` parameter ||
|| `iblock` | Field value must be 1 or greater | Pass a positive information block identifier ||
|| `id` | Required field `id` is missing | Pass the element identifier ||
|| `iblockId` | Iblock not found. | There is no information block with this identifier. Check the `iblock` value ||
|| `iblockId` | Unable to compile iblock entity. | The information block has no API code. Set it in the admin panel. This is not possible in cloud Bitrix24 ||
|| `iblockId` | Restricted iblock ID:`#ID#` (`#API_CODE#`) | REST access is disabled for the information block. Enable it in the admin panel ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Entity with ID `#ID#` was not found | There is no element with this identifier in the information block. Check `id` and `iblock` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `ElementDto` | Specify only the fields from the [item object description](#item) ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./iblock-element-list.md)
- [{#T}](./iblock-element-field-list.md)
- [{#T}](./index.md)
- [{#T}](../lists/index.md)
