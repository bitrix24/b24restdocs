# Get Field Values of Catalog Section v 3.0 catalog.section.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View catalog products" access permission and permission to read the section in the information block

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.section.get` returns the fields of a trade catalog section by identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Section identifier.

The identifier can be retrieved using the [catalog.section.list](./catalog-section-list-rest-v3.md) method or when creating a section with the [catalog.section.add](./catalog-section-add-rest-v3.md) method.

For a non-existent section, the method returns an access error ||
|| **select**
[`array`](../../data-types.md) | Array of strings with the names of the section fields to return in the response, for example `["id","name","iblockSectionId","active"]`.

Available fields:

- `id` — section identifier
- `iblockId` — information block identifier
- `name` — name
- `iblockSectionId` — parent section identifier
- `active` — activity
- `code` — symbolic code
- `xmlId` — external identifier
- `sort` — sort index
- `description` — description
- `descriptionType` — description format

The value `["*"]` is not supported.

Default: all fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.section.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":387,"select":["id","name","iblockSectionId","active"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.section.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":387,"select":["id","name","iblockSectionId","active"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.section.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type SectionGetResult = {
      item: {
        id?: number
        name?: string
        iblockSectionId?: number
        active?: 'Y' | 'N'
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<SectionGetResult>({
        method: 'catalog.section.get',
        params: {
          id: 387,
          select: [
            'id',
            'name',
            'iblockSectionId',
            'active',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Section:', result.item.id, result.item.name)
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
      async function getSection() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.section.get',
            params: {
              id: 387,
              select: [
                'id',
                'name',
                'iblockSectionId',
                'active',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Section:', result.item.id, result.item.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getSection)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    select = [
        "id",
        "name",
        "iblockSectionId",
        "active",
    ]

    try:
        bitrix_response = client.catalog.section.get(
            bitrix_id=387,
            select=select,
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
                'catalog.section.get',
                [
                    'id' => 387,
                    'select' => [
                        'id',
                        'name',
                        'iblockSectionId',
                        'active',
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
        'catalog.section.get',
        {
            id: 387,
            select: [
                'id',
                'name',
                'iblockSectionId',
                'active',
            ],
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
        'catalog.section.get',
        [
            'id' => 387,
            'select' => [
                'id',
                'name',
                'iblockSectionId',
                'active',
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
    res, err := client.Core().Call(ctx, "catalog.section.get", b24.Params{
    	"id":     387,
    	"select": []string{"id", "name", "iblockSectionId", "active"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.section.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID              int    `json:"id"`
    	Name            string `json:"name"`
    	IblockSectionID int    `json:"iblockSectionId"`
    	Active          string `json:"active"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name, item.IblockSectionID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 387,
            "name": "Bags and Backpacks",
            "iblockSectionId": 385,
            "active": "N"
        }
    },
    "time": {
        "start": 1792665608,
        "finish": 1792665608.633603,
        "duration": 0.6336030960083008,
        "processing": 0,
        "date_start": "2026-10-22T13:40:08+02:00",
        "date_finish": "2026-10-22T13:40:08+02:00",
        "operating_reset_at": 1792666200,
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
[`object`](../../data-types.md) | Section fields. The set depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

The table contains all fields that can be requested in `select`.

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

HTTP status: **403**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION",
        "message": "Access denied"
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass `id` — a positive number. A string value also causes this error ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `SectionDto` | Pass in `select` only the fields from the [Item Object](#item) table ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["id","name"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions for the catalog and the section. The error is also returned for a non-existent `id` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-section-add-rest-v3.md)
- [{#T}](./catalog-section-update-rest-v3.md)
- [{#T}](./catalog-section-list-rest-v3.md)
- [{#T}](./catalog-section-delete-rest-v3.md)
- [{#T}](./catalog-section-field-get.md)
