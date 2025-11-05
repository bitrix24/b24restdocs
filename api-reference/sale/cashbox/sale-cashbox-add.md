# Add Cash Register sale.cashbox.add

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to change settings")

This method adds a cash register.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../data-types.md) | Name of the cash register ||
|| **REST_CODE***
[`sale_cashbox_handler.CODE`](../data-types.md#sale_cashbox_handler) | REST handler code for the cash register. Specified when adding the handler in the method [sale.cashbox.handler.add](./sale-cashbox-handler-add.md) in the `CODE` parameter ||
|| **EMAIL***
[`string`](../../data-types.md) | Email address to which notifications will be sent in case of errors during receipt printing ||
|| **OFD**
[`string`](../../data-types.md) | OFD handler code. Available OFD handlers: 
- `bx_firstofd` — First OFD 
- `bx_platformaofd` — OFD Platform 
- `bx_yarusofd` — YARUS OFD
- `bx_taxcomofd` — Taxcom OFD 
- `bx_ofdruofd` — OFD.RU 
- `bx_tenzorofd` — Tensor OFD 
- `bx_conturofd` — Kontur OFD 

By default, without OFD
||
|| **OFD_SETTINGS**
[`object`](../../data-types.md) | OFD settings (detailed description provided [below](#ofd_settings)). 

By default, an empty array 
||
|| **NUMBER_KKM**
[`string`](../../data-types.md) | External identifier of the cash register.

By default, empty ||
|| **ACTIVE**
[`string`](../../data-types.md) | Activity status of the cash register. Possible values:
- `Y` — yes
- `N` — no
  
Default value: `N` ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting. Default `100` ||
|| **USE_OFFLINE**
[`string`](../../data-types.md) | Whether the cash register is used offline. Possible values:
- `Y` — yes
- `N` — no
  
Default value: `N`
||
|| **SETTINGS**
[`array`](../../data-types.md) | Cash register settings according to the settings structure passed in the `CONFIG` key of the `SETTINGS` field of the method [sale.cashbox.handler.add](./sale-cashbox-handler-add.md).

By default, empty ||
|#

### OFD_SETTINGS Parameter {#ofd_settings}

#|
|| **Name**
`type` | **Description** ||
|| **Settings for all OFDs** |  ||
|| **OFD_MODE**
[`object`](../../data-types.md) | Settings related to the OFD operation mode. The `IS_TEST` parameter is passed ([`string`](../../data-types.md) with values `Y/N`) — OFD operation mode: 
- `Y` — test mode 
- `N` — working mode ||
|| **Additional settings for OFD.RU** |  ||
|| **SELLER_INFO**
[`object`](../../data-types.md) | Settings for the "Seller Information" section. The required parameter `INN` ([`string`](../../data-types.md)) — seller's INN is passed
||
|| **Additional settings for YARUS OFD** |  ||
|| **AUTH**
[`object`](../../data-types.md) | Authorization settings. The `INN` parameter is passed ([`string`](../../data-types.md)) — security key
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"Rest-Cash Register","REST_CODE":"restcashbox01","EMAIL":"user@example.com","NUMBER_KKM":"123","ACTIVE":"Y","SORT":100,"OFD":"bx_ofdruofd","OFD_SETTINGS":{"OFD_MODE":{"IS_TEST":"N"}},"SETTINGS":{"AUTH":{"KEYWORD":"top_secret!","PREFERENCE":"SECOND"},"INTERACTION":{"MODE":"ACTIVE"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/sale.cashbox.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"Rest-Cash Register","REST_CODE":"restcashbox01","EMAIL":"user@example.com","NUMBER_KKM":"123","ACTIVE":"Y","SORT":100,"OFD":"bx_ofdruofd","OFD_SETTINGS":{"OFD_MODE":{"IS_TEST":"N"}},"SETTINGS":{"AUTH":{"KEYWORD":"top_secret!","PREFERENCE":"SECOND"},"INTERACTION":{"MODE":"ACTIVE"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.cashbox.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.cashbox.add",
    		{
    			"NAME": 'Rest-Cash Register',
    			"REST_CODE": 'restcashbox01',
    			"EMAIL": "user@example.com",
    			"NUMBER_KKM": "123",
    			"ACTIVE": "Y",
    			"SORT": 100,
    			"OFD": "bx_ofdruofd",
    			"OFD_SETTINGS":
    			{
    				"OFD_MODE":
    				{
    					"IS_TEST": "N"
    				}
    			},
    			"SETTINGS":
    			{
    				"AUTH":
    				{
    					"KEYWORD": "top_secret!",
    					"PREFERENCE": "SECOND"
    				},
    				"INTERACTION":
    				{
    					"MODE": "ACTIVE"
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
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
                'sale.cashbox.add',
                [
                    'NAME'       => 'Rest-Cash Register',
                    'REST_CODE'  => 'restcashbox01',
                    'EMAIL'      => 'user@example.com',
                    'NUMBER_KKM' => '123',
                    'ACTIVE'     => 'Y',
                    'SORT'       => 100,
                    'OFD'        => 'bx_ofdruofd',
                    'OFD_SETTINGS' => [
                        'OFD_MODE' => [
                            'IS_TEST' => 'N'
                        ]
                    ],
                    'SETTINGS' => [
                        'AUTH' => [
                            'KEYWORD'    => 'top_secret!',
                            'PREFERENCE' => 'SECOND'
                        ],
                        'INTERACTION' => [
                            'MODE' => 'ACTIVE'
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
        echo 'Error adding cash register: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.cashbox.add",
        {
            "NAME": 'Rest-Cash Register',
            "REST_CODE": 'restcashbox01',
            "EMAIL": "user@example.com",
            "NUMBER_KKM": "123",
            "ACTIVE": "Y",
            "SORT": 100,
            "OFD": "bx_ofdruofd",
            "OFD_SETTINGS":
            {
                "OFD_MODE":
                {
                    "IS_TEST": "N"
                }
            },
            "SETTINGS":
            {
                "AUTH":
                {
                    "KEYWORD": "top_secret!",
                    "PREFERENCE": "SECOND"
                },
                "INTERACTION":
                {
                    "MODE": "ACTIVE"
                }
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.cashbox.add',
        [
            'NAME' => 'Rest-Cash Register',
            'REST_CODE' => 'restcashbox01',
            'EMAIL' => 'user@example.com',
            'NUMBER_KKM' => '123',
            'ACTIVE' => 'Y',
            'SORT' => 100,
            'OFD' => 'bx_ofdruofd',
            'OFD_SETTINGS' =>
            [
                'OFD_MODE' =>
                [
                    'IS_TEST' => 'N'
                ]
            ],
            'SETTINGS' =>
            [
                'AUTH' =>
                [
                    'KEYWORD' => 'top_secret!',
                    'PREFERENCE' => 'SECOND'
                ],
                'INTERACTION' =>
                [
                    'MODE' => 'ACTIVE'
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../../tutorials/sale/cashbox-add-example.md)

{% endnote %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 5,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_cashbox.ID`](../data-types.md#sale_cashbox) | Identifier of the added cash register ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "ERROR_CHECK_FAILURE",
    "error_description": "Parameter NAME is not defined"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient rights to add a cash register | 403 ||
|| `ERROR_CHECK_FAILURE` | A required field value is not specified or the value of one of the fields is incorrect | 400 ||
|| `ERROR_CASHBOX_ADD` | Other errors. More detailed information about the error can be found in `error_description` | 400 ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-cashbox-handler-add.md)
- [{#T}](./sale-cashbox-handler-update.md)
- [{#T}](./sale-cashbox-handler-list.md)
- [{#T}](./sale-cashbox-handler-delete.md)
- [{#T}](./sale-cashbox-update.md)
- [{#T}](./sale-cashbox-list.md)
- [{#T}](./sale-cashbox-delete.md)
- [{#T}](./sale-cashbox-check-apply.md)
- [{#T}](../../../tutorials/sale/cashbox-add-example.md)