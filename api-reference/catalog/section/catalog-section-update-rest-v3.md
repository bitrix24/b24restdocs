# Update Catalog Section v 3.0 catalog.section.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View catalog products" access permission, the "Edit product" permission, and permission to modify the section in the information block

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.section.update` updates the fields of a trade catalog section and returns the section after the update.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Section identifier.

The identifier can be retrieved using the [catalog.section.list](./catalog-section-list-rest-v3.md) method or when creating a section with the [catalog.section.add](./catalog-section-add-rest-v3.md) method.

For a non-existent section, the method returns an access error ||
|| **fields***
[`object`](../../data-types.md) | Object with the section fields to update. Pass only the fields being changed. [Field description](#fields)

An empty object `{}` returns the section unchanged ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Section name. An empty string causes a validation error ||
|| **iblockSectionId**
[`integer`](../../data-types.md) | Identifier of the parent section in the same information block. The value `0` moves the section to the top level. A section cannot be moved into itself or into its own subsection.

The identifier can be retrieved using the [catalog.section.list](./catalog-section-list-rest-v3.md) method ||
|| **active**
[`string`](../../data-types.md) | Section activity.

Possible values:

- `Y` — the section is active
- `N` — the section is inactive

Any value other than `Y` is saved as `N` ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the section. If uniqueness of section symbolic codes is enabled in the information block settings, the code must be unique within the information block ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the section ||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|| **description**
[`string`](../../data-types.md) | Section description ||
|| **descriptionType**
[`string`](../../data-types.md) | Description format.

Possible values:

- `text` — plain text
- `html` — HTML markup

Other values are saved as `text` ||
|#

The `iblockId` field cannot be changed: the section remains in the information block in which it was created.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.section.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":387,"fields":{"name":"Bags and Backpacks","sort":200,"active":"N"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.section.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":387,"fields":{"name":"Bags and Backpacks","sort":200,"active":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.section.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SectionUpdateResult = {
      item: {
        id: number
        iblockId: number
        name: string
        iblockSectionId: number
        active: 'Y' | 'N'
        code: string
        xmlId: string
        sort: number
        description: string
        descriptionType: 'text' | 'html'
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<SectionUpdateResult>({
        method: 'catalog.section.update',
        params: {
          id: 387,
          fields: {
            name: 'Bags and Backpacks',
            sort: 200,
            active: 'N',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Section updated:', result.item.id, result.item.name)
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
      async function updateSection() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.section.update',
            params: {
              id: 387,
              fields: {
                name: 'Bags and Backpacks',
                sort: 200,
                active: 'N',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Section updated:', result.item.id, result.item.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateSection)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.section.update(
            bitrix_id=387,
            fields={
                "name": "Bags and Backpacks",
                "sort": 200,
                "active": "N",
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
                'catalog.section.update',
                [
                    'id' => 387,
                    'fields' => [
                        'name' => 'Bags and Backpacks',
                        'sort' => 200,
                        'active' => 'N',
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
        'catalog.section.update',
        {
            id: 387,
            fields: {
                name: 'Bags and Backpacks',
                sort: 200,
                active: 'N',
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
        'catalog.section.update',
        [
            'id' => 387,
            'fields' => [
                'name' => 'Bags and Backpacks',
                'sort' => 200,
                'active' => 'N',
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
    res, err := client.Core().Call(ctx, "catalog.section.update", b24.Params{
    	"id": 387,
    	"fields": b24.Params{
    		"name":   "Bags and Backpacks",
    		"sort":   200,
    		"active": "N",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.section.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID     int    `json:"id"`
    	Name   string `json:"name"`
    	Active string `json:"active"`
    	Sort   int    `json:"sort"`
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
            "id": 387,
            "iblockId": 25,
            "name": "Bags and Backpacks",
            "iblockSectionId": 385,
            "active": "N",
            "code": "bags",
            "xmlId": "bags-001",
            "sort": 200,
            "description": "",
            "descriptionType": "text"
        }
    },
    "time": {
        "start": 1792665575,
        "finish": 1792665576.002021,
        "duration": 1.002021074295044,
        "processing": 1,
        "date_start": "2026-10-22T13:39:35+02:00",
        "date_finish": "2026-10-22T13:39:36+02:00",
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
[`object`](../../data-types.md) | Object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | All section fields after the update. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Section identifier ||
|| **iblockId**
[`integer`](../../data-types.md) | Identifier of the trade catalog information block ||
|| **name**
[`string`](../../data-types.md) | Section name ||
|| **iblockSectionId**
[`integer`](../../data-types.md) | Identifier of the parent section. `0` — a top-level section ||
|| **active**
[`string`](../../data-types.md) | Section activity: `Y` — active, `N` — inactive ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the section. An empty string if the code is not set ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the section. An empty string if the identifier is not set ||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|| **description**
[`string`](../../data-types.md) | Section description. An empty string if the description is not set ||
|| **descriptionType**
[`string`](../../data-types.md) | Description format: `text` or `html` ||
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
                "message": "Field iblockId cannot be changed",
                "field": "iblockId"
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
|| `-` | At least one of the required fields must be not empty: id, filter | Pass `id` — a positive number ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object in the request body ||
|| `iblockId` | Field iblockId cannot be changed | Remove `iblockId` from `fields`. To place the section in a different information block, create a new section using the [catalog.section.add](./catalog-section-add-rest-v3.md) method ||
|| `sort` | This request requires data type `int` in field `sort` | Pass a number ||
|| `-` | Section name is not specified. | Pass a non-empty `name` value ||
|| `-` | Cannot move the section inside itself | In `iblockSectionId`, specify a section that is not in the branch of the section being updated ||
|| `-` | Duplicate entry for key [code] | A section with this `code` already exists in the information block, but the user does not have access to it. Specify a different symbolic code ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `filter` | The method does not support `filter`. Pass the section `id` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `SectionDto` | Pass in `fields` only the fields from the [Parameter fields](#fields) table ||
|#

#### Duplicate Symbolic Code Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYALREADYEXISTSEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Record with ID `#ID#` already exists | Uniqueness of section symbolic codes is enabled in the information block, and section `#ID#` already uses this `code`. Specify a different symbolic code ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for the catalog and the section. The error is also returned for a non-existent section `id` or `iblockSectionId` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-section-add-rest-v3.md)
- [{#T}](./catalog-section-get-rest-v3.md)
- [{#T}](./catalog-section-list-rest-v3.md)
- [{#T}](./catalog-section-delete-rest-v3.md)
- [{#T}](./catalog-section-field-list.md)
