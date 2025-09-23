# Get the list of activities crm.activity.list

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.list` returns a list of activities based on the filter, considering the access permissions of the current user.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array of fields of the activity [crm.activity.fields](./crm-activity-fields.md) that need to be selected. To get the fields `COMMUNICATIONS` and `FILES`, specify them in select.
||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected items in key-value format.

Possible values for `field` correspond to the fields of the activity [crm.activity.fields](./crm-activity-fields.md).

An additional prefix can be assigned to the key to clarify the filter's behavior. Possible prefix values:

- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as a value)
- `!@`— NOT IN (an array is passed as a value)
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for a substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
  - "mol%" — searching for values starting with "mol"
  - "%mol" — searching for values ending with "mol"
  - "%mol%" — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.
- `=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
  - "mol%" — searching for values not starting with "mol"
  - "%mol" — searching for values not ending with "mol"
  - "%mol%" — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
- `=` — equal, exact match (used by default)
- `!=` - not equal
- `!` — not equal
||
|| **order**
[`object`](../../../data-types.md) | A set of key-value pairs for sorting the output results. The keys can use the fields of the activity [crm.activity.fields](./crm-activity-fields.md).

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order

By default, it is sorted by increasing the Start Date field (`START_TIME`)
||
|| **start**
  [`integer`](../../../data-types.md) | This parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

See the description of [list methods](../../../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

{% note info "" %}

Pay attention to the peculiarity of the parameter `filter[BINDINGS]`.

Activity can be linked to multiple CRM entities. For example, a call can be linked to both a lead and an activity, so to retrieve these entities, there is a special filter key in the parameters of the method `crm.activity.list` - `BINDINGS`.

You need to specify an array of [system](../../../index.md) or [custom](../../../universal/user-defined-object-types/index.md) types of CRM objects for which you need to find the binding.

Each object can consist of the keys `OWNER_TYPE_ID` (entity type identifier) and `OWNER_ID` (entity identifier), either one or a combination of both. For example:

```json
"BINDINGS": [
    {"OWNER_TYPE_ID": 2},
    {"OWNER_TYPE_ID": 3}
]
```

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"DESC"},"filter":{"OWNER_TYPE_ID":3,"OWNER_ID":102},"select":["*","COMMUNICATIONS"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"DESC"},"filter":{"OWNER_TYPE_ID":3,"OWNER_ID":102},"select":["*","COMMUNICATIONS"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.list
    ```

- JS

    In this example, we retrieve the list of activities for the contact with `ID` = 102.

    ```js
    BX24.callMethod(
        "crm.activity.list",
        {
            order: { "ID": "DESC" },
            filter:
            {
                "OWNER_TYPE_ID": 3,
                "OWNER_ID": 102
            },
            select: [ "*", "COMMUNICATIONS" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.list',
        [
            'order' => [ 'ID' => 'DESC' ],
            'filter' => [
                'OWNER_TYPE_ID' => 3,
                'OWNER_ID' => 102
            ],
            'select' => [ '*', 'COMMUNICATIONS' ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](./crm-activity-list.md#example-bindings)
- [{#T}](./crm-activity-list.md#example-communications)
- [{#T}](./crm-activity-list.md#example-files)

{% endnote %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "20",
            "OWNER_ID": "15",
            "OWNER_TYPE_ID": "3",
            "TYPE_ID": "2",
            "PROVIDER_ID": "VOXIMPLANT_CALL",
            "PROVIDER_TYPE_ID": "CALL",
            "PROVIDER_GROUP_ID": null,
            "ASSOCIATED_ENTITY_ID": "0",
            "SUBJECT": "Outgoing call Andrew Nikolaev",
            "CREATED": "2020-09-27T13:26:55+03:00",
            "LAST_UPDATED": "2021-03-21T20:28:24+03:00",
            "START_TIME": "2020-09-27T13:25:00+03:00",
            "END_TIME": "2020-09-27T19:25:00+03:00",
            "DEADLINE": "2020-09-27T13:25:00+03:00",
            "COMPLETED": "Y",
            "STATUS": "2",
            "RESPONSIBLE_ID": "505",
            "PRIORITY": "2",
            "NOTIFY_TYPE": "1",
            "NOTIFY_VALUE": "15",
            "DESCRIPTION": "",
            "DESCRIPTION_TYPE": "1",
            "DIRECTION": "2",
            "LOCATION": "",
            "SETTINGS": [],
            "ORIGINATOR_ID": null,
            "ORIGIN_ID": null,
            "AUTHOR_ID": "505",
            "EDITOR_ID": "505",
            "PROVIDER_PARAMS": [],
            "PROVIDER_DATA": null,
            "RESULT_MARK": "0",
            "RESULT_VALUE": null,
            "RESULT_SUM": null,
            "RESULT_CURRENCY_ID": null,
            "RESULT_STATUS": "0",
            "RESULT_STREAM": "0",
            "RESULT_SOURCE_ID": null,
            "AUTOCOMPLETE_RULE": "0"
        },
        // .. 49 more items
    ],
    "next": 50,
    "total": 123456,
    "time": {
        "start": 1724677896.295857,
        "finish": 1724677897.197243,
        "duration": 0.901386022567749,
        "processing": 0.8762130737304688,
        "date_start": "2024-08-26T16:11:36+03:00",
        "date_finish": "2024-08-26T16:11:37+03:00",
        "operating_reset_at": "2024-08-26T16:11:37+03:00",
        "operating": 0.0162130737304688
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | The result of the operation. An array of activitys. For information about the structure of an activity, see the method [crm.activity.fields](./crm-activity-fields.md) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "INVALID_REQUEST",
    "error_description": "Https required"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Private Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

### Using BINDINGS {#example-bindings}

Retrieve fields: Identifier, Name, Owner Type (Entity Type Identifier), Owner (Entity Identifier)

Selection condition: the activity is linked to both a deal and a contact

{% note info %}

When using multiple pairs in `BINDINGS`, duplication may occur in the results. For example, in the result of executing the code example below, the activity linked to both entities will be output twice.

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"DESC"},"filter":{"BINDINGS":[{"OWNER_TYPE_ID":2},{"OWNER_TYPE_ID":3}]},"select":["*","COMMUNICATIONS"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ID":"DESC"},"filter":{"BINDINGS":[{"OWNER_TYPE_ID":2},{"OWNER_TYPE_ID":3}]},"select":["*","COMMUNICATIONS"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.list
    ```

- JS

    ```js
    BX24.callMethod(
            "crm.activity.list",
            {
                order: { "ID": "DESC" },
                filter:
                {
                    "BINDINGS": [
                        {"OWNER_TYPE_ID": 2},
                        {"OWNER_TYPE_ID": 3}
                    ]
                },
                select: [ "*", "COMMUNICATIONS" ]
            },
            function(result)
            {
                if(result.error())
                    console.error(result.error());
                else
                {
                    console.dir(result.data());
                    if(result.more())
                        result.next();
                }
            }
        );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.list',
        [
            'order' => ['ID' => 'DESC'],
            'filter' => [
                'BINDINGS' => [
                    ['OWNER_TYPE_ID' => 2],
                    ['OWNER_TYPE_ID' => 3]
                ]
            ],
            'select' => ['*', 'COMMUNICATIONS']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Retrieving COMMUNICATIONS {#example-communications}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ID":"20"},"select":["*","COMMUNICATIONS"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ID":"20"},"select":["*","COMMUNICATIONS"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.list
    ```

- JS

    ```js
    BX24.callMethod(
            "crm.activity.list",
            {
                filter:
                {
                    "ID": '20'
                },
                select: [ "*", "COMMUNICATIONS" ]
            },
            function(result)
            {
                if(result.error())
                    console.error(result.error());
                else
                {
                    console.dir(result.data());
                    if(result.more())
                        result.next();
                }
            }
        );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.list',
        [
            'filter' => [
                'ID' => '20'
            ],
            'select' => ['*', 'COMMUNICATIONS']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

#### Example of Returned Data

HTTP status: **200**

```json
{
    "result": [
        {
        "ID": "20",
        "COMMUNICATIONS": [
            {
                "ID": "23",
                "TYPE": "PHONE",
                "VALUE": "19152222222",
                "ENTITY_ID": "15",
                "ENTITY_TYPE_ID": "3",
                "ENTITY_SETTINGS": {
                    "HONORIFIC": "1",
                    "NAME": "Andrew ",
                    "SECOND_NAME": "Nikolaev",
                    "LAST_NAME": "",
                    "COMPANY_TITLE": "Ltd. Fusion",
                    "COMPANY_ID": "21"
                }
            }
        ]
    }
    ],
    "total": 1,
    "time": {
        "start": 1724659407.69855,
        "finish": 1724659407.723506,
        "duration": 0.02495598793029785,
        "processing": 0.003489971160888672,
        "date_start": "2024-08-26T11:03:27+03:00",
        "date_finish": "2024-08-26T11:03:27+03:00"
    }
}
```

### Retrieving Attachments {#example-files}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ID":"101121"},"select":["*","STORAGE_ELEMENT_IDS"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":{"ID":"101121"},"select":["*","STORAGE_ELEMENT_IDS"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.list
    ```

- JS

    ```js
    BX24.callMethod(
            "crm.activity.list",
            {
                filter:
                {
                    "ID": '101121'
                },
                select: [ "*", "STORAGE_ELEMENT_IDS" ]
            },
            function(result)
            {
                if(result.error())
                    console.error(result.error());
                else
                {
                    console.dir(result.data());
                    if(result.more())
                        result.next();
                }
            }
        );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.list',
        [
            'filter' => [
                'ID' => '101121'
            ],
            'select' => ['*', 'STORAGE_ELEMENT_IDS']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

#### Example of Returned Data

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "101121",
            "FILES": [
                {
                    "id": 3101820,
                    "url": "http://xxx.bitrix24.com/bitrix/tools/crm_show_file.php?fileId=3101820&ownerTypeId=6&ownerId=101121&auth="
                }
            ]
        }
    ],
    "total": 1,
    "time": {
        "start": 1724659652.591025,
        "finish": 1724659652.623784,
        "duration": 0.03275895118713379,
        "processing": 0.00624394416809082,
        "date_start": "2024-08-26T11:07:32+03:00",
        "date_finish": "2024-08-26T11:07:32+03:00"
    }
}
```

## Continue Learning 

- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity-between-objects.md)
- [{#T}](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity.md)
- [{#T}](../../../../../tutorials/crm/how-to-get-lists/get-activity-list-by-deals.md)