# Get the List of Sites landing.site.getList

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with "view" access permission for sites

The method `landing.site.getList` retrieves a list of sites based on selection parameters.

{% note warning %}

By default, only sites with `DELETED = "N"` are included in the selection. To retrieve deleted sites, pass `DELETED` or `=DELETED` in the filter.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../data-types.md) | Internal scope of landings. It is not related to the REST scope `landing` in the method name.

For `GROUP`, `KNOWLEDGE`, and `MAINPAGE`, pass the corresponding `scope` [(detailed description)](#type-scope) ||
|| **params**
[`object`](../../data-types.md) | Parameters for selecting sites [(detailed description)](#params) ||
|#

### Parameter params {#params}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | List of fields to select from [basic site fields](./base-fields.md). If the parameter is not provided or is not an array, `["*"]` is used. The method always adds `ID` and `TYPE` to the selection. ||
|| **filter**
[`object`](../../data-types.md) | Filter by fields from [basic site fields](./base-fields.md). If the parameter is not provided or is not an array, an empty filter `{}` is used.

If the `TYPE` parameter or `=TYPE` specifies the value `STORE`, the public method converts it to `["STORE", "SMN"]`.

Exact filtering by type works only for a single value that is allowed in the current internal `scope`. If an array or a value not available in the current scope is provided, the method will substitute it with a list of allowed types. ||
|| **order**
[`object`](../../data-types.md) | Sorting in the format `{"FIELD": "ASC DESC"}`. If the parameter is not provided, no special sorting is applied. ||
|| **group**
[`array`](../../data-types.md) | Grouping in ORM format. If not an array, the parameter is converted to an empty array. When filtering by access permissions, `ID` is added to the grouping. ||
|| **limit**
[`integer`](../../data-types.md) | Limit on the number of rows in the selection at the ORM level. By default, it is not set. ||
|| **offset**
[`integer`](../../data-types.md) | Offset for the selection at the ORM level. If the parameter is not provided, the default ORM behavior is applied. ||
|#

### TYPE and scope Correspondence {#type-scope}

Site types and rules for selecting the `scope` parameter are described in the article [Working with Site Types and Scopes](../types.md).
The table below applies to `params.filter.TYPE` and `params.filter.=TYPE`.

#|
|| **params.filter.TYPE** | **scope in request** | **When to use** ||
|| `PAGE`, `STORE`, `SMN` | do not pass | Selection of sites and stores in the standard `scope` ||
|| `GROUP` | `GROUP` | Selection of group sites ||
|| `KNOWLEDGE` | `KNOWLEDGE` | Selection of knowledge bases ||
|| `MAINPAGE` | `MAINPAGE` | Selection of the main page or vibe ||
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
            "TYPE"
          ],
          "filter": {
            "=DELETED": "N"
          },
          "order": {
            "ID": "DESC"
          }
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.getList.json"
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
            "TYPE"
          ],
          "filter": {
            "=DELETED": "N"
          },
          "order": {
            "ID": "DESC"
          }
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.getList.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.getList',
    		{
    			params: {
    				select: [
    					'ID',
    					'TITLE',
    					'TYPE'
    				],
    				filter: {
    					'=DELETED': 'N'
    				},
    				order: {
    					ID: 'DESC'
    				}
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
                'landing.site.getList',
                [
                    'params' => [
                        'select' => [
                            'ID',
                            'TITLE',
                            'TYPE',
                        ],
                        'filter' => [
                            '=DELETED' => 'N',
                        ],
                        'order' => [
                            'ID' => 'DESC',
                        ],
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting site list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getList',
        {
            params: {
                select: [
                    'ID',
                    'TITLE',
                    'TYPE'
                ],
                filter: {
                    '=DELETED': 'N'
                },
                order: {
                    ID: 'DESC'
                }
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
        'landing.site.getList',
        [
            'params' => [
                'select' => [
                    'ID',
                    'TITLE',
                    'TYPE',
                ],
                'filter' => [
                    '=DELETED' => 'N',
                ],
                'order' => [
                    'ID' => 'DESC',
                ],
            ]
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
            "ID": "157",
            "TITLE": "Randu Villa",
            "TYPE": "PAGE"
        },
        {
            "ID": "147",
            "TITLE": "Test Test yesss",
            "TYPE": "STORE"
        },
        {
            "ID": "3",
            "TITLE": "Fork Museum",
            "TYPE": "PAGE"
        }
    ],
    "time": {
        "start": 1773269838,
        "finish": 1773269838.647153,
        "duration": 0.6471529006958008,
        "processing": 0,
        "date_start": "2026-03-12T01:57:18+01:00",
        "date_finish": "2026-03-12T01:57:18+01:00",
        "operating_reset_at": 1773270438,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../data-types.md) | List of sites [(detailed description)](#site). The method may return `result: []` without an error if there are no suitable sites according to the filter or if the user does not have "view" access permission for these sites. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

#### Site Object {#site}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Site identifier. This field is always present in the response. ||
|| **TYPE**
[`string`](../../data-types.md) | Site type. This field is always present in the response. ||
|| **DOMAIN_NAME**
[`string`](../../data-types.md) | Site domain, returned when selecting the `DOMAIN_NAME` field. ||
|| **PUBLIC_URL**
[`string`](../../data-types.md) | Public URL of the site, returned when selecting the `PUBLIC_URL` field.

It may be an empty string if the URL could not be determined. ||
|| **PREVIEW_PICTURE**
[`string`](../../data-types.md) | URL of the preview of the main page of the site, returned when selecting the `PREVIEW_PICTURE` field. It may be an empty string if the preview is unavailable. ||
|| **PHONE**
[`string`](../../data-types.md) \| `null` | Company phone from CRM, returned when selecting the `PHONE` field. ||
|| **DATE_CREATE**
[`string`](../../data-types.md) | Creation date in string format, returned when selecting the `DATE_CREATE` field. ||
|| **DATE_MODIFY**
[`string`](../../data-types.md) | Modification date in string format, returned when selecting the `DATE_MODIFY` field. ||
|| **LANDING_ID_INDEX**
[`string`](../../data-types.md) \| `null` | May be present if `PREVIEW_PICTURE` was requested in `select`. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions to call landing methods. ||
|| `TYPE_ERROR` | Data type error in method call parameters. ||
|| `SYSTEM_ERROR` | Internal error during method execution. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add.md)
- [{#T}](./landing-site-update.md)
- [{#T}](./landing-site-delete.md)
- [{#T}](./landing-site-get-public-url.md)
- [{#T}](./landing-site-get-folders.md)
