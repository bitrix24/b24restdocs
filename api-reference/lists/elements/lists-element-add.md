# Create List Element lists.element.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created are not provided

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.element.add` creates a list element. If the element is created successfully, the response is `true`, otherwise *Exception*.

To upload files in a "File" type field, pass the content in [Base64](../../files/how-to-upload-files.md) format.

To upload files in a "File (Drive)" type field, you need to:

1. use the REST API of the disk module: disk.folder.uploadfile and disk.storage.uploadfile. In the response when uploading these files, you will receive `"FILE_ID": 290`.
2. Get the list of uploaded file `IDs`.
3. Then, using the REST API of the lists module, add files to the required field:

## Parameters

#|
|| **Parameter** | **Description** ||
|| **IBLOCK_TYPE_ID**^*^
[`unknown`](../../data-types.md) | Identifier of the information block type (required):
- **lists** - list information block type
- **bitrix_processes** - processes information block type
- **lists_socnet** - group lists information block type ||
|| **IBLOCK_CODE/IBLOCK_ID**^*^
[`unknown`](../../data-types.md) | Code or `id` of the information block (required) ||
|| **ELEMENT_CODE**^*^
[`unknown`](../../data-types.md) | Code of the information block element (required) ||
|| **LIST_ELEMENT_URL**
[`unknown`](../../data-types.md) | Template address to list elements ||
|| **FIELDS**
[`unknown`](../../data-types.md) | Array of fields and values. In the File type field `F`, you cannot pass the file ID from Drive ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.element.add',
    		{
    			'IBLOCK_TYPE_ID': 'lists_socnet',
    			'IBLOCK_CODE': 'rest_1',
    			'ELEMENT_CODE': 'element_1',
    			'LIST_ELEMENT_URL': '#list_id#/element/#section_id#/#element_id#/',
    			'FIELDS': {
    				'NAME': 'Test element',
    				'PROPERTY_62': 'Text string',
    				'PROPERTY_63': {
    					'0': '7',
    					'1': '9',
    					'2': '10'
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		alert("Error: " + result.error());
    	else
    		alert("Success: " + result.data());
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
            'IBLOCK_TYPE_ID'   => 'lists_socnet',
            'IBLOCK_CODE'      => 'rest_1',
            'ELEMENT_CODE'     => 'element_1',
            'LIST_ELEMENT_URL' => '#list_id#/element/#section_id#/#element_id#/',
            'FIELDS'           => [
                'NAME'       => 'Test element',
                'PROPERTY_62' => 'Text string',
                'PROPERTY_63' => [
                    '0' => '7',
                    '1' => '9',
                    '2' => '10'
                ]
            ]
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'lists.element.add',
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
        echo 'Error adding list element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists_socnet',
        'IBLOCK_CODE': 'rest_1',
        'ELEMENT_CODE': 'element_1',
        'LIST_ELEMENT_URL': '#list_id#/element/#section_id#/#element_id#/',
        'FIELDS': {
            'NAME': 'Test element',
            'PROPERTY_62': 'Text string',
            'PROPERTY_63': {
                '0': '7',
                '1': '9',
                '2': '10'
            }
        }
    };
    BX24.callMethod(
        'lists.element.add',
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

Example of adding a file:

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.element.add',
    		{
    			'IBLOCK_TYPE_ID': 'lists',
    			'IBLOCK_ID': '41',
    			'ELEMENT_CODE': 'element1',
    			'FIELDS': {
    				'NAME': 'Test element 1',
    				'PROPERTY_122': document.getElementById('fileInputId') // PROPERTY_122 - Custom property of type "File"
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
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID'      => '41',
            'ELEMENT_CODE'   => 'element1',
            'FIELDS'         => [
                'NAME'       => 'Test element 1',
                'PROPERTY_122' => $_POST['fileInputId'] // PROPERTY_122 - Custom property of type "File"
            ]
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'lists.element.add',
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
        echo 'Error adding list element: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists',
        'IBLOCK_ID': '41',
        'ELEMENT_CODE': 'element1',
        'FIELDS': {
            'NAME': 'Test element 1',
            'PROPERTY_122': document.getElementById('fileInputId') // PROPERTY_122 - Custom property of type "File"
        }
    };
    BX24.callMethod(
        'lists.element.add',
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

{% include [Example Notes](../../../_includes/examples.md) %}