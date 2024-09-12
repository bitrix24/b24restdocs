# Delete Bank Detail crm.requisite.bankdetail.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method deletes a bank detail.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the bank detail. Identifiers of bank details can be obtained using the [`crm.requisite.bankdetail.list`](./crm-requisite-bank-detail-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":357}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.bankdetail.delete
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":357,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.bankdetail.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.bankdetail.delete",
        { id: 357 },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.bankdetail.delete',
        ['id' => 357]
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
        "start": 1716554949.294631,
        "finish": 1716554949.795059,
        "duration": 0.5004279613494873,
        "processing": 0.057311058044433594,
        "date_start": "2024-05-24T14:49:09+02:00",
        "date_finish": "2024-05-24T14:49:09+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of deleting the bank detail:
- true — deleted
- false — not deleted
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The RequisiteBankDetail with ID '357' is not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Error Message** | **Description** ||
|| `ID is not defined or invalid` | The identifier of the bank detail is not specified or has an invalid value ||
|| `The RequisiteBankDetail with ID '357' is not found` | The bank detail with the specified identifier was not found ||
|| `Access denied` | Insufficient access permissions to delete the bank detail ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-bank-detail-add.md)
- [{#T}](./crm-requisite-bank-detail-update.md)
- [{#T}](./crm-requisite-bank-detail-get.md)
- [{#T}](./crm-requisite-bank-detail-list.md)
- [{#T}](./crm-requisite-bank-detail-fields.md)