# Create a New Call List crm.calllist.add

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with read access permissions for CRM entities

The method `crm.calllist.add` creates a new call list.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE***
[`string`](../../data-types.md) | Type of the object: 
- `CONTACT` — contact,
- `COMPANY` — company ||
|| **ENTITIES***
[`array`](../../data-types.md) | Array of `ID`s of contacts or companies, which can be obtained using the method [crm.item.list](../universal/crm-item-list.md) ||
|| **WEBFORM_ID**
[`integer`](../../data-types.md) | `ID` of the CRM form that will be displayed in the call list form. 
The `ID` can be found in the list of forms on Bitrix24 https://your-domain.com/crm/webform/ ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.calllist.add",
        {
            ENTITY_TYPE: "CONTACT",
            ENTITIES: [9,17,19],
            WEBFORM_ID: 1
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
         -d '{"ENTITY_TYPE":"CONTACT","ENTITIES":[1,2,3],"WEBFORM_ID":5}' \
         https://**your_bitrix24**/rest/**user_id**/**webhook**/crm.calllist.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"ENTITY_TYPE":"CONTACT","ENTITIES":[1,2,3],"WEBFORM_ID":5,"auth":"**put_access_token_here**"}' \
         https://**your_bitrix24**/rest/crm.calllist.add
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.calllist.add',
        [
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
    "result": 11,
    "time": {
        "start": 1752471668.062547,
        "finish": 1752471668.531711,
        "duration": 0.4691641330718994,
        "processing": 0.4174520969390869,
        "date_start": "2025-07-14T08:41:08+02:00",
        "date_finish": "2025-07-14T08:41:08+02:00",
        "operating_reset_at": 1752472268,
        "operating": 0.41742897033691406
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | ID of the created call list ||
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
|| `400` | `Access denied` | No permission to perform the operation ||
|| `400` | `Invalid parameters` | Invalid parameters were provided ||
|| `400` | `Incorrect entity type` | An unsupported object type was specified ||
|| `400` | `Entities is not array` | The `ENTITIES` parameter is not an array ||
|| `400` | `Incorrect entities id` | Invalid `ID`s of elements were provided ||
|| `403` | `You don't have access to these entities` | No access to the specified elements ||
|| `400` | `Incorrect webform id` | Invalid `ID` of the CRM form ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-calllist-get.md)
- [{#T}](./crm-calllist-items-get.md)
- [{#T}](./crm-calllist-list.md)
- [{#T}](./crm-calllist-statuslist.md)
- [{#T}](./crm-calllist-update.md)