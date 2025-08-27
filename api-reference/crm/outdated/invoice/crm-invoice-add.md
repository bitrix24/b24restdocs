# Add Invoice crm.invoice.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method creates a new invoice.

If you need to specify any details of the buyer/seller in the invoice (since there can be multiple for a company), use the method [crm.requisite.link.register](../../requisites/links/crm-requisite-link-register.md).

The created invoice must include the seller and buyer companies:
- `UF_COMPANY_ID`, if the buyer is a company or `UF_CONTACT_ID`, if the buyer is a contact 
- `UF_MYCOMPANY_ID` - seller 

The identifiers specified in **crm.requisite.link.register** and in the created invoice must correspond to the buyer and seller.

## Method Parameters

{% include [Note about required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`array`](../../../data-types.md) | Field values for creating the invoice.

To find out the required format of the fields, execute the method [crm.invoice.fields](./crm-invoice-fields.md) and check the format of the returned values for these fields ||
|#

## Code Examples

{% include [Note about examples](../../../../_includes/examples.md) %}

### Example 1

{% list tabs %}

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ORDER_TOPIC":"Invoice for legal entity","STATUS_ID":"P","DATE_INSERT":"'$(date -Iseconds --utc --date='TZ="Europe/Berlin" now')'","PAY_VOUCHER_DATE":"'$(date -Iseconds --utc --date='TZ="Europe/Berlin" now')'","PAY_VOUCHER_NUM":"876","DATE_MARKED":"'$(date -Iseconds --utc --date='TZ="Europe/Berlin" now')'","REASON_MARKED":"Invoice paid immediately.","COMMENTS":"manager's comment","USER_DESCRIPTION":"comment for the client","DATE_BILL":"'$(date -Iseconds --utc --date='TZ="Europe/Berlin" now')'","DATE_PAY_BEFORE":"'$(date -Iseconds --utc --date='TZ="Europe/Berlin" +1 month')'","RESPONSIBLE_ID":1,"UF_DEAL_ID":10,"UF_COMPANY_ID":5,"UF_CONTACT_ID":2,"PERSON_TYPE_ID":2,"PAY_SYSTEM_ID":6,"INVOICE_PROPERTIES":{"COMPANY":"Ltd. \"New Technologies\"","COMPANY_ADR":"543000 Berlin, Peschanskaya St., 15, office 55 (legal)","INN":"","KPP":"","CONTACT_PERSON":"Boris Sokolov","EMAIL":"pr@logistics-north.com","PHONE":"+1 (495) 234-54-32","FAX":"","ZIP":"","CITY":"","LOCATION":"","ADDRESS":""},"PRODUCT_ROWS":[{"ID":0,"PRODUCT_ID":438,"PRODUCT_NAME":"Product 01","QUANTITY":1,"PRICE":100},{"ID":0,"PRODUCT_ID":515,"PRODUCT_NAME":"Product 77","QUANTITY":1,"PRICE":118}]}}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.add?auth=**put_access_token_here**
    ```

- JS

    ```js
    try
    {
    	const current = new Date();
    	const nextMonth = new Date();
    	nextMonth.setMonth(current.getMonth() + 1);
    
    	const date2str = function(d)
    	{
    		return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+02:00';
    	};
    
    	const paddatepart = function(part)
    	{
    		return part >= 10 ? part.toString() : '0' + part.toString();
    	};
    
    	const response = await $b24.callMethod(
    		"crm.invoice.add",
    		{
    			"fields": {
    				"ORDER_TOPIC": "Invoice for legal entity",
    				"STATUS_ID": "P",
    				"DATE_INSERT": date2str(current),
    				"PAY_VOUCHER_DATE": date2str(current),
    				"PAY_VOUCHER_NUM": "876",
    				"DATE_MARKED": date2str(current),
    				"REASON_MARKED": "Invoice paid immediately.",
    				"COMMENTS": "manager's comment",
    				"USER_DESCRIPTION": "comment for the client",
    				"DATE_BILL": date2str(current),
    				"DATE_PAY_BEFORE": date2str(nextMonth),
    				"RESPONSIBLE_ID": 1,
    				"UF_DEAL_ID": 10,
    				"UF_COMPANY_ID": 5,
    				"UF_CONTACT_ID": 2,
    				"PERSON_TYPE_ID": 2,
    				"PAY_SYSTEM_ID": 6,
    				"INVOICE_PROPERTIES": {
    					"COMPANY": "Ltd. \"New Technologies\"",
    					"COMPANY_ADR": "543000 Berlin, Peschanskaya St., 15, office 55 (legal)",
    					"INN": "",
    					"KPP": "",
    					"CONTACT_PERSON": "Boris Sokolov",
    					"EMAIL": "pr@logistics-north.com",
    					"PHONE": "+1 (495) 234-54-32",
    					"FAX": "",
    					"ZIP": "",
    					"CITY": "",
    					"LOCATION": "",
    					"ADDRESS": ""
    				},
    				"PRODUCT_ROWS": [
    					{"ID": 0, "PRODUCT_ID": 438, "PRODUCT_NAME": "Product 01", "QUANTITY": 1, "PRICE": 100},
    					{"ID": 0, "PRODUCT_ID": 515, "PRODUCT_NAME": "Product 77", "QUANTITY": 1, "PRICE": 118}
    				]
    			}
    		}
    	);
    
    	const result = response.getData().result;
    	console.info("Invoice created with ID " + result);
    }
    catch(error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $current = new DateTime();
        $nextMonth = new DateTime();
        $nextMonth->setDate($current->format('Y'), $current->format('m') + 1, $current->format('d'));
    
        $date2str = function($d) {
            return $d->format('Y-m-d\TH:i:sP');
        };
    
        $paddatepart = function($part) {
            return $part >= 10 ? $part : '0' . $part;
        };
    
        $response = $b24Service
            ->core
            ->call(
                "crm.invoice.add",
                [
                    "fields" => [
                        "ORDER_TOPIC"       => "Invoice for legal entity",
                        "STATUS_ID"         => "P",
                        "DATE_INSERT"       => $date2str($current),
                        "PAY_VOUCHER_DATE"  => $date2str($current),
                        "PAY_VOUCHER_NUM"   => "876",
                        "DATE_MARKED"       => $date2str($current),
                        "REASON_MARKED"     => "Invoice paid immediately.",
                        "COMMENTS"          => "manager's comment",
                        "USER_DESCRIPTION"  => "comment for the client",
                        "DATE_BILL"         => $date2str($current),
                        "DATE_PAY_BEFORE"   => $date2str($nextMonth),
                        "RESPONSIBLE_ID"    => 1,
                        "UF_DEAL_ID"        => 10,
                        "UF_COMPANY_ID"     => 5,
                        "UF_CONTACT_ID"     => 2,
                        "PERSON_TYPE_ID"    => 2,
                        "PAY_SYSTEM_ID"     => 6,
                        "INVOICE_PROPERTIES" => [
                            "COMPANY"        => "Ltd. \"New Technologies\"",
                            "COMPANY_ADR"    => "543000 Berlin, Peschanskaya St., 15, office 55 (legal)",
                            "INN"            => "",
                            "KPP"            => "",
                            "CONTACT_PERSON" => "Boris Sokolov",
                            "EMAIL"          => "pr@logistics-north.com",
                            "PHONE"          => "+1 (495) 234-54-32",
                            "FAX"            => "",
                            "ZIP"            => "",
                            "CITY"           => "",
                            "LOCATION"       => "",
                            "ADDRESS"        => ""
                        ],
                        "PRODUCT_ROWS" => [
                            ["ID" => 0, "PRODUCT_ID" => 438, "PRODUCT_NAME" => "Product 01", "QUANTITY" => 1, "PRICE" => 100],
                            ["ID" => 0, "PRODUCT_ID" => 515, "PRODUCT_NAME" => "Product 77", "QUANTITY" => 1, "PRICE" => 118]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Invoice created with ID ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating invoice: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var current = new Date();
    var nextMonth = new Date();
    nextMonth.setMonth(current.getMonth() + 1);

    var date2str = function(d)
    {
        return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+02:00';
    };

    var paddatepart = function(part)
    {
        return part >= 10 ? part.toString() : '0' + part.toString();
    };

    BX24.callMethod(
        "crm.invoice.add",
        {
            "fields": {
                "ORDER_TOPIC": "Invoice for legal entity",
                "STATUS_ID": "P",
                "DATE_INSERT": date2str(current),
                "PAY_VOUCHER_DATE": date2str(current),
                "PAY_VOUCHER_NUM": "876",
                "DATE_MARKED": date2str(current),
                "REASON_MARKED": "Invoice paid immediately.",
                "COMMENTS": "manager's comment",
                "USER_DESCRIPTION": "comment for the client",
                "DATE_BILL": date2str(current),
                "DATE_PAY_BEFORE": date2str(nextMonth),
                "RESPONSIBLE_ID": 1,
                "UF_DEAL_ID": 10,
                "UF_COMPANY_ID": 5,
                "UF_CONTACT_ID": 2,
                "PERSON_TYPE_ID": 2,
                "PAY_SYSTEM_ID": 6,
                "INVOICE_PROPERTIES": {
                    "COMPANY": "Ltd. \"New Technologies\"",
                    "COMPANY_ADR": "543000 Berlin, Peschanskaya St., 15, office 55 (legal)",
                    "INN": "",
                    "KPP": "",
                    "CONTACT_PERSON": "Boris Sokolov",
                    "EMAIL": "pr@logistics-north.com",
                    "PHONE": "+1 (495) 234-54-32",
                    "FAX": "",
                    "ZIP": "",
                    "CITY": "",
                    "LOCATION": "",
                    "ADDRESS": ""
                },
                "PRODUCT_ROWS": [
                    {"ID": 0, "PRODUCT_ID": 438, "PRODUCT_NAME": "Product 01", "QUANTITY": 1, "PRICE": 100},
                    {"ID": 0, "PRODUCT_ID": 515, "PRODUCT_NAME": "Product 77", "QUANTITY": 1, "PRICE": 118}
                ]
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Invoice created with ID " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $current = new DateTime();
    $nextMonth = (new DateTime())->modify('+1 month');

    function date2str($d) {
        return $d->format('Y-m-d\TH:i:s+02:00');
    }

    $result = CRest::call(
        'crm.invoice.add',
        [
            'fields' => [
                'ORDER_TOPIC' => 'Invoice for legal entity',
                'STATUS_ID' => 'P',
                'DATE_INSERT' => date2str($current),
                'PAY_VOUCHER_DATE' => date2str($current),
                'PAY_VOUCHER_NUM' => '876',
                'DATE_MARKED' => date2str($current),
                'REASON_MARKED' => 'Invoice paid immediately.',
                'COMMENTS' => 'manager\'s comment',
                'USER_DESCRIPTION' => 'comment for the client',
                'DATE_BILL' => date2str($current),
                'DATE_PAY_BEFORE' => date2str($nextMonth),
                'RESPONSIBLE_ID' => 1,
                'UF_DEAL_ID' => 8,
                'UF_COMPANY_ID' => 0,
                'UF_CONTACT_ID' => 3,
                'PERSON_TYPE_ID' => 1,
                'PAY_SYSTEM_ID' => 6,
                'INVOICE_PROPERTIES' => [
                    'FIO' => 'Gleb Titov',
                    'EMAIL' => 'boss@yt-soft.net',
                    'PHONE' => '',
                    'ZIP' => '',
                    'CITY' => '',
                    'LOCATION' => '',
                    'ADDRESS' => ''
                ],
                'PRODUCT_ROWS' => [
                    ['ID' => 0, 'PRODUCT_ID' => 438, 'PRODUCT_NAME' => 'Product 01', 'QUANTITY' => 1, 'PRICE' => 100],
                    ['ID' => 0, 'PRODUCT_ID' => 515, 'PRODUCT_NAME' => 'Product 77', 'QUANTITY' => 1, 'PRICE' => 118]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Example 2

{% list tabs %}

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ORDER_TOPIC":"Invoice for individual","STATUS_ID":"P","DATE_INSERT":"'$(date -Iseconds --utc --date='TZ="Europe/Berlin" now')'","PAY_VOUCHER_DATE":"'$(date -Iseconds --utc --date='TZ="Europe/Berlin" now')'","PAY_VOUCHER_NUM":"876","DATE_MARKED":"'$(date -Iseconds --utc --date='TZ="Europe/Berlin" now')'","REASON_MARKED":"paid","COMMENTS":"comment","USER_DESCRIPTION":"comment to the client","DATE_BILL":"'$(date -Iseconds --utc --date='TZ="Europe/Berlin" now')'","DATE_PAY_BEFORE":"'$(date -Iseconds --utc --date='TZ="Europe/Berlin" +1 month')'","RESPONSIBLE_ID":1,"UF_DEAL_ID":8,"UF_COMPANY_ID":0,"UF_CONTACT_ID":3,"PERSON_TYPE_ID":1,"PAY_SYSTEM_ID":6,"INVOICE_PROPERTIES":{"FIO":"Gleb Titov","EMAIL":"boss@yt-soft.net","PHONE":"","ZIP":"","CITY":"","LOCATION":"","ADDRESS":""},"PRODUCT_ROWS":[{"ID":0,"PRODUCT_ID":438,"PRODUCT_NAME":"Product 01","QUANTITY":1,"PRICE":100},{"ID":0,"PRODUCT_ID":515,"PRODUCT_NAME":"Product 77","QUANTITY":1,"PRICE":118}]}}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.add?auth=**put_access_token_here**
    ```

- JS

    ```js
    try
    {
    	const current = new Date();
    	const nextMonth = new Date();
    	nextMonth.setMonth(current.getMonth() + 1);
    
    	const date2str = function(d)
    	{
    		return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+02:00';
    	};
    
    	const paddatepart = function(part)
    	{
    		return part >= 10 ? part.toString() : '0' + part.toString();
    	};
    
    	const response = await $b24.callMethod(
    		"crm.invoice.add",
    		{
    			"fields": {
    				"ORDER_TOPIC": "Invoice for individual",
    				"STATUS_ID": "P",
    				"DATE_INSERT": date2str(current),
    				"PAY_VOUCHER_DATE": date2str(current),
    				"PAY_VOUCHER_NUM": "876",
    				"DATE_MARKED": date2str(current),
    				"REASON_MARKED": "paid",
    				"COMMENTS": "comment",
    				"USER_DESCRIPTION": "comment to the client",
    				"DATE_BILL": date2str(current),
    				"DATE_PAY_BEFORE": date2str(nextMonth),
    				"RESPONSIBLE_ID": 1,
    				"UF_DEAL_ID": 8,
    				"UF_COMPANY_ID": 0,
    				"UF_CONTACT_ID": 3,
    				"PERSON_TYPE_ID": 1,
    				"PAY_SYSTEM_ID": 6,
    				"INVOICE_PROPERTIES": {
    					"FIO": "Gleb Titov",
    					"EMAIL": "boss@yt-soft.net",
    					"PHONE": "",
    					"ZIP": "",
    					"CITY": "",
    					"LOCATION": "",
    					"ADDRESS": ""
    				},
    				"PRODUCT_ROWS": [
    					{"ID": 0, "PRODUCT_ID": 438, "PRODUCT_NAME": "Product 01", "QUANTITY": 1, "PRICE": 100},
    					{"ID": 0, "PRODUCT_ID": 515, "PRODUCT_NAME": "Product 77", "QUANTITY": 1, "PRICE": 118}
    				]
    			}
    		}
    	);
    
    	const result = response.getData().result;
    	console.info("Invoice created with ID " + result);
    }
    catch(error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $current = new DateTime();
        $nextMonth = new DateTime();
        $nextMonth->setDate($current->format('Y'), $current->format('m') + 1, $current->format('d'));
    
        $date2str = function($d) {
            return $d->format('Y-m-d\TH:i:sP');
        };
    
        $paddatepart = function($part) {
            return $part >= 10 ? $part : '0' . $part;
        };
    
        $response = $b24Service
            ->core
            ->call(
                'crm.invoice.add',
                [
                    'fields' => [
                        'ORDER_TOPIC'       => 'Invoice for individual',
                        'STATUS_ID'         => 'P',
                        'DATE_INSERT'       => $date2str($current),
                        'PAY_VOUCHER_DATE'  => $date2str($current),
                        'PAY_VOUCHER_NUM'   => '876',
                        'DATE_MARKED'       => $date2str($current),
                        'REASON_MARKED'     => 'paid',
                        'COMMENTS'          => 'comment',
                        'USER_DESCRIPTION'  => 'comment to the client',
                        'DATE_BILL'         => $date2str($current),
                        'DATE_PAY_BEFORE'   => $date2str($nextMonth),
                        'RESPONSIBLE_ID'    => 1,
                        'UF_DEAL_ID'        => 8,
                        'UF_COMPANY_ID'     => 0,
                        'UF_CONTACT_ID'     => 3,
                        'PERSON_TYPE_ID'    => 1,
                        'PAY_SYSTEM_ID'     => 6,
                        'INVOICE_PROPERTIES' => [
                            'FIO'      => 'Gleb Titov',
                            'EMAIL'    => 'boss@yt-soft.net',
                            'PHONE'    => '',
                            'ZIP'      => '',
                            'CITY'     => '',
                            'LOCATION' => '',
                            'ADDRESS'  => '',
                        ],
                        'PRODUCT_ROWS' => [
                            ['ID' => 0, 'PRODUCT_ID' => 438, 'PRODUCT_NAME' => 'Product 01', 'QUANTITY' => 1, 'PRICE' => 100],
                            ['ID' => 0, 'PRODUCT_ID' => 515, 'PRODUCT_NAME' => 'Product 77', 'QUANTITY' => 1, 'PRICE' => 118],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Invoice created with ID ' . $result->data();
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating invoice: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var current = new Date();
    var nextMonth = new Date();
    nextMonth.setMonth(current.getMonth() + 1);

    var date2str = function(d)
    {
        return d.getFullYear() + '-' + paddatepart(1 + d.getMonth()) + '-' + paddatepart(d.getDate()) + 'T' + paddatepart(d.getHours()) + ':' + paddatepart(d.getMinutes()) + ':' + paddatepart(d.getSeconds()) + '+02:00';
    };

    var paddatepart = function(part)
    {
        return part >= 10 ? part.toString() : '0' + part.toString();
    };

    BX24.callMethod(
        "crm.invoice.add",
        {
            "fields": {
                "ORDER_TOPIC": "Invoice for individual",
                "STATUS_ID": "P",
                "DATE_INSERT": date2str(current),
                "PAY_VOUCHER_DATE": date2str(current),
                "PAY_VOUCHER_NUM": "876",
                "DATE_MARKED": date2str(current),
                "REASON_MARKED": "paid",
                "COMMENTS": "comment",
                "USER_DESCRIPTION": "comment to the client",
                "DATE_BILL": date2str(current),
                "DATE_PAY_BEFORE": date2str(nextMonth),
                "RESPONSIBLE_ID": 1,
                "UF_DEAL_ID": 8,
                "UF_COMPANY_ID": 0,
                "UF_CONTACT_ID": 3,
                "PERSON_TYPE_ID": 1,
                "PAY_SYSTEM_ID": 6,
                "INVOICE_PROPERTIES": {
                    "FIO": "Gleb Titov",
                    "EMAIL": "boss@yt-soft.net",
                    "PHONE": "",
                    "ZIP": "",
                    "CITY": "",
                    "LOCATION": "",
                    "ADDRESS": ""
                },
                "PRODUCT_ROWS": [
                    {"ID": 0, "PRODUCT_ID": 438, "PRODUCT_NAME": "Product 01", "QUANTITY": 1, "PRICE": 100},
                    {"ID": 0, "PRODUCT_ID": 515, "PRODUCT_NAME": "Product 77", "QUANTITY": 1, "PRICE": 118}
                ]
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Invoice created with ID " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $current = new DateTime();
    $nextMonth = (new DateTime())->modify('+1 month');

    function date2str($d) {
        return $d->format('Y-m-d\TH:i:s+02:00');
    }

    $result = CRest::call(
        'crm.invoice.add',
        [
            'fields' => [
                'ORDER_TOPIC' => 'Invoice for individual',
                'STATUS_ID' => 'P',
                'DATE_INSERT' => date2str($current),
                'PAY_VOUCHER_DATE' => date2str($current),
                'PAY_VOUCHER_NUM' => '876',
                'DATE_MARKED' => date2str($current),
                'REASON_MARKED' => 'paid',
                'COMMENTS' => 'comment',
                'USER_DESCRIPTION' => 'comment to the client',
                'DATE_BILL' => date2str($current),
                'DATE_PAY_BEFORE' => date2str($nextMonth),
                'RESPONSIBLE_ID' => 1,
                'UF_DEAL_ID' => 8,
                'UF_COMPANY_ID' => 0,
                'UF_CONTACT_ID' => 3,
                'PERSON_TYPE_ID' => 1,
                'PAY_SYSTEM_ID' => 6,
                'INVOICE_PROPERTIES' => [
                    'FIO' => 'Gleb Titov',
                    'EMAIL' => 'boss@yt-soft.net',
                    'PHONE' => '',
                    'ZIP' => '',
                    'CITY' => '',
                    'LOCATION' => '',
                    'ADDRESS' => ''
                ],
                'PRODUCT_ROWS' => [
                    ['ID' => 0, 'PRODUCT_ID' => 438, 'PRODUCT_NAME' => 'Product 01', 'QUANTITY' => 1, 'PRICE' => 100],
                    ['ID' => 0, 'PRODUCT_ID' => 515, 'PRODUCT_NAME' => 'Product 77', 'QUANTITY' => 1, 'PRICE' => 118]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}