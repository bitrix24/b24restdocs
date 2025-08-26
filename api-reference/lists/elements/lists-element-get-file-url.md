# Get File URL Path lists.element.get.file.url

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `lists.element.get.file.url` returns the path to the file. On success, it will return an array with a list of URLs for the required field of type File or File (Drive).

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **params**^*^
[`unknown`](../../data-types.md) | All fields are required
- IBLOCK_TYPE_ID - `id` of the information block type:
    - lists - list information block type;
    - bitrix_processes - processes information block type;
    - lists_socnet - group lists information block type. In this case, the parameter SOCNET_GROUP_ID - `id` of the group must be passed.
- IBLOCK_ID - `id` of the information block
- ELEMENT_ID - `id` of the element
- FIELD_ID - `id` of the field (property of the information block) File or File (Drive), without the prefix `"PROPERTY_"`
- SEF_FOLDER: '/my_section/lists/' - path to the folder with which the component works. This parameter is optional. By default, the value will be selected based on one of the system information block types. | ||
|| **SOCNET_GROUP_ID**^*^
[`unknown`](../../data-types.md) | `id` of the group (required if the list is created for a group); | ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'lists.element.get.file.url',
    		{
    			'IBLOCK_TYPE_ID': 'lists',
    			'IBLOCK_ID': '41',
    			'ELEMENT_ID': '683',
    			'FIELD_ID': '120'
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	alert("Error: " + error);
    }
    ```

- PHP

    ```php
    try {
        $params = [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID'      => '41',
            'ELEMENT_ID'     => '683',
            'FIELD_ID'       => '120',
        ];
    
        $response = $b24Service
            ->core
            ->call(
                'lists.element.get.file.url',
                $params
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling lists.element.get.file.url: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var params = {
        'IBLOCK_TYPE_ID': 'lists',
        'IBLOCK_ID': '41',
        'ELEMENT_ID': '683',
        'FIELD_ID': '120'
    };
    BX24.callMethod(
        'lists.element.get.file.url',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                console.log(result.data());
        }
    )
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}