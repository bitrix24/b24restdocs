# Add Log Entry crm.timeline.logmessage.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `user with permission to modify the CRM entity to which the entry will be added`

This method adds a new log entry to the timeline.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for adding a new log entry in the form of a structure:

```js
fields:
{
    entityTypeId: "value",
    entityId: "value",
    title: "value",
    text: "value",
    iconCode: "value",
},
```
 ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | [Identifier of the entity type](../../data-types.md#object_type) in which the entry will be created ||
|| **entityId***
[`integer`](../../../data-types.md) | Identifier of the entity item in which the entry will be created ||
|| **title***
[`string`](../../../data-types.md) | Entry title ||
|| **text***
[`string`](../../../data-types.md) | Entry text ||
|| **iconCode***
[`string`](../../../data-types.md) | Icon code.

A list of available codes can be obtained using the method [crm.timeline.icon.list](./icons/crm-timeline-icon-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityTypeId":1,"entityId":1,"title":"Test title","text":"Test text message","iconCode":"info"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.logmessage.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"entityTypeId":1,"entityId":1,"title":"Test title","text":"Test text message","iconCode":"info"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.logmessage.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.timeline.logmessage.add",
        {
            fields: {
                entityTypeId: 1,
                entityId: 1,
                title: "Test title",
                text: "Test text message",
                iconCode: "info",
            },
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.logmessage.add',
        [
            'fields' => [
                'entityTypeId' => 1,
                'entityId' => 1,
                'title' => 'Test title',
                'text' => 'Test text message',
                'iconCode' => 'info',
            ]
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
    "result": {
        "logMessage": {
            "id": 1,
            "created": "2024-04-03T10:26:32+02:00",
            "authorId": 1,
            "title": "Test title",
            "text": "Test note",
            "iconCode": "info"
        }
    },
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response.

The `result` field contains the [logMessage](#logMessage) object ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

#### logMessage Object {#logMessage}

#|
|| **Name**
`type` | **Description**  ||
|| **id** 
[`integer`](../../../data-types.md)| Identifier of the timeline entry ||
|| **created** 
[`datetime`](../../../data-types.md)| Date and time of creation ||
|| **authorId** 
[`integer`](../../../data-types.md)| User who created the entry ||
|| **title**
[`string`](../../../data-types.md)| Entry title ||
|| **text** 
[`string`](../../../data-types.md)| Entry content ||
|| **iconCode** 
[`string`](../../../data-types.md)| Icon code ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Access denied ||
|| `OWNER_NOT_FOUND` | The CRM entity with the specified `entityTypeId` and `entityId` does not exist ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal) ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-timeline-logmessage-get.md)
- [{#T}](./crm-timeline-logmessage-list.md)
- [{#T}](./crm-timeline-logmessage-delete.md)