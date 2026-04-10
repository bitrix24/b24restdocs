# Get a List of Pages landing.landing.getList

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" access permission for the site

The method `landing.landing.getList` retrieves a list of pages based on the selection parameters.

{% note warning %}

By default, the method returns only pages on non-deleted sites with `DELETED = "N"`. To retrieve deleted pages, pass `DELETED` or `=DELETED` in the filter. This only works for pages on non-deleted sites: the method does not return pages from deleted sites.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of the landings. It is not related to the REST scope `landing` in the method name.

For regular pages, this parameter is not needed. For `GROUP`, `KNOWLEDGE`, and `MAINPAGE`, pass the corresponding `scope`. More about selecting values can be found in the article [Working with Site Types and Scopes](../../types.md) ||
|| **params**
[`object`](../../../data-types.md) | Parameters for selecting pages [(detailed description)](#params) ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../../data-types.md) | List of fields to select from [Page Object Fields](../fields.md). If the parameter is not passed or is equal to `null`, `["*"]` is used.

The method only accepts simple field names of the page. Elements with `.` are ignored. ||
|| **filter**
[`object`](../../../data-types.md) | Filter by fields from [Page Object Fields](../fields.md). If the parameter is not passed or is in an incorrect format, the selection is performed without custom conditions. Keys with `.` and `CHECK_PERMISSIONS` are ignored.

If `SITE_ID` is passed in the filter, the method additionally excludes pages that are in folders of that site marked as deleted.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) ||
|| **order**
[`object`](../../../data-types.md) | Sorting in the format `{"FIELD": "ASC"}` or `{"FIELD": "DESC"}`. If the parameter is not passed, no special sorting is applied. ||
|| **group**
[`array`](../../../data-types.md) | Grouping in ORM format. The method does not have a default value. ||
|| **limit**
[`integer`](../../../data-types.md) | Limit on the number of rows in the selection at the ORM level. The method does not set its own default limit. ||
|| **offset**
[`integer`](../../../data-types.md) | Offset for the selection at the ORM level. ||
|| **get_preview**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If the value evaluates to `true`, each result element includes a `PREVIEW` field with a link to the page preview. ||
|| **get_urls**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If the value evaluates to `true`, each result element includes a `PUBLIC_URL` field with the public address of the page. ||
|| **check_area**
[`boolean`](../../../data-types.md) \| [`integer`](../../../data-types.md) | If the value evaluates to `true`, each result element includes an `IS_AREA` field indicating whether the page is an included area. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

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
            "SITE_ID",
            "DATE_MODIFY"
          ],
          "filter": {
            "SITE_ID": 205,
            "=DELETED": "N"
          },
          "order": {
            "ID": "DESC"
          },
          "get_urls": true,
          "get_preview": true,
          "check_area": true
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.getList.json"
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
            "SITE_ID",
            "DATE_MODIFY"
          ],
          "filter": {
            "SITE_ID": 205,
            "=DELETED": "N"
          },
          "order": {
            "ID": "DESC"
          },
          "get_urls": true,
          "get_preview": true,
          "check_area": true
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.getList.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.getList',
    		{
    			params: {
    				select: [
    					'ID',
    					'TITLE',
    					'SITE_ID',
    					'DATE_MODIFY'
    				],
    				filter: {
    					SITE_ID: 205,
    					'=DELETED': 'N'
    				},
    				order: {
    					ID: 'DESC'
    				},
    				get_urls: true,
    				get_preview: true,
    				check_area: true
    			}
    		}
    	);

    	const result = response.getData();
    	console.info(result.result);
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
                'landing.landing.getList',
                [
                    'params' => [
                        'select' => [
                            'ID',
                            'TITLE',
                            'SITE_ID',
                            'DATE_MODIFY',
                        ],
                        'filter' => [
                            'SITE_ID' => 205,
                            '=DELETED' => 'N',
                        ],
                        'order' => [
                            'ID' => 'DESC',
                        ],
                        'get_urls' => true,
                        'get_preview' => true,
                        'check_area' => true,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting landing list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.getList',
        {
            params: {
                select: [
                    'ID',
                    'TITLE',
                    'SITE_ID',
                    'DATE_MODIFY'
                ],
                filter: {
                    SITE_ID: 205,
                    '=DELETED': 'N'
                },
                order: {
                    ID: 'DESC'
                },
                get_urls: true,
                get_preview: true,
                check_area: true
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
        'landing.landing.getList',
        [
            'params' => [
                'select' => [
                    'ID',
                    'TITLE',
                    'SITE_ID',
                    'DATE_MODIFY',
                ],
                'filter' => [
                    'SITE_ID' => 205,
                    '=DELETED' => 'N',
                ],
                'order' => [
                    'ID' => 'DESC',
                ],
                'get_urls' => true,
                'get_preview' => true,
                'check_area' => true,
            ],
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "985",
            "TITLE": "Detailed News",
            "SITE_ID": "3",
            "DATE_MODIFY": "10/10/2022 03:25:30 pm",
            "DOMAIN_ID": "5"
        },
        {
            "ID": "573",
            "TITLE": "Page Video",
            "SITE_ID": "3",
            "DATE_MODIFY": "10/10/2022 03:25:30 pm",
            "DOMAIN_ID": "5"
        }
    ],
    "time": {
        "start": 1773712560,
        "finish": 1773712560.955928,
        "duration": 0.9559280872344971,
        "processing": 0,
        "date_start": "2026-03-17T04:56:00+01:00",
        "date_finish": "2026-03-17T04:56:00+01:00",
        "operating_reset_at": 1773713160,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | List of pages [(detailed description)](#page). The method may return `result: []` without an error if there are no matching pages for the filter or if the user does not have "view" access permission for these sites. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

#### Object page {#page}

#|
|| **Name**
`type` | **Description** ||
|| **FIELD**
[`string`](../../../data-types.md) \| `null` | Any field of the page from [Page Object Fields](../fields.md), if it is requested in `params.select` or if `params.select` is not passed. ||
|| **DOMAIN_ID**
[`string`](../../../data-types.md) \| `null` | Identifier of the domain of the site to which the page is linked. Present in the response even if it is not specified in `params.select`. ||
|| **PUBLIC_URL**
[`string`](../../../data-types.md) \| `null` | Public address of the page. Returned only if the `get_urls` flag is enabled. ||
|| **PREVIEW**
[`string`](../../../data-types.md) \| `null` | Link to the page preview. Returned only if the `get_preview` flag is enabled. ||
|| **IS_AREA**
[`boolean`](../../../data-types.md) | Indicates whether the page is used as an included area. Returned only if the `check_area` flag is enabled. ||
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
|| `ACCESS_DENIED` | Insufficient permissions to call the method. ||
|| `TYPE_ERROR` | Data type error in the method call parameters. ||
|| `SYSTEM_ERROR` | Internal error while executing the method. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-update.md)
- [{#T}](./landing-landing-delete.md)
- [{#T}](./landing-landing-get-additional-fields.md)
- [{#T}](./landing-landing-get-preview.md)
- [{#T}](./landing-landing-get-public-url.md)