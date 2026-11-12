# Update a Pipeline crm.category.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the permission to manage pipelines. For an SPA of a digital workplace, the permission is checked within that workplace

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.&lt;type&gt;.category.update](../../rest-v3/categories/crm-entity-category-update-rest-v3.md). Pipelines of invoices, CRM documents, and B2E documents are not available in REST 3.0 yet, so use this method for them.

{% endnote %}

The `crm.category.update` method updates the pipeline with the identifier `id`: it changes the values of the fields passed in `fields`. The values of other fields remain unchanged.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [system](../../index.md) or [user-defined type](../user-defined-object-types/index.md) of the CRM object for which the pipeline will be updated ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the pipeline. It can be obtained using the [`crm.category.list`](./crm-category-list.md) method or when creating a pipeline with the [`crm.category.add`](./crm-category-add.md) method ||
|| **fields***
[`object`](../../../data-types.md)  |  Field values (detailed description provided [below](#parametr-fields)) for updating the pipeline fields in the form of a structure:

```js
fields: {
    name: "Pipeline name",
    sort: 1000,
    isDefault: "Y",
},
```

  ||
|#

### Parameter fields {#parametr-fields}

For a full description of the pipeline fields, see [crm.category.fields](./crm-category-fields.md) and the [field table of crm.category.add](./crm-category-add.md#parametr-fields). Update specifics:

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Name of the pipeline ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index. Pass a non-negative number. A negative value is ignored, and the previous value is retained ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Default pipeline flag:
- `Y` or `true` — yes
- `N` or `false` — no

If changing the flag is available for the object type, pass `Y` or `true` to make the updated pipeline the default one. The previous default pipeline loses this flag.

In deals, the `isDefault` field is not available for modification.

To check whether the field can be changed for the object type, use the [`crm.category.fields`](./crm-category-fields.md) method: a read-only field has the property `isReadOnly = true` ||
|#

## Code Examples

How to update a pipeline with `id = 4` in an SPA with `entityTypeId = 1152`

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1152,"id":4,"fields":{"name":"New Pipeline Name","sort":1000,"isDefault":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1152,"id":4,"fields":{"name":"New Pipeline Name","sort":1000,"isDefault":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CategoryUpdateResult = {
      category: {
        id: number
        name: string
        sort: number
        entityTypeId: number
        isDefault: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<CategoryUpdateResult>({
        method: 'crm.category.update',
        params: {
          entityTypeId: 1152,
          id: 4,
          fields: {
            name: 'New pipeline name',
            sort: 1000,
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
      async function updateCategory() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.category.update',
            params: {
              entityTypeId: 1152,
              id: 4,
              fields: {
                name: 'New pipeline name',
                sort: 1000,
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

      document.addEventListener('DOMContentLoaded', updateCategory)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.category.update(
            bitrix_id=4,
            fields={
                "name": "New pipeline name",
                "sort": 1000,
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
                'crm.category.update',
                [
                    'entityTypeId' => 1152,
                    'id'          => 4,
                    'fields'      => [
                        'name'     => 'New Pipeline Name',
                        'sort'     => 1000,
                        'isDefault' => 'Y',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating category: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.category.update",
        {
            entityTypeId: 1152,
            id: 4,
            fields: {
                name: "New Pipeline Name",
                sort: 1000,
                isDefault: "Y",
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
        'crm.category.update',
        [
            'entityTypeId' => 1152,
            'id' => 4,
            'fields' => [
                'name' => "New Pipeline Name",
                'sort' => 1000,
                'isDefault' => "Y"
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
    res, err := client.Core().Call(ctx, "crm.category.update", b24.Params{
    	"entityTypeId": 1152,
    	"id":           4,
    	"fields": b24.Params{
    		"name":      "New Pipeline Name",
    		"sort":      1000,
    		"isDefault": "Y",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.category.update: %w", err)
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
            "id": 4,
            "name": "New Pipeline Name",
            "sort": 1000,
            "entityTypeId": 1152,
            "isDefault": "Y"
        }
    },
    "time": {
        "start": 1718359296.368324,
        "finish": 1718359296.65352,
        "duration": 0.28519606590270996,
        "processing": 0.03645014762878418,
        "date_start": "2024-06-14T12:01:36+02:00",
        "date_finish": "2024-06-14T12:01:36+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains the [`category`](#category) object ||
|| **category**
[`object`](../../../data-types.md) | Object of the updated pipeline within `result`. Its fields are described in the [Object category](#category) section ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

#### Object category {#category}

For a full description of the object, see [crm.category.add](./crm-category-add.md#category). Brief response structure:

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Pipeline identifier ||
|| **name**
[`string`](../../../data-types.md) | Name of the pipeline ||
|| **sort**
[`integer`](../../../data-types.md) | Sort index ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | CRM object type identifier ||
|| **isDefault**
[`boolean`](../../../data-types.md) | Default pipeline flag: `Y` — yes, `N` — no ||
|| **originId**, **originatorId**
[`string`](../../../data-types.md) | Identifiers for linking with an external system. Returned for additional deal pipelines ||
|| **isSystem**
[`boolean`](../../../data-types.md) | System pipeline flag: `Y` — yes, `N` — no. Returned for contacts and companies ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code of the pipeline. Returned for contacts and companies ||
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
|| `400` | `100` | Could not find value for parameter {fields} | A required parameter is missing. The name of the missing parameter is returned instead of `{fields}`: `entityTypeId`, `id`, or `fields` ||
|| `400` | `NOT_FOUND` | Smart Process Automation was not found | The CRM object type with the specified `entityTypeId` was not found ||
|| `400` | `ENTITY_TYPE_NOT_SUPPORTED` | CRM entity "{entityTypeName}" is not supported | The CRM object does not support pipelines. The name of the object type is returned instead of `{entityTypeName}` ||
|| `400` | `NOT_FOUND` | Item was not found | Occurs if the pipeline being updated does not exist ||
|| `400` | `ACCESS_DENIED` | Access denied | Occurs if the user updating the pipeline does not have sufficient rights ||
|#

The error text depends on the Bitrix24 language. The method also returns pipeline saving errors; their set depends on the CRM object type.

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-category-add.md)
- [{#T}](./crm-category-get.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-delete.md)
- [{#T}](./crm-category-fields.md)
