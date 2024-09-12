# Add a Kanban Stage / My Plan task.stages.add

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success
  
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.stages.add` adds a Kanban stage / My Plan.

## Method Parameters

#|
|| **Name** `type` | **Description** ||
|| **fields^*^**
[`object`](../../data-types.md) | Field values (detailed description provided below) for adding a stage ||
|| **isAdmin**
[`boolean`](../../data-types.md) | If set to `true`, permission checks will not occur, provided the requester is an account administrator. ||
|#

## Parameter fields

#|
|| **Name** `type` | **Description** ||
|| **TITLE^*^** [`string`](../../data-types.md) | Title of the stage. ||
|| **COLOR** [`string`](../../data-types.md) | Color of the stage. ||
|| **AFTER_ID** [`integer`](../../data-types.md) | Identifier of the stage after which the new stage should be added. If not specified or equal to `0`, it will be added at the beginning. ||
|| **ENTITY_ID** [`integer`](../../data-types.md)| Identifier of the entity. It can equal the `ID` of the group, in which case the stage will be added to the group's Kanban. If the permission level is insufficient, an access error will be displayed. If it equals `0` or is absent, the stage will be added to the current user's My Plan. ||
|#

## Code Examples

{% list tabs %}

- JS
    ```js
    BX24.callMethod(
        'task.stages.add',
        {
            fields: {
                TITLE: 'Stage Title',
                COLOR: '#FFAAEE',
                AFTER_ID: 1,
                ENTITY_ID: 1
            },
            isAdmin: false,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- cURL (oAuth)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "fields": {
        "TITLE": "Stage Title",
        "COLOR": "#FFAAEE",
        "AFTER_ID": 1,
        "ENTITY_ID": 1
    },
    "isAdmin": false
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.add
    ```

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "fields": {
        "TITLE": "Stage Title",
        "COLOR": "#FFAAEE",
        "AFTER_ID": 1,
        "ENTITY_ID": 1
    },
    "isAdmin": false
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.add
    ```

- PHP
    ```php
    require_once('crest.php'); // include CRest PHP SDK

    $fields = [
        "TITLE" => "Stage Title",
        "COLOR" => "#FFAAEE",
        "AFTER_ID" => 1,
        "ENTITY_ID" => 1
    ];

    // execute request to REST API
    $result = CRest::call(
        'task.stages.add',
        [
            'fields' => $fields,
            'isAdmin' => false
        ]
    );

    // Handle response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 1
}
```

### Returned Data

#|
|| **Name** `type` | **Description** ||
|| **result** [`integer`](../../data-types.md) | Identifier of the added stage. ||
|#

## Error Handling

```json
{
    "error": "EMPTY_TITLE",
    "error_description": "Stage title is not specified"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `EMPTY_TITLE` | Stage title is not specified ||
|| `ACCESS_DENIED` | You cannot manage stages ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}