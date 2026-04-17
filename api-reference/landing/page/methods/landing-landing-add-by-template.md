# Add Page by Template landing.landing.addByTemplate

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.landing.addByTemplate` creates a page in the specified site based on the template code and returns the identifier of the created page. The new page is created as inactive (`ACTIVE = N`).

If you need to fully manage the fields of the created page, use [landing.landing.add](./landing-landing-add.md).

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal scope of landings. It is not related to the REST scope `landing` in the method name.

The value of `scope` must correspond to the type of site [(detailed description)](../../types.md) ||
|| **siteId***
[`integer`](../../../data-types.md) | Identifier of the site where the page needs to be created.

The site identifier can be obtained using the [landing.site.getList](../../site/landing-site-get-list.md) method or from the result of the [landing.site.add](../../site/landing-site-add.md) method ||
|| **code***
[`string`](../../../data-types.md) | Template code for the page.

A list of available templates can be obtained using the [landing.demos.getPageList](../../demos/landing-demos-get-page-list.md) method. The code depends on the templates available in Bitrix24. For example, the value might look like `krayt.monotovar@KraytPetShop` ||
|| **fields**
[`object`](../../../data-types.md) | Additional parameters for creating the page [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../../data-types.md) | Overrides the title of the created page and the SEO titles of the template.

If not provided, values from the template will be used ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Overrides the SEO descriptions of the template.

The value is recorded in the additional fields `METAOG_DESCRIPTION` and `METAMAIN_DESCRIPTION`, but not in the `DESCRIPTION` field of the page itself ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of an existing page or folder in the same site.

If a folder is provided, the new page will be created within it. If a page from a folder is provided, the new page will be created in the same folder. For an item without a folder, the method will take the value from `fields.FOLDER_ID`, if this parameter is specified.

If an active item is provided, the new page will be published automatically after creation. If `ID` points to a folder or an item with a filled `FOLDER_ID`, the `fields.FOLDER_ID` parameter is ignored.

The page identifier can be obtained using the [landing.landing.getList](./landing-landing-get-list.md) method, and the folder identifier can be obtained using the [landing.site.getFolders](../../site/landing-site-get-folders.md) method ||
|| **FOLDER_ID**
[`integer`](../../../data-types.md) | Identifier of the folder where the page needs to be created. The folder must belong to the same site as `siteId`, otherwise the method will return an error.

The folder identifier can be obtained using the [landing.site.getFolders](../../site/landing-site-get-folders.md) method.

This parameter is used if the creation folder is not determined by `fields.ID` ||
|| **SITE_TYPE**
[`string`](../../../data-types.md) | Type of templates from which the page is created. Site types from the article [Working with Site Types and Scopes](../../types.md) are used.

If not provided, the site type is taken. For sites of types `STORE` and `SMN`, the default value is `PAGE` ||
|| **PREPARE_BLOCKS**
[`boolean`](../../../data-types.md) | Enables preparation of template blocks when creating the page.

Works only together with `PREPARE_BLOCKS_DATA`. Pass boolean `true` ||
|| **PREPARE_BLOCKS_DATA**
[`object`](../../../data-types.md) | Additional data for preparing template blocks when creating the page.

In `PREPARE_BLOCKS_DATA`, first specify the block code from the page template, and inside - parameters for this block [(detailed description)](#prepare-blocks-data) ||
|| **ADD_IN_MENU**
[`string`](../../../data-types.md) | Flag for adding the created page to the site menu. Supported values are `Y` and `N`.

The logic triggers only when the value is `Y`, if the page is not already added to the menu through `BLOCK_ID` and `MENU_CODE`, and if `TITLE` is provided ||
|| **BLOCK_ID**
[`integer`](../../../data-types.md) | Together with `MENU_CODE`, adds a link to the created page in the menu of the block with the specified identifier ||
|| **MENU_CODE**
[`string`](../../../data-types.md) | Together with `BLOCK_ID`, specifies the menu code within the block where the link to the created page should be added ||
|#

### Parameter PREPARE_BLOCKS_DATA {#prepare-blocks-data}

Allows you to pass settings for individual template blocks even before the page is created.

Used only together with `PREPARE_BLOCKS: true`. If `PREPARE_BLOCKS` is not provided or is not passed as boolean `true`, the parameter is ignored.

Structure of the object:

```json
{
    "block_code_from_template": {
        "ACTION": "changeComponentParams",
        "PARAMS": {
            "PARAMETER_NAME": "value"
        }
    }
}
```

#|
|| **Name**
`type` | **Description** ||
|| **ACTION**
[`string`](../../../data-types.md) | What needs to be done with the block. The value `changeComponentParams` is supported ||
|| **PARAMS**
[`object`](../../../data-types.md) | Component parameters that need to be substituted into the block when creating the page.

Replacement applies only to parameters that are defined as an empty string in the block template ||
|#

The set of supported parameters depends on the specific block. For example, the following parameters may be used in blocks:

#|
|| **Parameter** | **Description** ||
|| `IBLOCK_ID` | Identifier of the catalog information block ||
|| `SECTION_ID` | Identifier of the catalog section ||
|| `ELEMENT_ID` | Identifier of the catalog element ||
|| `REQUISITE` | Identifier of the company requisite in the format `{COMPANY_ID}_{REQUISITE_ID}` for blocks `69.1.contacts` and `69.2.requisites` ||
|| `BANK_REQUISITE` | Identifier of the bank requisite in the format `{COMPANY_ID}_{BANK_REQUISITE_ID}` for the block `69.3.bank_requisites` ||
|#

The block code must match the block code in the template.

If [landing.demos.getPageList](../../demos/landing-demos-get-page-list.md) returns the composition of blocks for the selected template, the block code can be taken from `DATA.items[].code`.

If the composition of blocks is not returned, create a test page and check the block codes using the [landing.block.getList](../../block/methods/landing-block-get-list.md) method.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 157,
        "code": "krayt.monotovar@KraytPetShop",
        "fields": {
          "TITLE": "Spring Promotion",
          "DESCRIPTION": "SEO description of the promotion page",
          "FOLDER_ID": 95
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.addByTemplate.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 157,
        "code": "krayt.monotovar@KraytPetShop",
        "fields": {
          "TITLE": "Spring Promotion",
          "DESCRIPTION": "SEO description of the promotion page",
          "FOLDER_ID": 95
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.addByTemplate.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.addByTemplate',
    		{
    			siteId: 157,
    			code: 'krayt.monotovar@KraytPetShop',
    			fields: {
    				TITLE: 'Spring Promotion',
    				DESCRIPTION: 'SEO description of the promotion page',
    				FOLDER_ID: 95
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
                'landing.landing.addByTemplate',
                [
                    'siteId' => 157,
                    'code' => 'krayt.monotovar@KraytPetShop',
                    'fields' => [
                        'TITLE' => 'Spring Promotion',
                        'DESCRIPTION' => 'SEO description of the promotion page',
                        'FOLDER_ID' => 95,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding landing page by template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.addByTemplate',
        {
            siteId: 157,
            code: 'krayt.monotovar@KraytPetShop',
            fields: {
                TITLE: 'Spring Promotion',
                DESCRIPTION: 'SEO description of the promotion page',
                FOLDER_ID: 95
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
        'landing.landing.addByTemplate',
        [
            'siteId' => 157,
            'code' => 'krayt.monotovar@KraytPetShop',
            'fields' => [
                'TITLE' => 'Spring Promotion',
                'DESCRIPTION' => 'SEO description of the promotion page',
                'FOLDER_ID' => 95,
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
    "result": 2229,
    "time": {
        "start": 1773700566,
        "finish": 1773700567.67178,
        "duration": 1.6717801094055176,
        "processing": 1,
        "date_start": "2026-03-17T01:36:06+01:00",
        "date_finish": "2026-03-17T01:36:07+01:00",
        "operating_reset_at": 1773701166,
        "operating": 1.4516642093658447
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created page ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "SITE_ERROR",
    "error_description": "Site not found or access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameters are missing: `siteId`, `code`, or both parameters are absent in the request ||
|| `SITE_ERROR` | Site not found or access denied: a non-existent site is provided in `siteId` or the user does not have access to it ||
|| `LANDING_ERROR` | Page or folder not found: an identifier of an element that does not exist in the specified site is provided in `fields.ID` ||
|| `ACCESS_DENIED` | Access to create the page is denied: the user does not have "edit" permission for the specified site ||
|| `FOLDER_NOT_FOUND` | Folder not found: a folder that does not belong to the specified site or does not exist is provided in `fields.FOLDER_ID` ||
|| `WRONG_DATA` | Invalid template data: unable to retrieve template data for the provided `code` ||
|| `SLASH_IS_NOT_ALLOWED` | Slash is not allowed in the landing address: the template contains an invalid address for the created page ||
|| `CANT_BE_EMPTY` | The page address cannot be empty: the template contains an empty address for the created page ||
|| `WRONG_CODE_FORMAT` | Invalid page address: the template contains an address in the format `<characters>_<number>_<number>`, for example `code_12_34` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [landing.demos.getPageList](../../demos/landing-demos-get-page-list.md)
- [landing.site.getList](../../site/landing-site-get-list.md)
- [landing.site.getFolders](../../site/landing-site-get-folders.md)
- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-copy.md)
- [{#T}](./landing-landing-delete.md)
- [{#T}](./landing-landing-get-additional-fields.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-update.md)