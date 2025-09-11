# Add a custom block to the repository landing.repo.register

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.repo.register` adds a block to the repository. It returns an error or the `ID` of the added block. This `ID` is used to add the block to programmatically created landing pages.

When adding, a check is performed. If a block with this code already exists in the system, it will be removed.

The method may return an error about dangerous content in the block. In this case, it is necessary to first check the registered content using the method [landing.repo.checkContent](./landing-repo-check-content.md).

When developing a new block or modifying an existing one, it may be necessary to see changes faster than re-adding the block or using the RESET flag allows. It is recommended to use the method [landing.block.updatecontent](../block/methods/landing-block-update-content.md) for these purposes. This method sends arbitrary content to the block and displays changes almost "on the fly." Once development is complete, the developer can finalize the registration.

The method is suitable only for changing content. When modifying the manifest, the block needs to be re-registered (without re-adding it to the page).

## Parameters

#|
|| **Method** | **Description** | **Version** ||
|| **code**
[`unknown`](../../data-types.md) | Unique code for your block, which will be used to remove the block if necessary. ||
|| **fields**
[`unknown`](../../data-types.md) | An array of fields describing your block, consisting of keys:
- NAME - block name
- DESCRIPTION - block description
- SECTIONS - categories in which the block should appear, separated by commas.

  {% note info %}
  
  If the required category is not in the list, simply write its text in the manifest, and the category will be added. The key for the new category becomes the value `md5(strtolower($sectionName))`.
  
  {% endnote %}

- PREVIEW - URL of the block's cover image
- CONTENT - HTML content of the block
- ACTIVE - block activity (Y / N)
- SITE_TEMPLATE_ID – binding the block to a specific template of the main module's site. **Only for on-premise versions!**

Additional parameters:
- RESET - if passed with the value Y, the system will automatically update all blocks added to the pages to the new layout. ||
|| **manifest**
[`unknown`](../../data-types.md) | An array of the manifest describing the block. ||
|#

{% note info %}

The **style** attribute may be stripped by the built-in sanitizer. To bypass this, use the **bxstyle** attribute instead. When added, the system converts it to the standard style.

{% endnote %}


## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.repo.register',
    		<?= \CUtil::PhpToJSObject($data) ?>
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.info(result);
    	}
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.repo.register',
                [
                    'code' => 'myblockx',
                    'fields' => [
                        'NAME'        => 'Test block',
                        'DESCRIPTION' => 'Just try!',
                        'SECTIONS'    => 'cover,about',
                        'PREVIEW'     => 'https://www.bitrix24.com/images/b24_screen.png',
                        'CONTENT'     => '
        <section class="landing-block">
            <div class="text-center g-color-gray-dark-v3 g-pa-10">
                <div class="g-width-600 mx-auto">
                    <div class="landing-block-node-text g-font-size-12 ">
                        <p>© 2017 All rights reserved. Developed by
                        <a href="#" class="landing-block-node-link g-color-primary">Bitrix24</a></p>
                    </div>
                </div>
            </div>
        </section>'
                    ],
                    'manifest' => [
                        'assets' => [
                            'css' => [
                                'https://site.com/aaa.css'
                            ],
                            'js'  => [
                                'https://site.com/aaa.js'
                            ]
                        ],
                        'nodes'  => [
                            '.landing-block-node-text' => [
                                'name' => 'Text',
                                'type' => 'text',
                            ],
                            '.landing-block-node-link' => [
                                'name' => 'Link',
                                'type' => 'link',
                            ],
                        ],
                        'style'  => [
                            '.landing-block-node-text' => [
                                'name' => 'Text',
                                'type' => 'typo',
                            ],
                            '.landing-block-node-link' => [
                                'name' => 'Link',
                                'type' => 'typo',
                            ],
                        ],
                        'attrs'  => [
                            '.landing-block-node-text' => [
                                'name'      => 'Copyright settings',
                                'type'      => 'dropdown',
                                'attribute' => 'data-copy',
                                'items'     => [
                                    'val1' => 'Value 1',
                                    'val2' => 'Value 2'
                                ]
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling landing.repo.register: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    <?php
    //for clarity, we will pass a PHP array to JS execution
    $data = array(
        'code' => 'myblockx',
        'fields' => array(
            'NAME' => 'Test block',
            'DESCRIPTION' => 'Just try!',
            'SECTIONS' => 'cover,about',
            'PREVIEW' => 'https://www.bitrix24.com/images/b24_screen.png',
            'CONTENT' => '
    <section class="landing-block">
        <div class="text-center g-color-gray-dark-v3 g-pa-10">
            <div class="g-width-600 mx-auto">
                <div class="landing-block-node-text g-font-size-12 ">
                    <p>© 2017 All rights reserved. Developed by
                    <a href="#" class="landing-block-node-link g-color-primary">Bitrix24</a></p>
                </div>
            </div>
        </div>
    </section>'
        ),
        'manifest' => array(
            'assets' => array(
                'css' => array(
                    'https://site.com/aaa.css'
                ),
                'js' => array(
                    'https://site.com/aaa.js'
                )
            ),
            'nodes' =>
                array(
                    '.landing-block-node-text' =>
                        array(
                            'name' => 'Text',
                            'type' => 'text',
                        ),
                    '.landing-block-node-link' =>
                        array(
                            'name' => 'Link',
                            'type' => 'link',
                        ),
                ),
            'style' =>
                array(
                    '.landing-block-node-text' =>
                        array(
                            'name' => 'Text',
                            'type' => 'typo',
                        ),
                    '.landing-block-node-link' =>
                        array(
                            'name' => 'Link',
                            'type' => 'typo',
                        ),
                ),
            'attrs' =>
                array(
                '.landing-block-node-text' =>
                    array(
                        'name' => 'Copyright settings',
                        'type' => 'dropdown',
                        'attribute' => 'data-copy',
                        'items' => array(
                            'val1' => 'Value 1',
                            'val2' => 'Value 2'
                                )
                        ),
                ),
        ),
    );
    ?>
    // note! the following is JS code.
    BX24.callMethod(
        'landing.repo.register',
        //abstract method that converts PHP array to JS object
        <?= \CUtil::PhpToJSObject($data)?>,
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}