# Get a List of Pipelines crm.category.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.&lt;type&gt;.category.list](../../rest-v3/categories/crm-entity-category-list-rest-v3.md). Pipelines of invoices, CRM documents, and B2E documents are not available in REST 3.0 yet, so use this method for them.

{% endnote %}

The `crm.category.list` method retrieves a list of pipelines that belong to the CRM object type with the identifier `entityTypeId`.

{% note warning "Which pipelines will be included in the list" %}

The method returns only the pipelines the user has read permission for.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) of the CRM object for which to retrieve the list of pipelines. The object type must support pipelines: [list of supported objects](./index.md#which-objects-support-pipelines) ||
|| **filter**
[`object`](../../../data-types.md) | Optional pipeline filter [(detailed description)](#filter). Empty by default ||
|| **isCategoryMenu**
[`boolean`](../../../data-types.md) | If `true`, returns pipeline data for the menu with the `counter` field. Optional parameter, `false` by default ||
|#

### Filter Parameter {#filter}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../../data-types.md) | Exact pipeline code. For example, `CATALOG_CONTRACTOR_CONTACT` for the contact vendors pipeline. If omitted, no filtering by code is applied. Other filter fields and operators are not supported ||
|#

The method returns the full list of pipelines available to the user that match the filter, without pagination.

## Code Examples

Get the list of pipelines for deals.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CategoryListResult = {
      categories: Array<{
        id: number
        name: string
        sort: number
        entityTypeId: number
        isDefault: 'Y' | 'N'
        originId?: string
        originatorId?: string
      }>
    }

    try {
      // The method returns the list of pipelines available to the user
      const response = await $b24.actions.v2.call.make<CategoryListResult>({
        method: 'crm.category.list',
        params: {
          entityTypeId: 2,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Categories:', result.categories, 'Count:', result.categories.length)
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
      async function loadCategoryList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // The method returns the list of pipelines available to the user
          const response = await $b24.actions.v2.call.make({
            method: 'crm.category.list',
            params: {
              entityTypeId: 2,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Categories:', result.categories, 'Count:', result.categories.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', loadCategoryList)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.category.list(
            entity_type_id=2,
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.category.list',
                [
                    'entityTypeId' => 2,
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

    ```js
    BX24.callMethod(
        "crm.category.list",
        {
            entityTypeId: 2,
        },
        (result) => 
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.category.list',
        [
            'entityTypeId' => 2
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.category.list", b24.Params{
    	"entityTypeId": 2,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.category.list: %w", err)
    }

    // The method wraps the response in an object with the "categories" key.
    raw, ok := b24.Unwrap(res.Result, "categories")
    if !ok {
    	return fmt.Errorf("no categories key in the response")
    }

    var items []struct {
    	ID           b24.ID `json:"id"`
    	Name         string `json:"name"`
    	Sort         int    `json:"sort"`
    	EntityTypeID b24.ID `json:"entityTypeId"`
    	IsDefault    string `json:"isDefault"`
    	OriginID     string `json:"originId"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "categories": [
            {
                "id": 9,
                "name": "Pipeline with Original Name",
                "sort": 200,
                "entityTypeId": 2,
                "isDefault": "N",
                "originId": "",
                "originatorId": ""
            },
            {
                "id": 10,
                "name": "Lead Route",
                "sort": 200,
                "entityTypeId": 2,
                "isDefault": "N",
                "originId": "",
                "originatorId": ""
            },
            {
                "id": 11,
                "name": "Path to Success",
                "sort": 200,
                "entityTypeId": 2,
                "isDefault": "N",
                "originId": "",
                "originatorId": ""
            },
            {
                "id": 0,
                "name": "General",
                "sort": 300,
                "entityTypeId": 2,
                "isDefault": "Y"
            }
        ]
    },
    "total": 4,
    "time": {
        "start": 1718293410.373042,
        "finish": 1718293425.947633,
        "duration": 15.574590921401978,
        "processing": 15.16909408569336,
        "date_start": "2024-06-13T17:43:30+02:00",
        "date_finish": "2024-06-13T17:43:45+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | The root element of the response [(detailed description)](#result) ||
|| **total**
[`integer`](../../../data-types.md) | The number of pipelines available to the user that match the filter ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **categories**
[`array`](../../../data-types.md) | Array of pipelines. With `isCategoryMenu=false`, the pipeline fields are described in the [`category`](./crm-category-add.md#category) object. With `isCategoryMenu=true`, the menu fields described below are returned ||
|#

With `isCategoryMenu=false`, the `isSystem` and `code` fields are returned only for contacts and companies — `entityTypeId=3` and `entityTypeId=4`. For deals and SPAs, these fields are not included in the response. The `isSystem` field is a `boolean` with the values `Y` or `N`; the `code` field is the string code of the pipeline.

With `isCategoryMenu=true`, each pipeline contains only `id`, `name`, `sort`, `entityTypeId`, `isDefault`, and `counter`. The `counter` field of type `integer` is the current user's counter value in this pipeline.

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Smart Process Automation was not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {entityTypeId} | The required `entityTypeId` parameter is not provided ||
|| `400` | `NOT_FOUND` | Smart Process Automation was not found | The CRM object type with the specified `entityTypeId` was not found ||
|| `400` | `ENTITY_TYPE_NOT_SUPPORTED` | CRM entity "{entityTypeName}" is not supported | The CRM object does not support pipelines. `{entityTypeName}` is replaced with the name of the object type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [CRM Pipelines: Overview of Methods](./index.md)
- [{#T}](./crm-category-add.md)
- [{#T}](./crm-category-update.md)
- [{#T}](./crm-category-get.md)
- [{#T}](./crm-category-delete.md)
- [{#T}](./crm-category-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contractor.md)
- [{#T}](../../../../tutorials/crm/how-to-get-lists/how-to-get-contractors.md)
