# Update Call List Composition crm.calllist.update

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with read access permissions for CRM entities

The method `crm.calllist.update` allows you to add or remove participants from an existing call list and update the associated CRM form.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **LIST_ID***
[`integer`](../../data-types.md) | Identifier of the call list ||
|| **ENTITY_TYPE***
[`string`](../../data-types.md) | Type of the entity: 
- `CONTACT` — contact,
- `COMPANY` — company ||
|| **ENTITIES***
[`array`](../../data-types.md) | Array of `ID`s of contacts or companies, which can be obtained using the method [crm.item.list](../universal/crm-item-list.md) ||
|| **WEBFORM_ID**
[`integer`](../../data-types.md) | `ID` of the CRM form that will be displayed in the call form. 
The `ID` can be found in the list of forms in Bitrix24 https://your-domain.com/crm/webform/ ||
|#

### Method Operation Features

The method overwrites the `ENTITIES` array. To add an element, include both current and new `ID`s in the request:

1. Current IDs: [1,2,3].
2. New IDs: [4].
3. Send: [1,2,3,4].

To remove an element, send only those `ID`s that should remain in the list:

4. Current IDs: [1,2,3].
5. Remove: [2].
6. Send: [1,3].

The method overwrites the `WEBFORM_ID` field. If the `WEBFORM_ID` field is not provided when calling the method, the field will be cleared.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.calllist.update",
        {
            LIST_ID: 123,
            ENTITY_TYPE: "CONTACT",
            ENTITIES: [1,2,3],
            WEBFORM_ID: 5
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"LIST_ID":123,"ENTITY_TYPE":"CONTACT","ENTITIES":[1,2,3],"WEBFORM_ID":5}' \
         https://**your_bitrix24**/rest/**user_id**/**webhook**/crm.calllist.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"LIST_ID":123,"ENTITY_TYPE":"CONTACT","ENTITIES":[1,2,3],"WEBFORM_ID":5,"auth":"**put_access_token_here**"}' \
         https://**your_bitrix24**/rest/crm.calllist.update
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.calllist.update',
        [
            'LIST_ID' => 123,
            'ENTITY_TYPE' => 'CONTACT',
            'ENTITIES' => [1,2,3],
            'WEBFORM_ID' => 5
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
        "start": 1752562914.533195,
        "finish": 1752562914.606445,
        "duration": 0.07325005531311035,
        "processing": 0.044027090072631836,
        "date_start": "2025-07-15T10:01:54+02:00",
        "date_finish": "2025-07-15T10:01:54+02:00",
        "operating_reset_at": 1752563514,
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
    "error": "Invalid parameters.",
    "error_description": "Invalid parameters were provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `Invalid parameters` | Invalid parameters were provided ||
|| `400` | `Incorrect entity type` | An unsupported entity type was specified ||
|| `400` | `Entities is not array` | The ENTITIES parameter is not an array ||
|| `400` | `Incorrect entities id` | Invalid IDs of entities were provided ||
|| `400` | `Incorrect list id or access denied` | Invalid list identifier or no access ||
|| `400` | `Discrepancy between the type of call participants and incoming type` | Mismatch between the type of participants and the provided type ||
|| `400` | `Incorrect webform id` | Invalid ID of the CRM form ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-calllist-add.md)
- [{#T}](./crm-calllist-get.md)
- [{#T}](./crm-calllist-items-get.md)
- [{#T}](./crm-calllist-list.md)
- [{#T}](./crm-calllist-statuslist.md)