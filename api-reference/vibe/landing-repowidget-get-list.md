# Get the list of widgets landing.repowidget.getlist

> Scope: [`landing`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.repowidget.getlist` returns a list of widgets for the current application, filtered by the specified criteria.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **params***
[`object`](../data-types.md) | Array of fields to retrieve the list of widgets ||
|#

### Parameter params

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../data-types.md) | Array [with the list of fields](#anchor-field) to be selected.

If not provided or an empty array is passed, all available widgets will be selected ||
|| **filter**
[`object`](../data-types.md) | Object for filtering the selected records in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` can be found [in the table below](#anchor-field).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - "mol%" — searching for values starting with "mol"
    - "%mol" — searching for values ending with "mol"
    - "%mol%" — searching for values where "mol" can be in any position

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides.

- `!=%` — NOT LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - "mol%" — searching for values not starting with "mol"
    - "%mol" — searching for values not ending with "mol"
    - "%mol%" — searching for values where the substring "mol" is not present in any position

- `!%=` — NOT LIKE (see description above)

- `=` — equals, exact match (used by default)
- `!=` - not equal
- `!` — not equal ||
|| **group**
[`array`](../data-types.md) | Array for grouping widgets. Grouping can be done [by fields](#anchor-field) of the widget ||
|| **order**
[`object`](../data-types.md) | Object for sorting the selected records in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` can be found [in the table below](#anchor-field).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||

|#

#### Fields field {#anchor-field}

Fields of the widget object. Present in the request and response.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../data-types.md) | Widget identifier ||
|| **XML_ID**
[`string`](../data-types.md) | Unique record code ||
|| **APP_CODE**
[`string`](../data-types.md) | Code of the current application ||
|| **ACTIVE**
[`char`](../data-types.md) | Widget activity. Accepts values: 

- `Y` - widget is active and available
- `N` - widget is inactive and unavailable ||
|| **NAME**
[`string`](../data-types.md) | Widget name ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Widget description ||
|| **SECTIONS**
[`string`](../data-types.md) | Code of the [section](./landing-repowidget-register.md#anchor-fields) where the widget will be added ||
|| **PREVIEW**
[`string`](../data-types.md) | URL of the widget cover image for the widget selection slider ||
|| **WIDGET_PARAMS**
[`object`](../data-types.md) | [Parameters](./landing-repowidget-register.md#anchor-widget-params) for the Vue template engine ||
|| **CONTENT**
[`string`](../data-types.md) | Widget markup using Vue constructs ||
|| **MANIFEST**
[`object`](../data-types.md) | Widget manifest ||
|| **CREATED_BY_ID**
[`integer`](../data-types.md) | Identifier of the user who created the record ||
|| **MODIFIED_BY_ID**
[`integer`](../data-types.md) | Identifier of the user who modified the record ||
|| **DATE_CREATE**
[`date`](../data-types.md) | Creation date ||
|| **DATE_MODIFY**
[`date`](../data-types.md) | Modification date ||
|| **SITE_TEMPLATE_ID**
[`string`](../data-types.md) | Binding of the widget to a specific site template. **Only for on-premise Bitrix24!** ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS


    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'landing.repowidget.getlist',
        {
          params: {
            select: [
              'ID', 'NAME'
            ],
            filter: {
              '>ID': '1'
            }
          }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('landing.repowidget.getlist', {
        params: {
          select: [
            'ID', 'NAME'
          ],
          filter: {
            '>ID': '1'
          }
        }
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('landing.repowidget.getlist', {
        params: {
          select: [
            'ID', 'NAME'
          ],
          filter: {
            '>ID': '1'
          }
        }
      }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.repowidget.getlist',
                [
                    'params' => [
                        'select' => [
                            'ID', 'NAME'
                        ],
                        'filter' => [
                            '>ID' => '1'
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting repowidget list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repowidget.getlist',
        {
            params: {
                select: [
                    'ID', 'NAME' 
                ],
                filter: {
                    '>ID': '1'
                }
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.repowidget.getList',
        [
            'params' => [
                'select' => ['ID', 'NAME'],
                'filter' => [
                    '>ID' => '1'
                ]
            ]
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
            "ID": "4",
            "XML_ID": "my_widget",
            "APP_CODE": "app.code",
            "ACTIVE": "Y",
            "NAME": "My widget",
            "DESCRIPTION": null,
            "SECTIONS": "widgets_company_life",
            "SITE_TEMPLATE_ID": null,
            "PREVIEW": "https://my-app.com/vibe_preview.jpg",
            "MANIFEST": {
                "block": {
                    "type": [
                        "mainpage"
                    ],
                    "subtype": [
                        "widgetvue"
                    ],
                    "subtype_params": {
                        "rootNode": ".w-container",
                        "demoData": {
                            "desc": "JustSome widget data",
                            "count": "420"
                        },
                        "handler": "https://my-app.com/vibe.php",
                        "style": "https://my-app.com/vibe.css",
                        "lang": {
                            "de": {
                                "W_TITLE": "People and their age",
                                "W_EMPTY": "No people"
                            },
                            "en": {
                                "W_TITLE": "Widget title",
                                "W_EMPTY": "Empty!"
                            }
                        }
                    }
                }
            },
            "CONTENT": "<div class=\"w-container\">not_var{{desc}}</div>",
            "CREATED_BY_ID": "1",
            "MODIFIED_BY_ID": "1",
            "DATE_CREATE": "10.10.2024 15:55:30",
            "DATE_MODIFY": "16.10.2024 16:12:57"
        }
    ],
    "time": {
        "start": 1729162340.81773,
        "finish": 1729162344.800994,
        "duration": 3.9832639694213867,
        "processing": 3.8990869522094727,
        "date_start": "2024-10-17T15:52:20+01:00",
        "date_finish": "2024-10-17T15:52:24+01:00",
        "operating": 3.899045944213867
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Array of widgets. Each element of the array is an object, permissible fields are described [above](#anchor-field). ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-repowidget-register.md)
- [{#T}](./landing-repowidget-unregister.md)
- [{#T}](./landing-repowidget-debug.md)

