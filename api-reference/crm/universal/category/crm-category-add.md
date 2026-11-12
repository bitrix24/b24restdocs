# Add a New Pipeline crm.category.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the permission to manage pipelines. For an SPA of a digital workplace, the permission is checked within that workplace

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.&lt;type&gt;.category.add](../../rest-v3/categories/crm-entity-category-add-rest-v3.md). Pipelines of invoices, CRM documents, and B2E documents are not available in REST 3.0 yet, so use this method for them.

{% endnote %}

The `crm.category.add` method creates a new pipeline for the CRM object type with the identifier `entityTypeId`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId*** 
[`integer`](../../../data-types.md) | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) of the CRM object for which the new pipeline will be created ||
|| **fields***
[`object`](../../../data-types.md)  | Field values (detailed description provided [below](#parametr-fields)) for adding a new pipeline in the form of a structure:

```js
fields: {
    name: "New pipeline",
    sort: 50,
    isDefault: "N",
},
```

 ||
|#

### Parameter fields {#parametr-fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Pipeline name:
- no longer than `255` characters
- not empty
- not consisting solely of whitespace characters ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index. A negative value is ignored.

For a deal pipeline, if `sort` is not passed or is negative, `0` is used. For contact, company, and SPA pipelines, `500` is used in these cases ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Default pipeline flag. Can have values:
- `Y` or `true` — yes
- `N` or `false` — no

Defaults to `N`.

In deals, the `isDefault` field is ignored. A default pipeline cannot be created for contacts and companies: the value `Y` or `true` causes an error.

For an SPA, when a pipeline is created with `isDefault: "Y"`, the previous default pipeline loses this flag. Restrictions on changing and deleting are described in [crm.category.update](./crm-category-update.md#parametr-fields) and [crm.category.delete](./crm-category-delete.md) ||
|#

## Code Examples

Create a new default pipeline in the SPA with `entityTypeId = 1152`.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId": 1152, "fields": {"name": "New Default Pipeline", "sort": 50, "isDefault": "Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId": 1152, "fields": {"name": "New Default Pipeline", "sort": 50, "isDefault": "Y"}, "auth": "**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CategoryAddResult = {
      category: {
        id: number
        name: string
        sort: number
        entityTypeId: number
        isDefault: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<CategoryAddResult>({
        method: 'crm.category.add',
        params: {
          entityTypeId: 1152,
          fields: {
            name: 'New default pipeline',
            sort: 50,
            isDefault: 'Y',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.category.id, result.category.name)
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
      async function addCategory() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.category.add',
            params: {
              entityTypeId: 1152,
              fields: {
                name: 'New default pipeline',
                sort: 50,
                isDefault: 'Y',
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
          console.info(result.category.id, result.category.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCategory)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.category.add(
            fields={
                "name": "New default pipeline",
                "sort": 50,
                "isDefault": "Y",
            },
            entity_type_id=1152,
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
                'crm.category.add',
                [
                    'entityTypeId' => 1152,
                    'fields' => [
                        'name'     => 'New Default Pipeline',
                        'sort'     => 50,
                        'isDefault' => 'Y',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding category: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.category.add",
        {
            entityTypeId: 1152,
            fields: {
                name: "New Default Pipeline",
                sort: 50,
                isDefault: 'Y',
            },
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
        'crm.category.add',
        [
            'entityTypeId' => 1152,
            'fields' => [
                'name' => "New Default Pipeline",
                'sort' => 50,
                'isDefault' => 'Y',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.category.add", b24.Params{
    	"entityTypeId": 1152,
    	"fields": b24.Params{
    		"name":      "New Default Pipeline",
    		"sort":      50,
    		"isDefault": "Y",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.category.add: %w", err)
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
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "category": {
            "id": 5,
            "name": "New Default Pipeline",
            "sort": 50,
            "entityTypeId": 1152,
            "isDefault": "Y"
        }
    },
    "time": {
        "start": 1718116794.208887,
        "finish": 1718116794.666272,
        "duration": 0.4573848247528076,
        "processing": 0.1496260166168213,
        "date_start": "2024-06-11T16:39:54+02:00",
        "date_finish": "2024-06-11T16:39:54+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains the [`category`](#category) object with information about the pipeline ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object category {#category}

#| 
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the pipeline ||
|| **name**
[`string`](../../../data-types.md) | Name of the pipeline ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) to which the pipeline belongs ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Default pipeline flag: `Y` — yes, `N` — no ||
|| **originId**
[`string`](../../../data-types.md) | Identifier of the item in the data source.

Exists only in deals ||
|| **originatorId**
[`string`](../../../data-types.md) | Identifier of the data source.

Exists only in deals ||
|| **isSystem** 
[`boolean`](../../../data-types.md) | Indicates whether the pipeline is a system pipeline.

The field is present for contact and company pipelines ||
|| **code**
[`string`](../../../data-types.md) | Alias for system pipelines.

The field is present for contact and company pipelines ||
|#

## Error Handling

HTTP Status: **400**

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
|| `400` | `NOT_FOUND` | Smart Process Automation was not found | The CRM object type with the specified `entityTypeId` was not found ||
|| `400` | `ENTITY_TYPE_NOT_SUPPORTED` | CRM entity "{entityTypeName}" is not supported | The CRM object does not support pipelines. `{entityTypeName}` is replaced with the name of the object type ||
|| `400` | `ADDING_DISABLED` | Cannot add a system pipeline | Occurs when `fields.isSystem = "Y"` is passed: creating system pipelines is prohibited ||
|| `400` | `ACCESS_DENIED` | Access denied | Occurs if the user does not have sufficient rights to add a pipeline ||
|| `400` | `0` | Field 'NAME' is required. | Occurs if the `name` field is not passed or is empty when creating a deal pipeline ||
|| `400` | `160` | Internal server error. | Occurs when attempting to create a default pipeline for contacts or companies: the common pipeline does not support changing the `isDefault` flag ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [CRM Pipelines: Overview of Methods](./index.md)
- [{#T}](./crm-category-update.md)
- [{#T}](./crm-category-get.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-delete.md)
- [{#T}](./crm-category-fields.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)
