# Get Payer Type by Id sale.persontype.get

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method is used to access the fields of the payer type by its `Id`.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`Type` | **Description** ||
|| **id***
[`sale_person_type.id`](../data-types.md) | Payer type number ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":id}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.persontype.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":id,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.persontype.get
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.persontype.get', 
        { id: id }, 
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.persontype.get',
        ['id' => $id]
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
    "result": {
        "personType": {
            "active": "Y",
            "code": null,
            "id": 37,
            "name": "Individual",
            "sort": "1233",
            "xmlId": null
        }
    },
    "time": {
        "start": 1712325939.41209,
        "finish": 1712325939.60039,
        "duration": 0.188301086425781,
        "processing": 0.00575995445251465,
        "date_start": "2024-04-05T16:05:39+02:00",
        "date_finish": "2024-04-05T16:05:39+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **personType**
[`sale_person_type`](../data-types.md) | Object with information about the updated payer type ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200740400001` | Payer type with the specified `id` does not exist ||
|| `200040300010` | No access to read ||
|| `100` | Parameter `id` is not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-person-type-add.md)
- [{#T}](./sale-person-type-update.md)
- [{#T}](./sale-person-type-list.md)
- [{#T}](./sale-person-type-delete.md)
- [{#T}](./sale-person-type-get-fields.md)