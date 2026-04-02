# Get Block Content from Repository landing.block.getContentFromRepository

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Sites and Stores" section

The method `landing.block.getContentFromRepository` returns the HTML content of a block from the repository.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`string`](../../../data-types.md) | The code of the block whose content needs to be retrieved.

Several value formats are supported:

- Short code of a standard block, for example, `01.big_with_text`,
- Block code from the application repository in the format `repo_<ID>`,
- Code of a saved block in the format `<code>@<ID>`.

The block code can be obtained from the result of the method [landing.block.getRepository](./landing-block-get-repository.md). For a saved block, it is `repo_<ID>`, for a block from the repository — `<code>@<ID>`, and for a standard block — the value from the `code` field.

If you need to get the description of the block and its parameters, use the method [landing.block.getManifestFile](./landing-block-get-manifest-file.md).

The method only works with supported code formats. If a code in the format `<namespace>:<code>`, an empty string, an unknown code, or the code of an already deleted saved block is passed, `result` will return `null`, without a separate error. ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "01.big_with_text"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.getContentFromRepository.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "01.big_with_text",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.getContentFromRepository.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.block.getContentFromRepository',
    		{
    			code: '01.big_with_text'
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
                'landing.block.getContentFromRepository',
                [
                    'code' => '01.big_with_text',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting repository block content: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.getContentFromRepository',
        {
            code: '01.big_with_text'
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
        'landing.block.getContentFromRepository',
        [
            'code' => '01.big_with_text',
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
    "result": "<section class=\"g-pos-rel landing-block g-overflow-hidden\" data-slider-autoplay=\"1\" data-slider-autoplay-speed=\"5000\" data-slider-dots=\"0\">\n\t<div class=\"js-carousel g-overflow-hidden g-max-height-100vh\" data-autoplay=\"true\" data-infinite=\"true\" data-fade=\"true\" data-pagi-classes=\"u-carousel-indicators-v1 g-absolute-centered--x g-bottom-20\" data-speed=\"5000\">\n\t\t<div class=\"landing-block-card-img js-slide g-bg-img-hero g-min-height-100vh\" style=\"background-image: url(https://cdn.bitrix24.site/bitrix/images/landing/business/1600x1075/img2.jpg);\"></div>\n\t\t<div class=\"landing-block-card-img js-slide g-bg-img-hero g-min-height-100vh\" style=\"background-image: url(https://cdn.bitrix24.site/bitrix/images/landing/business/1600x1075/img1.jpg);\"></div>\n\t</div>\n\n\t<div class=\"g-absolute-centered g-width-80x--md\">\n\t\t<div class=\"container text-center g-max-width-800\">\n\t\t\t<div class=\"landing-block-node-text-container info-v3-4 g-bg-primary-opacity-0_9 g-pa-20 g-pa-60--md js-animation fadeInLeft\">\n\t\t\t\t<div class=\"g-pos-rel g-z-index-3\">\n\t\t\t\t\t<h3 class=\"landing-block-node-small-title text-uppercase g-letter-spacing-3 g-color-white g-mb-10\">We are Company24</h3>\n\t\t\t\t\t<h2 class=\"landing-block-node-title h1 text-uppercase g-color-white g-letter-spacing-5 g-mb-20\">Business &amp; Corporation</h2>\n\t\t\t\t\t<div class=\"landing-block-node-text g-line-height-1_8 g-letter-spacing-3 g-color-white g-mb-20\" data-auto-font-scale>Sed feugiat porttitor nunc, non dignissim\n\t\t\t\t\t\t<br> ipsum vestibulum in. Donec in blandit dolor.</div>\n\t\t\t\t\t<a href=\"#\" class=\"landing-block-node-button btn g-btn-type-outline g-btn-white g-btn-size-md g-btn-px-m text-uppercase rounded-0\">Learn More</a>\n\t\t\t\t</div>\n\t\t\t</div>\n\t\t</div>\n\t</div>\n</section>",
    "time": {
        "start": 1774525475,
        "finish": 1774525475.811345,
        "duration": 0.811345100402832,
        "processing": 0,
        "date_start": "2026-03-26T14:44:35+01:00",
        "date_finish": "2026-03-26T14:44:35+01:00",
        "operating_reset_at": 1774526075,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../../data-types.md) \| [`null`](../../../data-types.md) | HTML content of the block.

For standard blocks, the method returns the HTML template of the block with values substituted for the current language in Bitrix24.

For blocks `repo_<ID>`, the method returns the HTML saved for the block in the application repository.

For blocks `<code>@<ID>`, the method returns the HTML of the saved block or `null` if such a block has already been deleted. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: code"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required parameter `code` was not provided. ||
|| `ACCESS_DENIED` | Access to the method call is denied: the user does not have access to the "Sites and Stores" section. ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-block-get-content.md)
- [{#T}](./landing-block-get-manifest.md)
- [{#T}](./landing-block-get-manifest-file.md)
- [{#T}](./landing-block-get-repository.md)