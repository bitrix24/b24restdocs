# Delete Sales Funnel crm.category.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with administrative access to the CRM section

This method deletes a funnel (direction) with the identifier `id`.

{% note warning "Cannot delete:" %}

* Default funnels
* Funnels that have at least one element

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](./../../index.md) or [user-defined type](./../user-defined-object-types/index.md) of the CRM entity from which the funnel will be deleted   ||
|| **id***
[`integer`][1] | Identifier of the funnel to be deleted. Can be obtained using the [`crm.category.list`](./crm-category-list.md) method or when creating a funnel with the [`crm.category.add`](./crm-category-add.md) method ||
|#

## Code Examples

Delete the funnel with `id = 5`, located in deals.

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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.category.delete',
    		{
    			entityTypeId: 2,
    			id: 5,
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
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
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting category: ' . $e->getMessage();
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
[`null`][1] | Root element of the response, equal to `null` ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **160**, **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Element not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `NOT_FOUND` | SPA not found | Occurs with incorrect values for `entityTypeId` ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | Entity type `{entityTypeName}` is not supported | Occurs if the CRM object does not support funnels ||
|| `NOT_FOUND` | Element not found | Occurs if the funnel to be deleted does not exist ||
|| `REMOVING_DISABLED` | Deletion of system category is prohibited | Occurs when attempting to delete a system funnel ||
|| `ACCESS_DENIED` | Access denied | Occurs if the user deleting the funnel does not have sufficient rights ||
|| `0` | Deals related to the funnel `{categoryName}` found | Occurs if the funnel to be deleted has at least one element. Relevant only for `deals` ||
|| `0` | Default deal category cannot be deleted | Occurs when attempting to delete a default funnel. Relevant only for `deals` ||
|| `BX_ERROR` | Elements related to the funnel found | Occurs if the funnel to be deleted has at least one element. Relevant for `SPAs` ||
|| `BX_ERROR` | This funnel is the default funnel | Occurs when attempting to delete a default funnel. Relevant for `SPAs` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-category-add.md)
- [{#T}](./crm-category-update.md)
- [{#T}](./crm-category-get.md)
- [{#T}](./crm-category-list.md)
- [{#T}](./crm-category-fields.md)

[1]: ../../../data-types.md