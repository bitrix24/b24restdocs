# Delete Custom Field crm.deal.userfield.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

The method `crm.deal.userfield.delete` removes a custom field from deals.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the custom field associated with the deal.

The identifier can be obtained using the methods [crm.deal.userfield.add](./crm-deal-userfield-add.md) or [crm.deal.userfield.list](./crm-deal-userfield-list.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":432}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/crm.deal.userfield.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":432,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.userfield.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.userfield.delete',
    		{
    			id: 432,
    		}
    	);
    	
    	const result = response.getData().result;
    	result.error()
    		? console.error(result.error())
    		: console.info(result)
    	;
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $userfieldId = 123; // Replace with the actual userfield ID you want to delete
        $result = $serviceBuilder
            ->getCRMScope()
            ->dealUserfield()
            ->delete($userfieldId);

        if ($result->isSuccess()) {
            print("Userfield deleted successfully.");
        } else {
            print("Failed to delete userfield.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.userfield.delete',
        {
            id: 432,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.userfield.delete',
        [
            'id' => 432
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
    "result": true,
    "time": {
        "start": 1724419843.518672,
        "finish": 1724419844.120328,
        "duration": 0.6016559600830078,
        "processing": 0.1907808780670166,
        "date_start": "2024-08-23T15:30:43+02:00",
        "date_finish": "2024-08-23T15:30:44+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `ID is not defined or invalid` | The provided `id` is either less than or equal to zero, or not provided at all ||
|| `403` | `Access denied` | Occurs when:
- the user does not have administrative rights
- the user attempts to delete a custom field not associated with deals ||
|| `ERROR_NOT_FOUND` | `The entity with ID 'id' is not found` | The custom field with the provided `id` does not exist ||
|| `400` | Error deleting `FIELD_NAME` for object `ENTITY_ID` | Unknown error during deletion ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-userfield-add.md)
- [{#T}](./crm-deal-userfield-get.md)
- [{#T}](./crm-deal-userfield-list.md)
- [{#T}](./crm-deal-userfield-update.md)