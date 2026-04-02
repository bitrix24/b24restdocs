# Update Attributes of Elements in landing.block.updateattrs

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "edit" access permission for the site

The method `landing.block.updateattrs` updates the attributes of HTML elements within a block in the draft of a page.

This method does not change the text, the entire HTML code of the block, or the styles. It only updates the attribute values of existing elements, such as `href`, `target`, `alt`, `title`, `data-*`, and `aria-*`.

If the page is already published, changes will be visible to visitors after re-publishing through the interface or using the method [landing.landing.publication](../../page/methods/landing-landing-publication.md).

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getList](../../page/methods/landing-landing-get-list.md) ||
|| **block***
[`integer`](../../../data-types.md) | Block identifier in the version of the page for editing.

The block identifier can be obtained using the method [landing.block.getlist](./landing-block-get-list.md) with the parameter `params.edit_mode = 1`. If you pass the block identifier from the published version of the page, the method may return an error ||
|| **data***
[`object`](../../../data-types.md) | Set of attributes to update [(detailed description)](#data) ||
|#

### Parameter data {#data}

#|
|| **Key**
`type` | **Description** ||
|| **<selector>**
[`object`](../../../data-types.md) | The key must match the CSS selector from the block manifest. The value must be an object of the form `{ '<attribute>': <value> }`.

The method finds elements by the selector and assigns them new attribute values. If the selector appears multiple times, you can specify the position using `@`, for example, `.landing-block-node-text@1`. Positions are numbered starting from `0`.

If the position is not specified, the method updates all found elements with that selector ||
|#

The allowed selectors and attributes are taken from the `style.nodes`, `attrs`, `cards`, and `style.block` sections of the block manifest. You can check them using the method [landing.block.getmanifest](./landing-block-get-manifest.md). If you are checking the block from the version of the page for editing, pass the parameter `params.edit_mode = 1` to `landing.block.getmanifest`.

If the required attribute is described only in the `nodes` section of the manifest, use [landing.block.updatenodes](./landing-block-update-nodes.md). If the selector or attribute is not present in the supported sections of the manifest, the method will ignore it without an error.

Use the method when you need to change the settings of an element through attributes, rather than its content. For example, you can change the link of a button, the `target` for opening in a new tab, the `alt` for an image, or `data-*` for a form.

For instance, in a CRM form block, the method allows you to change the `data-b24form` attribute of the `.bitrix24forms` element and connect a different form. Acceptable values for such an attribute should be taken from the manifest of the specific block.

Strings, numbers, and boolean values are saved as HTML attributes. If you pass an array or object, the method converts them into a JSON string. The data format must correspond to the type of the attribute specified in the block manifest. The method does not check whether the passed value fits the logic of a specific block, so it is better to refer to the manifest. Examples of formats for different attribute types can be found in the article [Attribute Types](../attributes.md#attribute-types).

For example, if a repeating element in the manifest allows the `data-test-checkbox` attribute, the request for the second card might look like this:

```json
{
    ".container-fluid@1": {
        "data-test-checkbox": [1, 2, 3]
    }
}
```

For example, if the block contains a button:

```html
<a class="landing-block-node-button" href="/old/" target="_self">Buy</a>
```

If you pass the following data:

```json
{
    ".landing-block-node-button": {
        "href": "/catalog/",
        "target": "_blank"
    }
}
```

The button text will not change, but the method will update its attributes. After the call, the button will link to `/catalog/` and open in a new tab.

To change the attributes of the block wrapper, pass the actual identifier in the format `#block<blockId>`, for example, `#block6058`. You cannot pass `#wrapper` as a key in `data`: such a request will not work.

For dynamic block and component parameters, use [landing.block.updatenodes](./landing-block-update-nodes.md).

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 313,
        "block": 6134,
        "data": {
          ".bitrix24forms": {
            "data-b24form": "#crmFormInline45",
            "data-b24form-use-style": "N"
          }
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.updateattrs.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 313,
        "block": 6134,
        "data": {
          ".bitrix24forms": {
            "data-b24form": "#crmFormInline45",
            "data-b24form-use-style": "N"
          }
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.updateattrs.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.updateattrs',
    		{
    			lid: 313,
    			block: 6134,
    			data: {
    				'.bitrix24forms': {
    					'data-b24form': '#crmFormInline45',
    					'data-b24form-use-style': 'N'
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
                'landing.block.updateattrs',
                [
                    'lid' => 313,
                    'block' => 6134,
                    'data' => [
                        '.bitrix24forms' => [
                            'data-b24form' => '#crmFormInline45',
                            'data-b24form-use-style' => 'N',
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
        echo 'Error updating block attributes: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.updateattrs',
        {
            lid: 313,
            block: 6134,
            data: {
                '.bitrix24forms': {
                    'data-b24form': '#crmFormInline45',
                    'data-b24form-use-style': 'N'
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
        'landing.block.updateattrs',
        [
            'lid' => 313,
            'block' => 6134,
            'data' => [
                '.bitrix24forms' => [
                    'data-b24form' => '#crmFormInline45',
                    'data-b24form-use-style' => 'N',
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

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774442460,
        "finish": 1774442460.28751,
        "duration": 0.2875099182128906,
        "processing": 0,
        "date_start": "2026-03-25T11:01:00+01:00",
        "date_finish": "2026-03-25T11:01:00+01:00",
        "operating_reset_at": 1774443060,
        "operating": 0.09410285949707031
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The result of updating the attributes. Upon successful execution, the method returns `true` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "BLOCK_NOT_FOUND",
    "error_description": "Block not found"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid`, `block`, or `data` is missing ||
|| `ACCESS_DENIED` | Insufficient permissions to edit the site ||
|| `LANDING_NOT_EXIST` | Page with identifier `lid` not found or not accessible to the current user ||
|| `BLOCK_NOT_FOUND` | Block with identifier `block` not found on page `lid` or not accessible in the version of the page for editing ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-update-nodes.md)
- [{#T}](./landing-block-update-styles.md)
- [{#T}](./landing-block-get-list.md)
- [{#T}](./landing-block-get-manifest.md)
- [{#T}](../../page/methods/landing-landing-publication.md)