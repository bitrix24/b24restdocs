# Update the current universal list lists.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.update` updates an existing list. If the list is successfully updated, the response is `true`, otherwise *Exception*.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | `id` of the information block type (required):
- **lists** - list information block type
- **bitrix_processes** - processes information block type
- **lists_socnet** - group lists information block type ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | code or `id` of the information block (required); ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); ||
|| **FIELDS**
[`unknown`](../../data-types.md) | information block fields:
- **NAME**^*^ - name of the information block (required);
- **DESCRIPTION** - description;
- **SORT** - sorting;
- **PICTURE** - image;
- **BIZPROC** - enabling support for workflows. ||
|| **MESSAGES**
[`unknown`](../../data-types.md) | labels for list items and sections; ||
|| **RIGHTS**
[`unknown`](../../data-types.md) | access permission management. ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.update',
    		{
    			'IBLOCK_TYPE_ID': 'lists_socnet',
    			'IBLOCK_CODE': 'rest_1',
    			'FIELDS': {
    				'NAME': 'List 1 (Update)',
    				'DESCRIPTION': 'Test list (Update)',
    				'SORT': '20',
    				'PICTURE': document.getElementById('iblock-image-update')
    			},
    			'RIGHTS': {
    				'G1': 'X'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		alert("Error: " + result.error());
    	else
    		alert("Success: " + result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP


    ```php
    try {
        $params = [
            'IBLOCK_TYPE_ID' => 'lists_socnet',
            'IBLOCK_CODE'    => 'rest_1',
            'FIELDS'         => [
                'NAME'        => 'List 1 (Update)',
                'DESCRIPTION' => 'Test list (Update)',
                'SORT'        => '20',
                'PICTURE'     => $_POST['iblock-image-update'], // Assuming this is coming from a form POST request
            ],
            'RIGHTS'         => [
                'G1' => 'X',
            ],
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'lists.update',
                $params
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists_socnet',
        'IBLOCK_CODE': 'rest_1',
        'FIELDS': {
            'NAME': 'List 1 (Update)',
            'DESCRIPTION': 'Test list (Update)',
            'SORT': '20',
            'PICTURE': document.getElementById('iblock-image-update')
        },
        'RIGHTS': {
            'G1': 'X'
        }
    };
    BX24.callMethod(
        'lists.update',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}