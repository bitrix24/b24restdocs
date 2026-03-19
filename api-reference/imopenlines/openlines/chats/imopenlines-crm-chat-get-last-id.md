# Get the Id of the Last Chat imopenlines.crm.chat.getLastId

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the CRM entity

The method `imopenlines.crm.chat.getLastId` retrieves the identifier of the last chat associated with the CRM entity.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CRM_ENTITY_TYPE***
[`string`](../../../data-types.md) | Type of the CRM entity. Possible values:
- `lead` — lead
- `deal` — deal
- `company` — company
- `contact` — contact ||
|| **CRM_ENTITY***
[`integer`](../../../data-types.md) | Identifier of the CRM entity.

You can obtain the identifier using the universal method [getting a list of CRM entities](../../../crm/universal/crm-item-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1205}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.crm.chat.getLastId
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CRM_ENTITY_TYPE":"lead","CRM_ENTITY":1205,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imopenlines.crm.chat.getLastId
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.crm.chat.getLastId',
            {
                CRM_ENTITY_TYPE: 'lead',
                CRM_ENTITY: 1205
            }
        );

        const result = response.getData().result;
        console.log(result);
    }
    catch (error)
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
                'imopenlines.crm.chat.getLastId',
                [
                    'CRM_ENTITY_TYPE' => 'lead',
                    'CRM_ENTITY' => 1205,
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
        'imopenlines.crm.chat.getLastId',
        {
            CRM_ENTITY_TYPE: 'lead',
            CRM_ENTITY: 1205
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imopenlines.crm.chat.getLastId',
        [
            'CRM_ENTITY_TYPE' => 'lead',
            'CRM_ENTITY' => 1205,
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 1763,
    "time": {
        "start": 1773758808,
        "finish": 1773758808.520651,
        "duration": 0.52065110206604,
        "processing": 0,
        "date_start": "2026-03-17T17:46:48+02:00",
        "date_finish": "2026-03-17T17:46:48+02:00",
        "operating_reset_at": 1773759408,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the last chat ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CRM_CHAT_EMPTY_CRM_DATA",
    "error_description": "Empty CRM data"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CRM_CHAT_EMPTY_CRM_DATA` | Empty CRM data | Required parameters `CRM_ENTITY_TYPE` and `CRM_ENTITY` are not provided ||
|| `400` | `CRM_CHAT_EMPTY_CRM_DATA` | Could not find CRM entity | Possible reasons:
- No chat found for the specified CRM entity
- Invalid `CRM_ENTITY_TYPE` specified
- Non-existent `CRM_ENTITY` specified
- User does not have access to the CRM entity ||
|| `400` | `ERROR_ARGUMENT` | The value of an argument CRM_ENTITY has an invalid type | The `CRM_ENTITY` parameter is provided in an incorrect format ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-crm-chat-get.md)
- [{#T}](./imopenlines-crm-chat-user-add.md)
- [{#T}](./imopenlines-crm-chat-user-delete.md)