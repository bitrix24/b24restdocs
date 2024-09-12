# List of Kanban Stages / My Plan task.stages.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.stages.get` retrieves the stages of the Kanban / My Plan.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **entityId^*^**
[`integer`](../../data-types.md) | Entity identifier. If it equals the `ID` of a group, the stages of the group's Kanban are returned. If the access level is insufficient, an access error is displayed. If the parameter equals `0`, the stages of the current user's My Plan are returned. ||
|| **isAdmin**
[`boolean`](../../data-types.md) | If set to `true`, permission checks will not occur, provided that the requester is an administrator of the account. ||
|#

Returns an array of stages, fields are described in the [stages table](./index.md).

## Examples

{% list tabs %}

- JS
    ```js
    const entityId = 0;
    BX24.callMethod(
        'task.stages.get',
        {
            entityId: entityId,
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- cURL (oAuth)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "entityId": 0
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.get
    ```

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "entityId": 0
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.get
    ```

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $entityId = 0;

    // executing the request to the REST API
    $result = CRest::call(
        'task.stages.get',
        [
            'entityId' => $entityId
        ]
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "5": {
         "ID": "5",
         "TITLE": "Not Planned",
         "SORT": "100",
         "COLOR": "00C4FB",
         "SYSTEM_TYPE": "NEW",
         "ENTITY_ID": "1",
         "ENTITY_TYPE": "U",
         "ADDITIONAL_FILTER": [],
         "TO_UPDATE": [],
         "TO_UPDATE_ACCESS": null
        },
        "6": {
         "ID": "6",
         "TITLE": "I will do it this week",
         "SORT": "200",
         "COLOR": "47D1E2",
         "SYSTEM_TYPE": null,
         "ENTITY_ID": "1",
         "ENTITY_TYPE": "U",
         "ADDITIONAL_FILTER": [],
         "TO_UPDATE": [],
         "TO_UPDATE_ACCESS": null
        }
    }
}
```

## Returned Data

#|
|| **Field** `type` | **Description** ||
|| **result** `object` | An object containing data about the Kanban / My Plan stages, with stage identifiers as keys ||
|| **ID** `integer` | Stage identifier ||
|| **TITLE** `string` | Title ||
|| **SORT** `integer` | Sorting ||
|| **COLOR** `string` | Color ||
|| **SYSTEM_TYPE** `string` | System type (e.g., "NEW", "PROGRESS", "WORK", "REVIEW", "FINISH") ||
|| **ENTITY_ID** `integer` | Entity identifier (group or user) ||
|| **ENTITY_TYPE** `string` | Entity type (e.g., "U" for user, "G" for group) ||
|| **ADDITIONAL_FILTER** `array` | Additional filters (system parameter, always has an empty array value) ||
|| **TO_UPDATE** `array` | Array of elements to update (system parameter, always has an empty array value) ||
|| **TO_UPDATE_ACCESS** `null` | Functions applied to the task when moving to this stage (system parameter, always has a null value) ||
|#

## Error Handling

HTTP Status: **200**

```json
{
"error": "ACCESS_DENIED",
"error_description": "You cannot view stages in this group"
}
```

### Possible Error Codes

#|
|| **Code** | **Value** ||
|| `ACCESS_DENIED` | You cannot view stages in this group ||
|#