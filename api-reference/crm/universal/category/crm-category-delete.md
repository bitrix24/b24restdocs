# Delete a Pipeline crm.category.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the permission to manage pipelines. For an SPA of a digital workplace, the permission is checked within that workplace

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.&lt;type&gt;.category.delete](../../rest-v3/categories/crm-entity-category-delete-rest-v3.md). Pipelines of invoices, CRM documents, and B2E documents are not available in REST 3.0 yet, so use this method for them.

{% endnote %}

The method `crm.category.delete` deletes a pipeline with the identifier `id`.

{% note warning "Deletion restrictions" %}

You cannot delete a system pipeline or a pipeline that contains items. In deals, you also cannot delete the default pipeline. In an SPA, the default pipeline cannot be deleted if this object type has at least one item.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [system](./../../index.md) or [user-defined type](./../user-defined-object-types/index.md) of the CRM object for which the pipeline will be deleted. The object type must support pipelines: [list of supported objects](./index.md#which-objects-support-pipelines) ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the pipeline to be deleted. Can be obtained using the [`crm.category.list`](./crm-category-list.md) method or when creating a pipeline with the [`crm.category.add`](./crm-category-add.md) method ||
|#

## Code Examples

Delete the pipeline with `id = 5`, located in deals.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"id":5}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.category.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":2,"id":5,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.category.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<null>({
        method: 'crm.category.delete',
        params: {
          entityTypeId: 2,
          id: 5,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Category deleted successfully', result)
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
      async function deleteCategory() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.category.delete',
            params: {
              entityTypeId: 2,
              id: 5,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Category deleted successfully', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteCategory)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.category.delete(
            bitrix_id=5,
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
                'crm.category.delete',
                [
                    'entityTypeId' => 2,
                    'id'          => 5,
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

    ```javascript
    BX24.callMethod(
        "crm.category.delete",
        {
            entityTypeId: 2,
            id: 5,
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
        'crm.category.delete',
        [
            'entityTypeId' => 2,
            'id' => 5
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.category.delete", b24.Params{
    	"entityTypeId": 2,
    	"id":           5,
    })
    if err != nil {
    	return fmt.Errorf("crm.category.delete: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": null,
    "time": {
        "start": 1718288046.047386,
        "finish": 1718288046.892514,
        "duration": 0.845128059387207,
        "processing": 0.4207921028137207,
        "date_start": "2024-06-13T16:14:06+02:00",
        "date_finish": "2024-06-13T16:14:06+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`null`](../../../data-types.md) | Root element of the response, equal to `null` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

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
|| `400` | `ENTITY_TYPE_NOT_SUPPORTED` | CRM entity "{entityTypeName}" is not supported | The CRM object does not support pipelines. The object type name is returned instead of `{entityTypeName}` ||
|| `400` | `NOT_FOUND` | Item was not found | Occurs if the pipeline to be deleted does not exist ||
|| `400` | `REMOVING_DISABLED` | Cannot delete a system pipeline | Occurs when attempting to delete a system pipeline ||
|| `400` | `ACCESS_DENIED` | Access denied | Occurs if the user does not have the permission to manage pipelines in the corresponding CRM or digital workplace ||
|| `400` | `0` | Found "{categoryName}" pipeline deals | Occurs if the deal pipeline to be deleted contains at least one deal. The pipeline name is returned instead of `{categoryName}` ||
|| `400` | `BX_ERROR` | There are items in this pipeline | Occurs if the SPA pipeline to be deleted contains at least one item ||
|| `400` | `BX_ERROR` | This is a default pipeline | Occurs when attempting to delete the default SPA pipeline if this object type has at least one item ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [CRM Pipelines: Overview of Methods](./index.md)
- [{#T}](./crm-category-add.md)
- [{#T}](./crm-category-update.md)
- [{#T}](./crm-category-get.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-fields.md)
- [How to Create a New Pipeline with Stages in an SPA](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)
