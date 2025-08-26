# Get the list of storage items entity.item.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

The method `entity.item.get` retrieves a list of storage items. It is a list method.

The user must have at least read access permission (**R**) to the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY^*^** | Required. String identifier of the storage. ||
|| **SORT** | Similar to the *arOrder* and *arFilter* parameters of the PHP method `CIBlockElement::GetList` (including filter operations and complex logic). ||
|| **FILTER** | Similar to the *arOrder* and *arFilter* parameters of the PHP method `CIBlockElement::GetList` (including filter operations and complex logic). ||
|| **start** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.get',
    		{
    			ENTITY: 'menu',
    			SORT: {
    				DATE_ACTIVE_FROM: 'ASC',
    				ID: 'ASC'
    			},
    			FILTER: {
    				'>=DATE_ACTIVE_FROM': dateStart,
    				'<DATE_ACTIVE_FROM': dateFinish
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	this.buildData(result);
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
                'entity.item.get',
                [
                    'ENTITY' => 'menu',
                    'SORT' => [
                        'DATE_ACTIVE_FROM' => 'ASC',
                        'ID' => 'ASC'
                    ],
                    'FILTER' => [
                        '>=DATE_ACTIVE_FROM' => $dateStart,
                        '<DATE_ACTIVE_FROM' => $dateFinish
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        $this->buildData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting entity items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.get',
        {
            ENTITY: 'menu',
            SORT: {
                DATE_ACTIVE_FROM: 'ASC',
                ID: 'ASC'
            },
            FILTER: {
                '>=DATE_ACTIVE_FROM': dateStart,
                '<DATE_ACTIVE_FROM': dateFinish
            }
        },
        $.proxy(
            this.buildData,
            this
        )
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.item.get.json?=&ENTITY=menu&FILTER%5B%3CDATE_ACTIVE_FROM%5D=2013-07-01T00%3A00%3A00.000Z&FILTER%5B%3E%3DDATE_ACTIVE_FROM%5D=2013-06-24T00%3A00%3A00.000Z&SORT%5BDATE_ACTIVE_FROM%5D=ASC&SORT%5BID%5D=ASC&auth=723867cdb1ada1de7870de8b0e558679
    ```

{% endlist %}

### Example of a call with a complex filter

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'entity.item.get',
    		{
    			ENTITY: 'menu',
    			SORT: {
    				DATE_ACTIVE_FROM: 'ASC',
    				ID: 'ASC'
    			},
    			FILTER: {
    				'1':{
    					'LOGIC':'OR',
    					'PROPERTY_MYPROP1':'value1',
    					'PROPERTY_MYPROP2':'value2'
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	// Your required data processing logic
    	processResult(result);
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
                'entity.item.get',
                [
                    'ENTITY' => 'menu',
                    'SORT' => [
                        'DATE_ACTIVE_FROM' => 'ASC',
                        'ID' => 'ASC'
                    ],
                    'FILTER' => [
                        '1' => [
                            'LOGIC' => 'OR',
                            'PROPERTY_MYPROP1' => 'value1',
                            'PROPERTY_MYPROP2' => 'value2'
                        ]
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
        echo 'Error getting entity items: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'entity.item.get',
        {
            ENTITY: 'menu',
            SORT: {
                DATE_ACTIVE_FROM: 'ASC',
                ID: 'ASC'
            },
            FILTER: {
                '1':{
                    'LOGIC':'OR',
                    'PROPERTY_MYPROP1':'value1',
                    'PROPERTY_MYPROP2':'value2'
                }
            }
        }
    );
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}

## Response in case of success

> 200 OK
```json
{
    "result":
    [
        {
            "ID":"838",
            "TIMESTAMP_X":"2013-06-25T15:06:47+02:00",
            "MODIFIED_BY":"1",
            "DATE_CREATE":"2013-06-25T15:06:47+02:00",
            "CREATED_BY":"1",
            "ACTIVE":"Y",
            "DATE_ACTIVE_FROM":"2013-07-01T03:00:00+02:00",
            "DATE_ACTIVE_TO":"",
            "SORT":"500",
            "NAME":"Buckwheat in the shell",
            "PREVIEW_PICTURE":null,
            "PREVIEW_TEXT":null,
            "DETAIL_PICTURE":null,
            "DETAIL_TEXT":null,
            "CODE":null,
            "ENTITY":"menu",
            "SECTION":null,
            "PROPERTY_VALUES":
            {
                "dish":"813",
                "price":"16"
            }
        }
    ],
    "total":1
}
```