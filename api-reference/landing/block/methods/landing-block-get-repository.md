# Get a List of Blocks from the Repository `landing.block.getrepository`

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Sites and Stores" section

The method `landing.block.getrepository` returns the available sections of the block repository or the data for a specific section.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **section**
[`string`](../../../data-types.md) | The code of the repository section, for example `text`.

If the parameter is not provided, the method will return all available sections. In this case, the keys of the `result` object will be the section codes.

In addition to standard sections, the response may include sections from partner applications and system sections. To get the current section codes, call the method without `section`.

If a section with the specified code is not found or an empty string is provided, the method will return `false` without an error ||
|| **scope**
[`string`](../../../data-types.md) | An additional top-level parameter of the REST call that affects the type of site for which the repository is being collected.

For site types `PAGE`, `STORE`, and `SMN`, this parameter does not need to be provided. For `GROUP`, `KNOWLEDGE`, and `MAINPAGE`, provide the corresponding `scope`.

The rules for selecting the value are described in detail in the article [Working with Site Types and Scopes](../../types.md).

If `scope` is not provided, the method uses the current site type, and if it cannot be determined from the context, it operates as if for the `PAGE` type ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "section": "text"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.getrepository.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "section": "text",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.getrepository.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.getrepository',
    		{
    			section: 'text'
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
                'landing.block.getrepository',
                [
                    'section' => 'text',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting repository blocks: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.getrepository',
        {
            section: 'text'
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
        'landing.block.getrepository',
        [
            'section' => 'text',
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

### Example Response for Request with Section Parameter

```json
{
    "result": {
        "name": "Text",
        "meta": {
            "ai_text_placeholder": "text for website, family business, sale of flowers",
            "ai_text_max_tokens": 150
        },
        "new": false,
        "type": null,
        "specialType": null,
        "separator": false,
        "app_code": false,
        "items": {
            "03.1.three_cols_big_with_text_and_titles": {
                "id": null,
                "name": "Text in 3 Columns Full Width on Color Background",
                "namespace": "bitrix",
                "new": false,
                "version": null,
                "type": [],
                "section": [
                    "columns",
                    "text"
                ],
                "system": false,
                "description": "Three columns with headings and text",
                "preview": "//example.bitrix24.com/bitrix/blocks/bitrix/03.1.three_cols_big_with_text_and_titles/preview.jpg",
                "restricted": false,
                "repo_id": false,
                "app_code": false,
                "only_for_license": "",
                "requires_updates": false
            },
            "repo_405": {
                "id": null,
                "new": false,
                "name": "Block 'Text + Image' with Button, Text on the Right, Image on the Left",
                "description": null,
                "namespace": "krayt.monotovar",
                "type": [],
                "section": [
                    "about",
                    "text",
                    "image"
                ],
                "preview": "https://example.com/upload/iblock/5aa/5aabf5b9241876561d5db49c482dcd96.png",
                "restricted": true,
                "repo_id": "405",
                "app_code": "krayt.monotovar",
                "requires_updates": false
            }
        }
    },
    "time": {
        "start": 1774521670,
        "finish": 1774521670.823288,
        "duration": 0.8232879638671875,
        "processing": 0,
        "date_start": "2026-03-26T13:41:10+02:00",
        "date_finish": "2026-03-26T13:41:10+02:00",
        "operating_reset_at": 1774522270,
        "operating": 0
    }
}
```

### Example Response for Request without Section Parameter

```json
{
    "result": {
        "favourite": {
            "name": "Favorites",
            "meta": [],
            "new": false,
            "type": null,
            "specialType": null,
            "separator": false,
            "app_code": false,
            "items": {}
        },
        "text": {
            "name": "Text",
            "meta": {
                "ai_text_placeholder": "text for website, family business, sale of flowers",
                "ai_text_max_tokens": 150
            },
            "new": false,
            "type": null,
            "specialType": null,
            "separator": false,
            "app_code": false,
            "items": {
                "03.1.three_cols_big_with_text_and_titles": {
                    "id": null,
                    "name": "Text in 3 Columns Full Width on Color Background",
                    "namespace": "bitrix",
                    "new": false,
                    "version": null,
                    "type": [],
                    "section": [
                        "columns",
                        "text"
                    ],
                    "system": false,
                    "description": "Three columns with headings and text",
                    "preview": "//example.bitrix24.com/bitrix/blocks/bitrix/03.1.three_cols_big_with_text_and_titles/preview.jpg",
                    "restricted": false,
                    "repo_id": false,
                    "app_code": false,
                    "only_for_license": "",
                    "requires_updates": false
                }
            }
        }
    },
    "time": {
        "start": 1774521670,
        "finish": 1774521670.823288,
        "duration": 0.8232879638671875,
        "processing": 0,
        "date_start": "2026-03-26T13:41:10+02:00",
        "date_finish": "2026-03-26T13:41:10+02:00",
        "operating_reset_at": 1774522270,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) \| [`boolean`](../../../data-types.md) | The result of the request.

If `section` is not provided, `result` contains an object where the keys are the section codes and the values are the section objects [(detailed description)](#repository-section).

If `section` is provided and found, `result` contains a single section object [(detailed description)](#repository-section).

If `section` is provided but such a section does not exist, the method will return `false` without an error ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

### Section Object {#repository-section}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | The name of the section ||
|| **meta**
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | Additional data for the section.

For example, the section may return service fields for AI prompts ||
|| **new**
[`boolean`](../../../data-types.md) | Indicates whether there are new blocks in the section ||
|| **type**
[`string`](../../../data-types.md) \| [`array`](../../../data-types.md) \| [`null`](../../../data-types.md) | Restriction of the section by site type, if specified.

The value can be an array of types, a string, or `null` if no restriction is specified. For rules on selecting the site type, refer to the article [Working with Site Types and Scopes](../../types.md) ||
|| **specialType**
[`string`](../../../data-types.md) \| [`null`](../../../data-types.md) | Additional type of the section, if specified.

If no additional type is specified, the method will return `null` ||
|| **separator**
[`boolean`](../../../data-types.md) | Indicates a service separator.

Sections with this flag are needed for visually separating categories in the block editor. They do not contain blocks, so such sections can be skipped when traversing the response ||
|| **app_code**
[`string`](../../../data-types.md) \| [`boolean`](../../../data-types.md) | The application code if the section belongs to the partner catalog, otherwise `false` ||
|| **items**
[`object`](../../../data-types.md) | A set of blocks in the section, where the key is the block code and the value is the block object [(detailed description)](#repository-block).

For favorite blocks, the key may contain the suffix `@<ID>` ||
|#

Section Features:

- `last` contains blocks that the user recently added in the editor. This section is filled only in edit mode. In a regular REST call, this section is usually not returned,
- `favourite` contains favorite blocks and may be returned even with an empty `items`,
- example of a separator: `separator_apps`,
- the composition of the section fields may vary. Sections from partner applications may lack `meta`, `type`, and `specialType`.

### Block Object {#repository-block}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) \| [`null`](../../../data-types.md) | Internal identifier of the block from the repository, if specified.

For some blocks, the method will return `null` ||
|| **name**
[`string`](../../../data-types.md) | The name of the block ||
|| **namespace**
[`string`](../../../data-types.md) | The namespace of the block ||
|| **new**
[`boolean`](../../../data-types.md) | Indicates whether the block is new ||
|| **version**
[`string`](../../../data-types.md) \| [`null`](../../../data-types.md) | Minimum version of the product for which the block is designed, if specified.

If no restriction is specified, the method will return `null` ||
|| **type**
[`array`](../../../data-types.md) | List of site types for which the block is available.

An empty array means that no specific restriction by site type is set for the block. For rules on selecting the site type, refer to the article [Working with Site Types and Scopes](../../types.md) ||
|| **section**
[`string`](../../../data-types.md) \| [`array`](../../../data-types.md) | The code of the section or a list of section codes in which the block is displayed.

Usually, an array of codes is returned in the response, for example `["columns", "text"]`. These codes can be used in the `section` parameter in the next method call ||
|| **system**
[`boolean`](../../../data-types.md) | Indicates whether the block is a system block used internally by the platform — not intended for user addition to the page.

Such blocks are not included in the method's response by default ||
|| **description**
[`string`](../../../data-types.md) \| [`null`](../../../data-types.md) | A brief description of the block.

The field may be an empty string or `null` ||
|| **preview**
[`string`](../../../data-types.md) | The path or URL of the preview image.

For standard blocks, a URL without specifying the protocol may be returned, such as `//<domain>/...`, for partner blocks - an absolute URL, and if there is no image, the field may be an empty string ||
|| **restricted**
[`boolean`](../../../data-types.md) | Indicates additional restrictions on the block ||
|| **repo_id**
[`integer`](../../../data-types.md) \| [`string`](../../../data-types.md) \| [`boolean`](../../../data-types.md) | Identifier of the partner block from the application repository or `false` for a standard block ||
|| **app_code**
[`string`](../../../data-types.md) \| [`boolean`](../../../data-types.md) | The application code to which the block belongs, or `false` ||
|| **only_for_license**
[`string`](../../../data-types.md) | The license code for which the block is available, if specified.

If the field is filled, the value corresponds to the current license of the account. Blocks for other licenses do not appear in the response ||
|| **app_expired**
[`boolean`](../../../data-types.md) | Indicates whether the subscription for the application to which the block belongs has expired.

This field is present only for partner blocks with an expired subscription ||
|| **requires_updates**
[`boolean`](../../../data-types.md) | Indicates whether a newer version of the product is required for the block to function correctly ||
|| **favorite**
[`boolean`](../../../data-types.md) | Additional flag for favorite blocks ||
|| **favoriteMy**
[`boolean`](../../../data-types.md) | Additional flag for favorite blocks indicating that the block is saved by the current user ||
|#

The composition of the block fields may vary for standard, partner, and favorite blocks. The method returns only those fields that are present for a specific block.

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Insufficient permissions."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | The user does not have access to the "Sites and Stores" section ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-get-content-from-repository.md)
- [{#T}](./landing-block-get-manifest.md)
- [{#T}](./landing-block-get-manifest-file.md)
