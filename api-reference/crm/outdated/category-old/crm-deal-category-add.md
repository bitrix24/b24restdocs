# Create a New Deal Direction crm.dealcategory.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [crm.category.add](../../universal/category/crm-category-add.md).

{% endnote %}

This method creates a new deal direction.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`array`](../../../data-types.md) | Field values for creating a deal direction.

To find out the required format for the fields, execute the method [`crm.dealcategory.fields`](./crm-deal-category-fields.md) and check the format of the returned field values.
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"NAME":"New Direction","SORT":"20"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.dealcategory.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"NAME":"New Direction","SORT":"20"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.dealcategory.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'crm.dealcategory.add',
            {
                fields:
                {
                    "NAME": "New Direction",
                    "SORT": "20"
                }
            }
        );
        
        const result = response.getData().result;
        console.info('Created direction with ID ' + result);
    }
    catch( error )
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
                'crm.dealcategory.add',
                [
                    'fields' => [
                        'NAME' => 'New Direction',
                        'SORT' => '20',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Created direction with ID ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating deal category: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.dealcategory.add",
        {
            fields:
            {
                "NAME": "New Direction",
                "SORT": "20"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Created direction with ID " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.dealcategory.add',
        [
            'fields' =>
            [
                'NAME' => 'New Direction',
                'SORT' => '20'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}