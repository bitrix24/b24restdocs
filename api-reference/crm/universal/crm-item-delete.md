# Delete CRM Item - crm.item.delete

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with the "delete" access permission for CRM object items

This method deletes a CRM object item by its item ID and item type ID.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | The ID of the [system](./index.md) or [user-defined type](./user-defined-object-types/index.md) whose item we want to delete ||
|| **id***
[`integer`][1] | The ID of the item to be deleted.

This can be obtained using the [`crm.item.list`](./crm-item-list.md) method or when creating an item with [`crm.item.add`](./crm-item-add.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Deleting an item with `id = 1`, belonging to a smart process with `entityTypeId = 1268`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1268,"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1268,"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.item.delete',
    		{
    			entityTypeId: 1268,
    			id: 1,
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
                'crm.item.delete',
                [
                    'entityTypeId' => 1268,
                    'id'           => 1,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            return;
        }
    
        echo 'Success: ' . print_r($result->data(), true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
        BX24.callMethod(
            'crm.item.delete',
            {
                entityTypeId: 1268,
                id: 1,
            },
            (result) => {
                if (result.error())
                {
                    console.error(result.error());

                    return;
                }

                console.info(result.data());
            },
        );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.delete',
        [
            'entityTypeId' => 1268,
            'id' => 1
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
    "result": [],
    "time": {
        "start": 1721657688.755373,
        "finish": 1721657689.65017,
        "duration": 0.8947970867156982,
        "processing": 0.6092040538787842,
        "date_start": "2024-07-22T16:14:48+02:00",
        "date_finish": "2024-07-22T16:14:49+02:00",
        "operating": 0
    }
}
```

### Returned Values

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`][1] | The root element of the response.

Returns an empty array `[]` in case of success ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Item not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code**                          | **Description**                                     | **Value**                                                      ||
|| `allowed_only_intranet_user`     | Action allowed only for intranet users            | User is not an intranet user                                   ||
|| `NOT_FOUND`                      | Smart process not found                             | Occurs when an invalid `entityTypeId` is passed               ||
|| `NOT_FOUND`                      | Item not found                                     | An item with the given `id` of type `entityTypeId` does not exist ||
|| `ACCESS_DENIED`                  | Access denied                                       | User does not have permission to delete items of type `entityTypeId` ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}


## Continue Learning

- [{#T}](crm-item-add.md)
- [{#T}](crm-item-update.md)
- [{#T}](crm-item-get.md)
- [{#T}](crm-item-list.md)
- [{#T}](crm-item-fields.md)
- [{#T}](./object-fields.md)

[1]: ./../data-types.md