# Get the list of page blocks landing.block.getlist

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.block.getlist` retrieves a list of page blocks. It returns an array of blocks or an error.

## Parameters

#|
|| **Method** | **Description** | **Version** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the page, or an array of identifiers. | ||
|| **params**
[`unknown`](../../../data-types.md) | Parameters:
- **edit_mode** - Editing mode (1) or not (0 - default), will return a different set of blocks. Note that if you have not [published the page](../../page/methods/landing-landing-publication.md), nothing will be returned in mode 0.
- **deleted** - deleted (1) or not (0) blocks, by default all non-deleted blocks are displayed. In `edit_mode=0`, there cannot be any deleted blocks. | ||
|#

## Examples

{% list tabs %}

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.block.getlist',
                [
                    'lid' => 313,
                    'params' => [
                        'edit_mode' => 0
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting block list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.getlist',
        {
            lid: 313,
            params: {
                edit_mode: 0
            }
        },
        function(result)
        {
            if(result.error())
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

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}