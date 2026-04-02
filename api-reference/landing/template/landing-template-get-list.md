# Get a List of View Templates landing.template.getlist

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access permission to the "Sites and Stores" section

The method `landing.template.getlist` retrieves a list of view templates for the current account based on the selection parameters.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **params**
[`object`](../../data-types.md) | An object containing the selection parameters for view templates in the following format:

```json
{
    "select": ["field_1", "field_2"],
    "filter": {
        "field": "value"
    },
    "order": {
        "field": "ASC"
    },
    "group": ["field"],
    "limit": 50,
    "offset": 0
}
```

The list of available fields is described [below](#params) ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | A list of fields from the [fields of the View Template object](./fields.md).

If the parameter is not provided, the method returns all available fields of the template.

Fields like `CREATED_BY.NAME` are not supported ||
|| **filter**
[`object`](../../data-types.md) | A filter for fields from the [fields of the View Template object](./fields.md).

If the parameter is not provided, the method returns all templates of the current account, including inactive ones.

Fields like `CREATED_BY.NAME` are not supported ||
|| **order**
[`object`](../../data-types.md) | Sorting in the format `{"FIELD": "ASC"}` or `{"FIELD": "DESC"}`.

Multiple fields can be provided. If the parameter is not provided, the method does not apply any sorting ||
|| **group**
[`array`](../../data-types.md) | An array of field names for grouping, such as `["ACTIVE"]` or `["ACTIVE", "SORT"]`.

Calculated fields via `runtime` are not supported ||
|| **limit**
[`integer`](../../data-types.md) | The maximum number of items in the response.

If `limit` is not specified, the method returns all found templates without restriction. Use together with `offset` for pagination ||
|| **offset**
[`integer`](../../data-types.md) | The offset from the start of the selection. Use together with `limit` for pagination ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "params": {
          "select": [
            "ID",
            "TITLE",
            "XML_ID",
            "SORT",
            "ACTIVE",
            "DATE_MODIFY"
          ],
          "filter": {
            "=ACTIVE": "Y"
          },
          "order": {
            "SORT": "ASC",
            "ID": "ASC"
          },
          "limit": 2,
          "offset": 0
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.template.getlist.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "params": {
          "select": [
            "ID",
            "TITLE",
            "XML_ID",
            "SORT",
            "ACTIVE",
            "DATE_MODIFY"
          ],
          "filter": {
            "=ACTIVE": "Y"
          },
          "order": {
            "SORT": "ASC",
            "ID": "ASC"
          },
          "limit": 2,
          "offset": 0
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.template.getlist.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.template.getlist',
            {
                params: {
                    select: [
                        'ID',
                        'TITLE',
                        'XML_ID',
                        'SORT',
                        'ACTIVE',
                        'DATE_MODIFY'
                    ],
                    filter: {
                        '=ACTIVE': 'Y'
                    },
                    order: {
                        SORT: 'ASC',
                        ID: 'ASC'
                    },
                    limit: 2,
                    offset: 0
                }
            }
        );

        const result = response.getData().result;
        console.info(result);
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
                'landing.template.getlist',
                [
                    'params' => [
                        'select' => [
                            'ID',
                            'TITLE',
                            'XML_ID',
                            'SORT',
                            'ACTIVE',
                            'DATE_MODIFY',
                        ],
                        'filter' => [
                            '=ACTIVE' => 'Y',
                        ],
                        'order' => [
                            'SORT' => 'ASC',
                            'ID' => 'ASC',
                        ],
                        'limit' => 2,
                        'offset' => 0,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting template list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.template.getlist',
        {
            params: {
                select: [
                    'ID',
                    'TITLE',
                    'XML_ID',
                    'SORT',
                    'ACTIVE',
                    'DATE_MODIFY'
                ],
                filter: {
                    '=ACTIVE': 'Y'
                },
                order: {
                    SORT: 'ASC',
                    ID: 'ASC'
                },
                limit: 2,
                offset: 0
            }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.template.getlist',
        [
            'params' => [
                'select' => [
                    'ID',
                    'TITLE',
                    'XML_ID',
                    'SORT',
                    'ACTIVE',
                    'DATE_MODIFY',
                ],
                'filter' => [
                    '=ACTIVE' => 'Y',
                ],
                'order' => [
                    'SORT' => 'ASC',
                    'ID' => 'ASC',
                ],
                'limit' => 2,
                'offset' => 0,
            ],
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "1",
            "TITLE": "With Header and Footer",
            "XML_ID": "header_footer",
            "SORT": "100",
            "ACTIVE": "Y",
            "DATE_MODIFY": "10/10/2022 03:25:30 pm"
        },
        {
            "ID": "2",
            "TITLE": "With Left Sidebar",
            "XML_ID": "sidebar_left",
            "SORT": "200",
            "ACTIVE": "Y",
            "DATE_MODIFY": "10/10/2022 03:25:30 pm"
        }
    ],
    "time": {
        "start": 1774765200,
        "finish": 1774765200.411258,
        "duration": 0.4112579822540283,
        "processing": 0,
        "date_start": "2026-03-29T10:00:00+02:00",
        "date_finish": "2026-03-29T10:00:00+02:00",
        "operating_reset_at": 1774765800,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../data-types.md) | A list of view templates [(detailed description)](#template).

If nothing is found by the filter, the method returns `result: []` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Object template {#template}

#|
|| **Name**
`type` | **Description** ||
|| **FIELD**
[`string`](../../data-types.md) \| [`integer`](../../data-types.md) \| `null` | A field of the view template from the [fields of the View Template object](./fields.md), if it was included in the selection.

For the fields `DATE_CREATE` and `DATE_MODIFY`, the method always returns a string, for example `04/20/2020 12:48:10 pm` ||
|#

Response Features:

- The `TITLE` field is usually returned as the name in the current language of the account
- If a translation for a system template is not found, the `TITLE` may return a language key like `#...#`, for example `#HEADER_ONLY#`

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Insufficient permissions."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions to work with the "Sites and Stores" section ||
|| `TYPE_ERROR` | Data type error while processing the call parameters ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-template-get-landing-ref.md)
- [{#T}](./landing-template-get-site-ref.md)
- [{#T}](./landing-template-set-landing-ref.md)
- [{#T}](./landing-template-set-site-ref.md)
- [Fields of the View Template object](./fields.md)