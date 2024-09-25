# Get a list of log entries from crm.timeline.logmessage.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method retrieves a list of timeline log entries.

{% note info "" %}

It's important to note that the method can only retrieve data for entries that were previously added using [`crm.timeline.logmessage.add`](./crm-timeline-logmessage-add.md). System entries cannot be retrieved using `crm.timeline.logmessage.list`.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | [Identifier of the entity type](../../data-types.md#object_type) for which to retrieve the list of log entries (for example, `1` — lead) ||
|| **entityId***
[`integer`](../../../data-types.md) | Identifier of the entity item for which to retrieve the list of log entries (for example, `1`) ||
|| **order**
[`object`](../../../data-types.md) | List for sorting, where the key is the field and the value is `asc` or `desc`.

By default, `desc` is used.

Sorting is only supported by the fields **id** and **created** ||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static: 10 entries.

To select the second page of results, you need to pass the value `10`. To select the third page of results — the value `20`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 10`, where `N` is the desired page number ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"entityId":1,"order":{"created":"desc"},"start":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.logmessage.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"entityId":1,"order":{"created":"desc"},"start":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.logmessage.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.timeline.logmessage.list",
        {
            entityTypeId: 1,
            entityId: 1,
            order: { created: "desc" },
            start: 1,
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
        'crm.timeline.logmessage.list',
        [
            'entityTypeId' => 1,
            'entityId' => 1,
            'order' => ['created' => 'desc'],
            'start' => 1
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
    "result": [
        {
            "id": 42074,
            "created": "2024-04-03T10:26:32+02:00",
            "authorId": 1,
            "title": "Test title new",
            "text": "Test note new",
            "iconCode": "info"
        },
        {
            "id": 42073,
            "created": "2024-04-03T10:26:32+02:00",
            "authorId": 1,
            "title": "Test title",
            "text": "Test note",
            "iconCode": "info"
        }
    ],
    "total": 2,
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
[`array`](../../../data-types.md) | The root element of the response.

The `result` field contains an array, each entry of which contains an associative array of fields for the log entry [logMessage](./crm-timeline-logmessage-add.md#logMessage) ||
|| **total**
[`integer`](../../../data-types.md) | The total number of found entries ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {entityTypeId}"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `100` | Required fields are missing ||
|| `0` | Other errors (e.g., fatal) ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-timeline-logmessage-add.md)
- [{#T}](./crm-timeline-logmessage-get.md)
- [{#T}](./crm-timeline-logmessage-delete.md)