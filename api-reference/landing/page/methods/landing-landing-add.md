# Add Page or Folder landing.landing.add

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for the site

The method `landing.landing.add` adds a page or folder to the specified site and returns the identifier of the created object. The new object is created as inactive (`ACTIVE = N`).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Set of fields for the new page or folder [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE***
[`string`](../../../data-types.md) | Title of the page ||
|| **SITE_ID***
[`integer`](../../../data-types.md) | Identifier of the site where the page is created.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) or from the result of the method [landing.site.add](../../site/landing-site-add.md) ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the page. It must not contain the character `/` and must not be in the format `<characters>_<number>_<number>`, for example `code_12_34`.

If the field is not provided or a string consisting only of spaces is passed, the code will be generated from `TITLE`. If the transliterated code is empty, a string of 12 characters will be generated.

An empty string `''` cannot be passed without the `FOLDER` parameter — the method will return an error. If the `FOLDER` parameter is passed with any value, an empty string is allowed, and the code will be generated from `TITLE`.

If such a code is already in use within the site or folder after creation, a suffix of the form `_<4 random characters>` will be added to it ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Arbitrary description of the page ||
|| **XML_ID**
[`string`](../../../data-types.md) | External identifier of the page ||
|| **SITEMAP**
[`string`](../../../data-types.md) | Flag to include the page in the sitemap. Supported values are `Y` and `N`, default is `N` ||
|| **FOLDER**
[`string`](../../../data-types.md) | Used if a folder needs to be created instead of a page. Supported values are `Y` and `N`, default is `N` ||
|| **FOLDER_ID**
[`integer`](../../../data-types.md) | Identifier of the folder where the page should be created. The folder must belong to the same site as `SITE_ID`.

The folder identifier can be obtained using the method [landing.site.getFolders](../../site/landing-site-get-folders.md) ||
|| **TPL_ID**
[`integer`](../../../data-types.md) | Identifier of the page view template.

The view template identifier can be obtained using the method [landing.template.getlist](../../template/landing-template-get-list.md) ||
|| **ADDITIONAL_FIELDS**
[`object`](../../../data-types.md) | Additional fields for the page. Available codes and values are described in the article [Additional Page Fields](../additional-fields.md) ||
|| **BLOCK_ID**
[`integer`](../../../data-types.md) | Used with `MENU_CODE` to add a link to the menu of the block with the specified identifier after the page is created ||
|| **MENU_CODE**
[`string`](../../../data-types.md) | Used with `BLOCK_ID` to specify the menu code in the block where the link to the created page should be added ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "fields": {
          "TITLE": "Spring Sale",
          "SITE_ID": 292,
          "CODE": "spring-sale",
          "DESCRIPTION": "Page for seasonal promotion",
          "ADDITIONAL_FIELDS": {
            "THEME_CODE": "wedding"
          }
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.add.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "fields": {
          "TITLE": "Spring Sale",
          "SITE_ID": 292,
          "CODE": "spring-sale",
          "DESCRIPTION": "Page for seasonal promotion",
          "ADDITIONAL_FIELDS": {
            "THEME_CODE": "wedding"
          }
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.add.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.add',
    		{
    			fields: {
    				TITLE: 'Spring Sale',
    				SITE_ID: 292,
    				CODE: 'spring-sale',
    				DESCRIPTION: 'Page for seasonal promotion',
    				ADDITIONAL_FIELDS: {
    					THEME_CODE: 'wedding'
    				}
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
                'landing.landing.add',
                [
                    'fields' => [
                        'TITLE' => 'Spring Sale',
                        'SITE_ID' => 292,
                        'CODE' => 'spring-sale',
                        'DESCRIPTION' => 'Page for seasonal promotion',
                        'ADDITIONAL_FIELDS' => [
                            'THEME_CODE' => 'wedding',
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding landing page: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.add',
        {
            fields: {
                TITLE: 'Spring Sale',
                SITE_ID: 292,
                CODE: 'spring-sale',
                DESCRIPTION: 'Page for seasonal promotion',
                ADDITIONAL_FIELDS: {
                    THEME_CODE: 'wedding'
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
        'landing.landing.add',
        [
            'fields' => [
                'TITLE' => 'Spring Sale',
                'SITE_ID' => 292,
                'CODE' => 'spring-sale',
                'DESCRIPTION' => 'Page for seasonal promotion',
                'ADDITIONAL_FIELDS' => [
                    'THEME_CODE' => 'wedding',
                ],
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

{% note warning %}

If you want the new page to immediately appear in the block menu, pass both service parameters: `BLOCK_ID` and `MENU_CODE` in `fields`

{% endnote %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 2227,
    "time": {
        "start": 1773694924,
        "finish": 1773694924.307754,
        "duration": 0.3077540397644043,
        "processing": 0,
        "date_start": "2026-03-17T00:02:04+01:00",
        "date_finish": "2026-03-17T00:02:04+01:00",
        "operating_reset_at": 1773695524,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created page or folder ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "SLASH_IS_NOT_ALLOWED",
    "error_description": "Slash is not allowed in the landing address."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required top-level parameter `fields` is missing ||
|| `ACCESS_DENIED` | Access to create the page is denied: the user does not have "edit" permission for the specified site ||
|| `SITE_NOT_FOUND` | Site not found: a non-existent site identifier is passed in `fields.SITE_ID` ||
|| `FOLDER_NOT_FOUND` | Folder not found: a folder that does not belong to the specified site or does not exist is passed in `fields.FOLDER_ID` ||
|| `SLASH_IS_NOT_ALLOWED` | Slash is not allowed in the landing address: a character `/` is passed in `fields.CODE` ||
|| `CANT_BE_EMPTY` | The page address cannot be empty: an empty string `''` is passed in `fields.CODE` ||
|| `WRONG_CODE_FORMAT` | Invalid page address: a value in the format `<characters>_<number>_<number>` is passed in `fields.CODE`, for example `code_12_34` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add-by-template.md)
- [{#T}](./landing-landing-copy.md)
- [{#T}](./landing-landing-delete.md)
- [{#T}](./landing-landing-get-additional-fields.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-publication.md)
- [{#T}](./landing-landing-update.md)