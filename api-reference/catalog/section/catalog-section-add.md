# Add a Section to the Catalog catalog.section.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `catalog.section.add` adds a section to the catalog.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a new catalog section ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the information block.

To obtain existing identifiers, use [catalog.catalog.list](../catalog/catalog-catalog-list.md) ||
|| **iblockSectionId**
[`catalog_section.id`](../data-types.md#catalog_section) | Identifier of the parent section.

To obtain existing identifiers, use [catalog.section.list](./catalog-section-list.md).

By default, the top level is selected ||
|| **name***
[`string`](../data-types.md) | Name of the catalog section ||
|| **xmlId**
[`string`](../data-types.md) | External identifier.

Can be used to synchronize the current catalog section with a similar position in an external system ||
|| **code**
[`string`](../data-types.md) | Code of the catalog section. Must be unique ||
|| **sort**
[`integer`](../data-types.md) | Sorting.

Default is 500 ||
|| **active**
[`string`](../data-types.md) | Indicator of the catalog section's activity:
- `Y` — active
- `N` — inactive

Default is `Y` ||
|| **description**
[`string`](../data-types.md) | Description ||
|| **descriptionType**
[`string`](../data-types.md) | Type of description. Available types: `text`, `html` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "fields": {
            "name": "Children's Toys",
            "iblockId": 14,
            "iblockSectionId": 13,
            "sort": "100",
            "active": "Y",
            "code": "toys",
            "xmlId": "myXmlId",
            "description": "Products for children - toys",
            "descriptionType": "text"
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.section.add
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "fields": {
            "name": "Children's Toys",
            "iblockId": 14,
            "iblockSectionId": 13,
            "sort": "100",
            "active": "Y",
            "code": "toys",
            "xmlId": "myXmlId",
            "description": "Products for children - toys",
            "descriptionType": "text"
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/catalog.section.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type SectionAddResult = {
      section: {
        active: string
        code: string
        description: string
        descriptionType: string
        iblockId: number
        iblockSectionId: number
        id: number
        name: string
        sort: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<SectionAddResult>({
        method: 'catalog.section.add',
        params: {
          fields: {
            name: "Kids Toys",
            iblockId: 14,
            iblockSectionId: 13,
            sort: '100',
            active: 'Y',
            code: 'toys',
            xmlId: 'myXmlId',
            description: "Products for children - toys",
            descriptionType: "text",
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.section.id, result.section.name)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addCatalogSection() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.section.add',
            params: {
              fields: {
                name: "Kids Toys",
                iblockId: 14,
                iblockSectionId: 13,
                sort: '100',
                active: 'Y',
                code: 'toys',
                xmlId: 'myXmlId',
                description: "Products for children - toys",
                descriptionType: "text",
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.section.id, result.section.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCatalogSection)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.section.add',
                [
                    'fields' => [
                        'name'            => 'Children\'s Toys',
                        'iblockId'        => 14,
                        'iblockSectionId' => 13,
                        'sort'            => '100',
                        'active'          => 'Y',
                        'code'            => 'toys',
                        'xmlId'           => 'myXmlId',
                        'description'     => "Products for children - toys",
                        'descriptionType' => "text",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding catalog section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.section.add', 
        {
            fields: {
                name: 'Children\'s Toys',
                iblockId: 14,
                iblockSectionId: 13,
                sort: '100',
                active: 'Y',
                code: 'toys',
                xmlId: 'myXmlId',
                description: "Products for children - toys",
                descriptionType: "text"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.section.add',
        [
            'fields' => [
                'name' => 'Children\'s Toys',
                'iblockId' => 14,
                'iblockSectionId' => 13,
                'sort' => '100',
                'active' => 'Y',
                'code' => 'toys',
                'xmlId' => 'myXmlId',
                'description' => 'Products for children - toys',
                'descriptionType' => 'text'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "section": {
            "active": "Y",
            "code": "toys",
            "description": "Products for children - toys",
            "descriptionType": "text",
            "iblockId": 14,
            "iblockSectionId": 13,
            "id": 31,
            "name": "Children's Toys",
            "sort": 100,
            "xmlId": "myXmlId"
        }
    },
    "time": {
        "start": 1716552521.40908,
        "finish": 1716552521.69852,
        "duration": 0.289434909820557,
        "processing": 0.011207103729248,
        "date_start": "2024-05-24T14:08:41+02:00",
        "date_finish": "2024-05-24T14:08:41+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **section**
[`catalog_section`](../data-types.md#catalog_section) | Object containing information about the added catalog section ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":200040300040,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300040` | No access to editing ||
|| `200700300000` | Errors while adding, for example, the identifier of the information block of the created section does not match the identifier of the parent section ||
|| `200700300040` | Violation of the uniqueness of the `code` field ||
|| `200700300050` | Information block with the specified `iblockId` does not exist ||
|| `100` | Required parameter `fields` not provided ||
|| `0` | Required fields not set ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-section-update.md)
- [{#T}](./catalog-section-get.md)
- [{#T}](./catalog-section-list.md)
- [{#T}](./catalog-section-delete.md)
- [{#T}](./catalog-section-get-fields.md)