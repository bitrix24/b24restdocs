# Update Page landing.landing.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "change settings" access permission for the site. To delete the page or change its publication date, additional "delete" and "publication" permissions are required on the site.

The method `landing.landing.update` updates the parameters of the page.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | The identifier of the page to be updated.

The page identifier can be obtained using the [landing.landing.getList](./landing-landing-get-list.md) method, as well as from the results of the [landing.landing.add](./landing-landing-add.md), [landing.landing.addByTemplate](./landing-landing-add-by-template.md), and [landing.landing.copy](./landing-landing-copy.md) methods. ||
|| **fields***
[`object`](../../../data-types.md) | A set of fields for the page to be updated [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../../data-types.md) | The title of the page. ||
|| **CODE**
[`string`](../../../data-types.md) | The symbolic code of the page. The code cannot contain the character `/` and must not be in the format `<letters, digits or _>_<number>_<number>`, for example `code_12_34`.

If an empty string is passed and the `FOLDER` field is not specified, the method will return an error. If the `FOLDER` field is passed with any value, there will be no error. ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | A free-form description of the page. ||
|| **XML_ID**
[`string`](../../../data-types.md) | The external identifier of the page. ||
|| **SITE_ID**
[`integer`](../../../data-types.md) | The identifier of the site to which the page should be linked. If the parameter is not passed, the current site of the page is used.

If both a new site and a folder are specified, the folder will be checked not in the new site but in the current site of the page. For such a scenario, it is better to use [landing.landing.move](./landing-landing-move.md). ||
|| **SITEMAP**
[`string`](../../../data-types.md) | A flag to include the page in the sitemap. Supported values are `Y` and `N`. Any other value will be saved as `N`. ||
|| **FOLDER**
[`string`](../../../data-types.md) | A folder indicator. Supported values are `Y` and `N`. Any other value will be saved as `N`. ||
|| **FOLDER_ID**
[`integer`](../../../data-types.md) | The identifier of the folder in which to place the page. The folder must be within the current site of the page. If `0` is passed, the page will be moved to the root of the site.

The folder identifier can be obtained using the [landing.site.getFolders](../../site/landing-site-get-folders.md) method. ||
|| **TPL_ID**
[`integer`](../../../data-types.md) | The identifier of the page view template.

The template identifier can be obtained using the [landing.template.getlist](../../template/landing-template-get-list.md) method. ||
|| **DELETED**
[`string`](../../../data-types.md) | Manages the state of the page in the trash. When this field is passed, the page becomes inactive.

`Y` — the page is moved to the trash and becomes inactive.
`N` — the page is restored from the trash but remains inactive until it is published separately.

For deleting and restoring the page, it is better to use the methods [landing.landing.markDelete](./landing-landing-mark-delete.md) and [landing.landing.markUnDelete](./landing-landing-mark-undelete.md).

If the page is set as the main page in the site settings and there are other undeleted pages in this site, the method will return the error `CANT_DELETE_MAIN`. ||
|| **DATE_PUBLIC**
[`string`](../../../data-types.md) | The date and time of the page publication.

To change this field, the "publication" permission on the site is required. Changing the field does not publish the page but only updates the publication date.

For publishing and unpublishing, use the methods [landing.landing.publication](./landing-landing-publication.md) and [landing.landing.unpublic](./landing-landing-unpublic.md). ||
|| **ADDITIONAL_FIELDS**
[`object`](../../../data-types.md) | Additional fields of the page. Available codes and values are described in the article [Additional Page Fields](../additional-fields.md). ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 349,
        "fields": {
          "TITLE": "Spring Sale 2026",
          "CODE": "spring-sale-2026",
          "DESCRIPTION": "Updated description of the promotion page",
          "XML_ID": "promo-2026-landing"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.update.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 349,
        "fields": {
          "TITLE": "Spring Sale 2026",
          "CODE": "spring-sale-2026",
          "DESCRIPTION": "Updated description of the promotion page",
          "XML_ID": "promo-2026-landing"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.update.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.update',
    		{
    			lid: 349,
    			fields: {
    				TITLE: 'Spring Sale 2026',
    				CODE: 'spring-sale-2026',
    				DESCRIPTION: 'Updated description of the promotion page',
    				XML_ID: 'promo-2026-landing'
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
                'landing.landing.update',
                [
                    'lid' => 349,
                    'fields' => [
                        'TITLE' => 'Spring Sale 2026',
                        'CODE' => 'spring-sale-2026',
                        'DESCRIPTION' => 'Updated description of the promotion page',
                        'XML_ID' => 'promo-2026-landing',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating page: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.update',
        {
            lid: 349,
            fields: {
                TITLE: 'Spring Sale 2026',
                CODE: 'spring-sale-2026',
                DESCRIPTION: 'Updated description of the promotion page',
                XML_ID: 'promo-2026-landing'
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
        'landing.landing.update',
        [
            'lid' => 349,
            'fields' => [
                'TITLE' => 'Spring Sale 2026',
                'CODE' => 'spring-sale-2026',
                'DESCRIPTION' => 'Updated description of the promotion page',
                'XML_ID' => 'promo-2026-landing',
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
    "result": true,
    "time": {
        "start": 1773835076,
        "finish": 1773835076.29,
        "duration": 0.28999996185302734,
        "processing": 0,
        "date_start": "2026-03-18T14:57:56+02:00",
        "date_finish": "2026-03-18T14:57:56+02:00",
        "operating_reset_at": 1773835676,
        "operating": 0.1001579761505127
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The result of the update. Returns `true` on success. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: lid"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required parameter `lid` or `fields` is missing. ||
|| `ACCESS_DENIED` | Insufficient permissions to modify the page or a specific field. For example, for `fields.DELETED`, the "delete" permission on the site is required, and for `fields.DATE_PUBLIC`, the "publication" permission on the site is required. ||
|| `SITE_NOT_FOUND` | An identifier of a non-existent site is passed in `fields.SITE_ID`. ||
|| `FOLDER_NOT_FOUND` | A folder that does not belong to the current site of the page or does not exist is passed in `fields.FOLDER_ID`. ||
|| `SLASH_IS_NOT_ALLOWED` | The character `/` is passed in `fields.CODE`. ||
|| `CANT_BE_EMPTY` | An empty string `''` is passed in `fields.CODE`, and the `fields.FOLDER` key is absent in the same request. ||
|| `WRONG_CODE_FORMAT` | A value in the format `<letters, digits or _>_<number>_<number>` is passed in `fields.CODE`, for example `code_12_34` or `my_page_1_2`. ||
|| `CANT_DELETE_MAIN` | Cannot mark the main page of the site as deleted if there are other undeleted pages in the site. ||
|| `TYPE_ERROR` | Data type error in the method call parameters. ||
|| `SYSTEM_ERROR` | Internal error during method execution. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-add-by-template.md)
- [{#T}](./landing-landing-delete.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-mark-delete.md)
- [{#T}](./landing-landing-mark-undelete.md)
- [{#T}](./landing-landing-move.md)
- [{#T}](./landing-landing-publication.md)
- [{#T}](./landing-landing-unpublic.md)