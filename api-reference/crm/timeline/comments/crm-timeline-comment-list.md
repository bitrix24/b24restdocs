# Get a List of Comments crm.timeline.comment.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method retrieves a list of all comments of the specified CRM entity type.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array containing the list of fields to be selected (see the fields of the [result](./crm-timeline-comment-fields.md#fields) object). If not provided or an empty array is passed, the result will return an empty array ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected comments in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [result](./crm-timeline-comment-fields.md#fields) object.

Required fields: `ENTITY_ID`, `ENTITY_TYPE`.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:

- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
  - `"mol%"` — looking for values starting with "mol"
  - `"%mol"` — looking for values ending with "mol"
  - `"%mol%"` — looking for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
  - `"mol%"` — looking for values not starting with "mol"
  - `"%mol"` — looking for values not ending with "mol"
  - `"%mol%"` — looking for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected comments in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [result](./crm-timeline-comment-fields.md#fields) object.

Possible values for `order`:

- `ASC` — in ascending order
- `DESC` — in descending order
 ||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page
 ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ENTITY_ID":10,"ENTITY_TYPE":"deal"},"select":["ID","CREATED","ENTITY_ID","ENTITY_TYPE","AUTHOR_ID","COMMENT","FILES"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.comment.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ENTITY_ID":10,"ENTITY_TYPE":"deal"},"select":["ID","CREATED","ENTITY_ID","ENTITY_TYPE","AUTHOR_ID","COMMENT","FILES"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.comment.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.timeline.comment.list',
        {
          filter: {
            "ENTITY_ID": 10,
            "ENTITY_TYPE": "deal",
          },
          select: [
            "ID",
            "CREATED",
            "ENTITY_ID",
            "ENTITY_TYPE",
            "AUTHOR_ID",
            "COMMENT", 
            "FILES",
          ],
        },
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative fetching using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.timeline.comment.list', {
        filter: {
          "ENTITY_ID": 10,
          "ENTITY_TYPE": "deal",
        },
        select: [
          "ID",
          "CREATED",
          "ENTITY_ID",
          "ENTITY_TYPE",
          "AUTHOR_ID",
          "COMMENT", 
          "FILES",
        ],
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.timeline.comment.list', {
        filter: {
          "ENTITY_ID": 10,
          "ENTITY_TYPE": "deal",
        },
        select: [
          "ID",
          "CREATED",
          "ENTITY_ID",
          "ENTITY_TYPE",
          "AUTHOR_ID",
          "COMMENT", 
          "FILES",
        ],
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.timeline.comment.list',
                [
                    'filter' => [
                        'ENTITY_ID'   => 10,
                        'ENTITY_TYPE' => 'deal',
                    ],
                    'select' => [
                        'ID',
                        'CREATED',
                        'ENTITY_ID',
                        'ENTITY_TYPE',
                        'AUTHOR_ID',
                        'COMMENT',
                        'FILES',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching timeline comments: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.comment.list",
        {
            filter: {
                "ENTITY_ID": 10,
                "ENTITY_TYPE": "deal",
            },
            select: [
                "ID",
                "CREATED",
                "ENTITY_ID",
                "ENTITY_TYPE",
                "AUTHOR_ID",
                "COMMENT", 
                "FILES",
            ],
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.comment.list',
        [
            'filter' => [
                'ENTITY_ID' => 10,
                'ENTITY_TYPE' => 'deal',
            ],
            'select' => [
                'ID',
                'CREATED',
                'ENTITY_ID',
                'ENTITY_TYPE',
                'AUTHOR_ID',
                'COMMENT',
                'FILES',
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
    "result": [
        {
            "ID": "999",
            "ENTITY_ID": "2",
            "ENTITY_TYPE": "deal",
            "CREATED": "2020-03-02T12:00:00+02:00",
            "COMMENT": "New comment was added",
            "AUTHOR_ID": "1",
            "FILES": {
                "1": {
                    "id": 1,
                    "date": "2020-03-02T12:00:00+02:00",
                    "type": "image",
                    "name": "1.gif",
                    "size": 43,
                    "image": {
                        "width": 1,
                        "height": 1
                    },
                    "authorId": 1,
                    "authorName": "John Dou",
                    "urlPreview": "https://my.bitrix24.com/disk/showFile/930/?&ncc=1&width=640&height=640&signature=292f450929833cd881070155e05a2c41b5bb265ea8c8c1bc2108dbcbb56f667f&ts=1718366521&filename=1.gif",
                    "urlShow": "https://my.bitrix24.com/disk/showFile/930/?&ncc=1&ts=1718366521&filename=1.gif",
                    "urlDownload": "https://my.bitrix24.com/disk/downloadFile/930/?&ncc=1&filename=1.gif"
                },
                "2": {
                    "id": 2,
                    "date": "2020-03-02T12:00:00+02:00",
                    "type": "image",
                    "name": "2.gif",
                    "size": 43,
                    "image": {
                        "width": 1,
                        "height": 1
                    },
                    "authorId": 1,
                    "authorName": "John Dou",
                    "urlPreview": "https://my.bitrix24.com/disk/showFile/931/?&ncc=1&width=640&height=640&signature=118de010a40eff06fb9d691ee9235e2ef809a17780e46927bf8b12f8dc3224db&ts=1718366521&filename=2.gif",
                    "urlShow": "https://my.bitrix24.com/disk/showFile/931/?&ncc=1&ts=1718366521&filename=2.gif",
                    "urlDownload": "https://my.bitrix24.com/disk/downloadFile/931/?&ncc=1&filename=2.gif"
                }
            },
        },
        {
            "ID": "1000",
            "ENTITY_ID": "2",
            "ENTITY_TYPE": "deal",
            "CREATED": "2020-03-02T12:00:00+02:00",
            "COMMENT": "Test comment",
            "AUTHOR_ID": "1",
            "FILES": {},
        }
    ],
    "total": 2,
    "time": {
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-03T17:19:01+02:00",
        "date_finish": "2024-05-03T17:19:01+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | The root element of the response, containing an array of objects with information about the selected comments ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | Access denied. | No rights to edit the entity in CRM ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-comment-add.md)
- [{#T}](./crm-timeline-comment-update.md)
- [{#T}](./crm-timeline-comment-get.md)
- [{#T}](./crm-timeline-comment-delete.md)
- [{#T}](./crm-timeline-comment-fields.md)