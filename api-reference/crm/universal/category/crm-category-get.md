# Get a Pipeline by ID crm.category.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "Read" access permission to the pipeline

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.&lt;type&gt;.category.get](../../rest-v3/categories/crm-entity-category-get-rest-v3.md). Pipelines of invoices, CRM documents, and B2E documents are not available in REST 3.0 yet, so use this method for them.

{% endnote %}

The method `crm.category.get` retrieves information about the pipeline with the identifier `id`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) of the CRM object for which you need to get the pipeline. The object type must support pipelines: [list of supported objects](./index.md#which-objects-support-pipelines) ||
|| **id***
[`integer`](../../../data-types.md) | Pipeline identifier. For the general pipeline of deals, contacts, or companies, specify `0`. The identifier can be obtained using the [`crm.category.list`](./crm-category-list.md) method or when creating a pipeline using the [`crm.category.add`](./crm-category-add.md) method ||
|#

## Code Examples

How to get information about the pipeline with `id` = `1`, located in deals.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CategoryGetResult = {
      category: {
        id: number
        name: string
        sort: number
        entityTypeId: number
        isDefault: string
        originId?: string | null
        originatorId?: string | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<CategoryGetResult>({
        method: 'crm.category.get',
        params: {
          entityTypeId: 2,
          id: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.category)
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
      async function getCategoryById() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.category.get',
            params: {
              entityTypeId: 2,
              id: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.category)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCategoryById)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.category.get(
            bitrix_id=1,
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
                'crm.category.get',
                [
                    'entityTypeId' => 2,
                    'id'          => 1,
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
        "crm.category.get",
        {
            entityTypeId: 2,
            id: 1,
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
        'crm.category.get',
        [
            'entityTypeId' => 2,
            'id' => 1
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.category.get", b24.Params{
    	"entityTypeId": 2,
    	"id":           1,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.category.get: %w", err)
    }

    // The method wraps the response in an object with the "category" key.
    raw, ok := b24.Unwrap(res.Result, "category")
    if !ok {
    	return fmt.Errorf("no category key in the response")
    }

    var item struct {
    	ID           b24.ID `json:"id"`
    	Name         string `json:"name"`
    	Sort         int    `json:"sort"`
    	EntityTypeID b24.ID `json:"entityTypeId"`
    	IsDefault    string `json:"isDefault"`
    	OriginID     string `json:"originId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "category": {
            "id": 1,
            "name": "New pipeline #1",
            "sort": 200,
            "entityTypeId": 2,
            "isDefault": "N",
            "originId": "",
            "originatorId": ""
        }
    },
    "time": {
        "start": 1718291429.253404,
        "finish": 1718291429.654617,
        "duration": 0.4012131690979004,
        "processing": 0.025265216827392578,
        "date_start": "2024-06-13T17:10:29+02:00",
        "date_finish": "2024-06-13T17:10:29+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains the `category` object with information about the pipeline ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

The `result.category` object contains the main fields:

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Pipeline identifier. For the general pipeline of deals, it may be `0` ||
|| **name**
[`string`](../../../data-types.md) | Pipeline name ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Identifier of the CRM object type to which the pipeline belongs ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Default pipeline flag: `Y` — yes, `N` — no ||
|#

Other fields depend on the object type and the pipeline. The `originId` and `originatorId` fields are returned for deal pipelines but are absent for the general pipeline with `id = 0`. The `isSystem` and `code` fields are returned for contacts and companies; they are absent in responses for deals and SPAs. For a detailed description of the fields, see the [category object table](./crm-category-add.md#category); for the fields available for an object type, see the [crm.category.fields](./crm-category-fields.md) method.

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Item was not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {id} | The required parameter `id` is not provided ||
|| `400` | `100` | Could not find value for parameter {entityTypeId} | The required parameter `entityTypeId` is not provided ||
|| `400` | `100` | Invalid value {abc} to match with parameter {id}. Should be value of type int. | A value of the wrong type is provided. Example: `id=abc` instead of an integer ||
|| `400` | `NOT_FOUND` | Smart Process Automation was not found | The CRM object type with the specified `entityTypeId` was not found ||
|| `400` | `ENTITY_TYPE_NOT_SUPPORTED` | CRM entity "{entityTypeName}" is not supported | The CRM object does not support pipelines. The name of the object type is returned instead of `{entityTypeName}` ||
|| `400` | `NOT_FOUND` | Item was not found | A pipeline with the specified `entityTypeId` and `id` does not exist ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to view elements of this pipeline ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [CRM Pipelines: Overview of Methods](./index.md)
- [{#T}](./crm-category-add.md)
- [{#T}](./crm-category-update.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-delete.md)
- [{#T}](./crm-category-fields.md)
