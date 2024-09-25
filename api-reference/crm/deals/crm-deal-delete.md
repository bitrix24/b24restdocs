# Delete deal crm.deal.delete

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "delete" access permission for deals

The method `crm.deal.delete` removes a deal and all associated objects.

Deleting a deal will result in the removal of all related objects, such as CRM activities, history, Timeline activities, and others.

Objects are deleted if they are not linked to other entities or elements. If the objects are linked to other entities, only the link to the deleted deal will be removed.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the deal.

The identifier can be obtained using the methods [crm.deal.list](./crm-deal-list.md) or [crm.deal.add](./crm-deal-add.md) ||
|#

{% note tip "Related methods and topics" %}

[{#T}](./recurring-deals/crm-deal-recurring-delete.md)

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":12}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":12,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.deal.delete',
        {
            id: 12,
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.delete',
        [
            'ID' => 12
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
        "start": 1725019350.037109,
        "finish": 1725019355.903999,
        "duration": 5.86689019203186,
        "processing": 5.262096881866455,
        "date_start": "2024-08-30T14:02:30+02:00",
        "date_finish": "2024-08-30T14:02:35+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Description** | **Value** ||
|| `ID is not defined or invalid` | The `id` parameter is either not provided or is not a positive integer ||
|| `Access denied` | The user does not have permission to "delete" deals ||
|| `Not found` | A deal with the provided `id` does not exist ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-deal-add.md)
- [{#T}](./crm-deal-update.md)
- [{#T}](./crm-deal-get.md)
- [{#T}](./crm-deal-list.md)
- [{#T}](./crm-deal-fields.md)