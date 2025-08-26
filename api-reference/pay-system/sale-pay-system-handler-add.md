# Add REST handler for payment system sale.paysystem.handler.add

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow to modify settings")

This method adds a REST handler for the payment system.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../data-types.md) | Name of the REST handler ||
|| **SORT**
[`integer`](../data-types.md) | Sorting. Default is `100` ||
|| **CODE***
[`string`](../data-types.md) | Code of the REST handler. Must be unique among all handlers ||
|| **SETTINGS***
[`object`](../data-types.md) | Handler settings (detailed description provided [below](#parametr-settings)) ||
|#

### SETTINGS Parameter

{% include [Note on required parameters](../../_includes/required.md) %}

Depending on the operating mode used, one of the following parameters must be present: `FORM_DATA`, `CHECKOUT_DATA`, `IFRAME_DATA`.

#|
|| **Name**
`type` | **Description** ||
|| **CODES***
[`object`](../data-types.md) | List of handler parameters. The keys are parameter codes (`string`), and the values are parameter descriptions (detailed description provided [below](#parametr-codes)).

Parameter values will be available to the administrator for filling in the settings of the created payment system. They can be specified when adding the payment system in the method [sale.paysystem.add](./sale-pay-system-add.md) in the `SETTINGS` parameter and modified using the method [sale.paysystem.settings.update](./sale-pay-system-settings-update.md) ||
|| **FORM_DATA**
[`object`](../data-types.md) | Form settings when using the operating mode in [form](#form) ||
|| **CHECKOUT_DATA**
[`object`](../data-types.md) | Settings for the [Checkout](#checkout) mode (creating an order on the service side and redirecting the customer to this page for payment) ||
|| **IFRAME_DATA**
[`object`](../data-types.md) | Settings for the page displayed in the [iframe](#iframe) on the seller's site on the payment page ||
|| **CLIENT_TYPE**
[`string`](../data-types.md) | Type of customers with whom the handler can work. Available values:
- `b2c` — individuals
- `b2b` — legal entities

Default value is `b2c`
||
|| **CURRENCY**
[`crm_currency.CURRENCY[]`](../crm/data-types.md) | List of currencies supported by the payment system. Default is empty ||
|#

### CODES Parameter

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../data-types.md) | Name of the parameter ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the parameter ||
|| **SORT**
[`int`](../data-types.md) | Sorting ||
|| **GROUP**
[`string`](../data-types.md) | Code of the group to which the parameter belongs ||
|| **DEFAULT**
[`object`](../data-types.md) | Description of the default value (detailed description provided [below](#parametr-default))
||
|| **INPUT**
[`object`](../data-types.md) | Object describing the input field. The structure of the object contains the `TYPE` parameter — the type of the field. Supported fields: 
- `STRING` — string
- `Y/N` — checkbox
- `ENUM` — list
 ||
|#

### DEFAULT Parameter

#|
|| **Name**
`type` | **Description** ||
|| **PROVIDER_KEY**
[`string`](../data-types.md) | Key of the provider from which the default value will be taken. Possible values of the key are provided [below](#vozmozhnye-znacheniya-klyucha-provider_key) ||
|| **PROVIDER_VALUE**
[`string`](../data-types.md) | Code of the value that will be taken from the provider. Possible values of the key are provided [below](#vozmozhnye-znacheniya-klyucha-provider_value) ||
|#

### Possible values for the PROVIDER_KEY

#|
|| **Name** | **Description** ||
|| **ORDER** | Order ||
|| **PROPERTY** | Invoice properties ||
|| **PAYMENT** | Payment ||
|| **USER** | User ||
|| **VALUE** | Arbitrary string value ||
|| **Y\N** | Checkbox ||
|#

### Possible values for the PROVIDER_VALUE

#|
|| **Name** | **Description** ||
|| **ORDER** | 
- `ID` — identifier (for invoices corresponds to `ID` of the invoice)
- `ACCOUNT_NUMBER` — order number (for invoices corresponds to the invoice number)
- `ORDER_TOPIC` — subject
- `DATE_INSERT` — order date (for invoices corresponds to the invoice date)
- `DATE_INSERT_DATE` — order date without time (for invoices corresponds to the invoice date)
- `DATE_BILL` — date and time of issuance
- `DATE_BILL_DATE` — date of issuance
- `DATE_PAY_BEFORE` — payment deadline
- `SHOULD_PAY` — invoice amount (for invoices corresponds to the invoice amount)
- `CURRENCY` — currency
- `PRICE` — order cost (for invoices corresponds to the invoice cost)
- `PRICE_DELIVERY` — delivery cost
- `DISCOUNT_VALUE` — discount amount
- `USER_ID` — customer code
- `PAY_SYSTEM_ID` — payment system code
- `DELIVERY_ID` — delivery service code
- `TAX_VALUE` — tax
- `USER_DESCRIPTION` — comment
||
|| **PAYMENT** | 
- `ID` — identifier
- `ACCOUNT_NUMBER` — payment number
- `DATE_BILL` — date and time of issuance
- `DATE_BILL_DATE` — date of issuance without time
- `SUM` — invoice amount
- `CURRENCY` — currency
- `PAID` — paid
- `DATE_PAID` — payment date
- `PAY_SYSTEM_ID` — payment system code
- `PAY_VOUCHER_NUM` — voucher number
- `PAY_VOUCHER_DATE` — voucher date
- `DATE_PAY_BEFORE` — pay by
- `XML_ID` — XML identifier
- `PAY_SYSTEM_NAME` — name of the payment system
- `COMPANY_ID` — company code
- `PAY_RETURN_NUM` — return number
- `PAY_RETURN_DATE` — return date
- `PAY_RETURN_COMMENT` — return comment
||
|| **USER** | 
- `ID` — customer code,
- `LOGIN` — login
- `NAME` — first name
- `SECOND_NAME` — middle name
- `LAST_NAME` — last name
- `EMAIL` — EMail
- `PERSONAL_PROFESSION` — profession
- `PERSONAL_WWW` — personal website
- `PERSONAL_ICQ` — ICQ number
- `PERSONAL_GENDER` — gender
- `PERSONAL_FAX` — fax number
- `PERSONAL_MOBILE` — phone number
- `PERSONAL_STREET` — address
- `PERSONAL_MAILBOX` — mailbox
- `PERSONAL_CITY` — city
- `PERSONAL_STATE` — state
- `PERSONAL_ZIP` — zip code
- `PERSONAL_COUNTRY` — country
- `WORK_COMPANY` — company
- `WORK_DEPARTMENT` — department
- `WORK_POSITION` — position
- `WORK_WWW` — company website
- `WORK_PHONE` — work phone
- `WORK_FAX` — work fax
- `WORK_STREET` — company address
- `WORK_MAILBOX` — work mailbox
- `WORK_CITY` — company city
- `WORK_STATE` — company state
- `WORK_ZIP` — company zip code
- `WORK_COUNTRY` — company country
||
|#

## Form Operating Mode {#form}

When adding a handler, the `FORM_DATA` parameter must be passed in the `SETTINGS`. This method is suitable if no information is required from the customer or only a small set of data needs to be requested.

Form fields are automatically displayed according to the design of the payment page.

Form data (the `FIELDS` values from `FORM_DATA`) will be sent to `ACTION_URI`. In addition to the data defined in `FORM_DATA`, two system keys will also be sent with the form:

#|
|| **Name**
`type` | **Description** ||
|| **BX_PAYSYSTEM_ID**
[`sale_paysystem.ID`](../sale/data-types.md) | Identifier of the payment system through which the payment is made. Can be used to call the payment method [sale.paysystem.pay.payment](./sale-pay-system-pay-payment.md) ||
|| **BX_RETURN_URL**
[`string`](../data-types.md) | URL of the store's site to which the user will be redirected ||
|#

### Parameters Passed When Adding a Handler in the FORM_DATA Array

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ACTION_URI*** 
[`string`](../data-types.md) | URL to which the form is sent ||
|| **METHOD**
[`string`](../data-types.md) | HTTP method used when sending the form. Default is empty, in which case the [GET](https://htmlbook.ru/html/form/method) method is used ||
|| **FIELDS**
[`object`](../data-types.md) | Description of form fields (detailed description provided [below](#parametr-fields)) ||
|| **PARAMS**
[`object`](../data-types.md) | Description of form fields. This parameter is deprecated; it is recommended to use the `FIELDS` parameter.

Represents a mapping between field names in the form (`string`) and handler parameter codes (`string`): `[field_code => handler_parameter_code, ...]`

Fields are added to the form as `hidden` type elements.

If both `FIELDS` and `PARAMS` are passed, only `FIELDS` will be used.
||
|#

### FIELDS Parameter

{% include [Note on required parameters](../../_includes/required.md) %}

Represents an array of descriptions of fields displayed in the form and sent to `ACTION_URI`. The key is the field code used as the field name in the form. The values are the field parameter objects.

#|
|| **Name**
`type` | **Description** ||
|| **CODE*** 
[`string`\|`object`](../data-types.md) | If the value of the `CODE` key is of type `string`, this value will be used to find a match between form fields and handler parameters (`CODES`). The name and value will be obtained from the handler parameters.

If an `object` is passed in the `CODE` key, a field will be added to the payment form according to the description in the array content (detailed description provided [below](#parametr-code))
||
|| **VISIBLE**
[`string`](../data-types.md) | Whether the field is displayed in the form for input. Available values: 
- `Y` — yes
- `N` — no

Default value is `N`, the field is displayed in the form as `hidden` ||
|#

### CODE Parameter

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../data-types.md) | Name of the field ||
|| **INPUT**
[`object`](../data-types.md) | Description of the input field. Contains the `TYPE` key — the type of the field, which can take values: 
- `STRING` — string
- `Y/N` — checkbox
||
|#

### Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"Handler.Rest FORM","CODE":"resthandlerform","SORT":100,"SETTINGS":{"CURRENCY":["USD"],"CLIENT_TYPE":"b2c","FORM_DATA":{"ACTION_URI":"http://example.com/payment_form.php","METHOD":"POST","FIELDS":{"phone":{"VISIBLE":"Y","CODE":{"NAME":"Phone number","TYPE":"STRING"}},"selection":{"VISIBLE":"Y","CODE":{"NAME":"Selection illusion","INPUT":{"TYPE":"Y/N"}}},"paymentId":{"CODE":"PAYMENT_ID","VISIBLE":"Y"},"serviceid":{"CODE":"REST_SERVICE_ID"}}},"CODES":{"REST_SERVICE_ID":{"NAME":"Store number","DESCRIPTION":"Store number","SORT":"100"},"REST_SERVICE_KEY":{"NAME":"Secret key","DESCRIPTION":"Secret key","SORT":"300"},"PAYMENT_ID":{"NAME":"Payment number","SORT":"400","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"PAYMENT","PROVIDER_VALUE":"ACCOUNT_NUMBER"}},"PAYMENT_SHOULD_PAY":{"NAME":"Payment amount","SORT":"600","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"PAYMENT","PROVIDER_VALUE":"SUM"}},"PS_CHANGE_STATUS_PAY":{"NAME":"Automatic payment status change","SORT":"700","INPUT":{"TYPE":"Y/N"}},"PAYMENT_BUYER_ID":{"NAME":"Customer code","SORT":"1000","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"ORDER","PROVIDER_VALUE":"USER_ID"}},"PS_WORK_MODE":{"NAME":"Payment system operating mode","SORT":"1100","INPUT":{"TYPE":"ENUM","OPTIONS":{"TEST":"Test","REGULAR":"Live"}}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.handler.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"Handler.Rest FORM","CODE":"resthandlerform","SORT":100,"SETTINGS":{"CURRENCY":["USD"],"CLIENT_TYPE":"b2c","FORM_DATA":{"ACTION_URI":"http://example.com/payment_form.php","METHOD":"POST","FIELDS":{"phone":{"VISIBLE":"Y","CODE":{"NAME":"Phone number","TYPE":"STRING"}},"selection":{"VISIBLE":"Y","CODE":{"NAME":"Selection illusion","INPUT":{"TYPE":"Y/N"}}},"paymentId":{"CODE":"PAYMENT_ID","VISIBLE":"Y"},"serviceid":{"CODE":"REST_SERVICE_ID"}}},"CODES":{"REST_SERVICE_ID":{"NAME":"Store number","DESCRIPTION":"Store number","SORT":"100"},"REST_SERVICE_KEY":{"NAME":"Secret key","DESCRIPTION":"Secret key","SORT":"300"},"PAYMENT_ID":{"NAME":"Payment number","SORT":"400","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"PAYMENT","PROVIDER_VALUE":"ACCOUNT_NUMBER"}},"PAYMENT_SHOULD_PAY":{"NAME":"Payment amount","SORT":"600","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"PAYMENT","PROVIDER_VALUE":"SUM"}},"PS_CHANGE_STATUS_PAY":{"NAME":"Automatic payment status change","SORT":"700","INPUT":{"TYPE":"Y/N"}},"PAYMENT_BUYER_ID":{"NAME":"Customer code","SORT":"1000","GROUP":"PAYMENT","DEFAULT":{"PROVIDER_KEY":"ORDER","PROVIDER_VALUE":"USER_ID"}},"PS_WORK_MODE":{"NAME":"Payment system operating mode","SORT":"1100","INPUT":{"TYPE":"ENUM","OPTIONS":{"TEST":"Test","REGULAR":"Live"}}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.handler.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.paysystem.handler.add",
    		{
    			"NAME": "Handler.Rest FORM",
    			"CODE": "resthandlerform",
    			"SORT": 100,
    			"SETTINGS": {
    				"CURRENCY": [
    					"USD"
    				],
    				"CLIENT_TYPE": "b2c",
    				"FORM_DATA": {
    					"ACTION_URI": "http://example.com/payment_form.php",
    					"METHOD": "POST",
    					"FIELDS": {
    						"phone": {
    							"VISIBLE": "Y",
    							"CODE": {
    								"NAME": "Phone number",
    								"TYPE": "STRING"
    							}
    						},
    						"selection": {
    							"VISIBLE": "Y",
    							"CODE": {
    								"NAME": "Selection illusion",
    								"INPUT": {
    									"TYPE": "Y/N"
    								}
    							}
    						},
    						"paymentId": {
    							"CODE": "PAYMENT_ID",
    							"VISIBLE": "Y"
    						},
    						"serviceid": {
    							"CODE": "REST_SERVICE_ID"
    						}
    					}
    				},
    				"CODES": {
    					"REST_SERVICE_ID": {
    						"NAME": "Store number",
    						"DESCRIPTION": "Store number",
    						"SORT": "100"
    					},
    					"REST_SERVICE_KEY": {
    						"NAME": "Secret key",
    						"DESCRIPTION": "Secret key",
    						"SORT": "300"
    					},
    					"PAYMENT_ID": {
    						"NAME": "Payment number",
    						"SORT": "400",
    						"GROUP": "PAYMENT",
    						"DEFAULT": {
    							"PROVIDER_KEY": "PAYMENT",
    							"PROVIDER_VALUE": "ACCOUNT_NUMBER"
    						}
    					},
    					"PAYMENT_SHOULD_PAY": {
    						"NAME": "Payment amount",
    						"SORT": "600",
    						"GROUP": "PAYMENT",
    						"DEFAULT": {
    							"PROVIDER_KEY": "PAYMENT",
    							"PROVIDER_VALUE": "SUM"
    						}
    					},
    					"PS_CHANGE_STATUS_PAY": {
    						"NAME": "Automatic payment status change",
    						"SORT": "700",
    						"INPUT": {
    							"TYPE": "Y/N"
    						}
    					},
    					"PAYMENT_BUYER_ID": {
    						"NAME": "Customer code",
    						"SORT": "1000",
    						"GROUP": "PAYMENT",
    						"DEFAULT": {
    							"PROVIDER_KEY": "ORDER",
    							"PROVIDER_VALUE": "USER_ID"
    						}
    					},
    					"PS_WORK_MODE": {
    						"NAME": "Payment system operating mode",
    						"SORT": "1100",
    						"INPUT": {
    							"TYPE": "ENUM",
    							"OPTIONS": {
    								"TEST": "Test",
    								"REGULAR": "Live"
    							}
    						}
    					}
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch(error)
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
                'sale.paysystem.handler.add',
                [
                    'NAME'     => 'Handler.Rest FORM',
                    'CODE'     => 'resthandlerform',
                    'SORT'     => 100,
                    'SETTINGS' => [
                        'CURRENCY'    => ['USD'],
                        'CLIENT_TYPE' => 'b2c',
                        'FORM_DATA'   => [
                            'ACTION_URI' => 'http://example.com/payment_form.php',
                            'METHOD'     => 'POST',
                            'FIELDS'     => [
                                'phone'     => [
                                    'VISIBLE' => 'Y',
                                    'CODE'    => [
                                        'NAME' => 'Phone number',
                                        'TYPE' => 'STRING',
                                    ],
                                ],
                                'selection' => [
                                    'VISIBLE' => 'Y',
                                    'CODE'    => [
                                        'NAME' => 'Selection illusion',
                                        'INPUT' => [
                                            'TYPE' => 'Y/N',
                                        ],
                                    ],
                                ],
                                'paymentId' => [
                                    'CODE'    => 'PAYMENT_ID',
                                    'VISIBLE' => 'Y',
                                ],
                                'serviceid' => [
                                    'CODE' => 'REST_SERVICE_ID',
                                ],
                            ],
                        ],
                        'CODES'      => [
                            'REST_SERVICE_ID'    => [
                                'NAME'        => 'Store number',
                                'DESCRIPTION' => 'Store number',
                                'SORT'        => '100',
                            ],
                            'REST_SERVICE_KEY'   => [
                                'NAME'        => 'Secret key',
                                'DESCRIPTION' => 'Secret key',
                                'SORT'        => '300',
                            ],
                            'PAYMENT_ID'         => [
                                'NAME'    => 'Payment number',
                                'SORT'    => '400',
                                'GROUP'   => 'PAYMENT',
                                'DEFAULT' => [
                                    'PROVIDER_KEY'   => 'PAYMENT',
                                    'PROVIDER_VALUE' => 'ACCOUNT_NUMBER',
                                ],
                            ],
                            'PAYMENT_SHOULD_PAY' => [
                                'NAME'    => 'Payment amount',
                                'SORT'    => '600',
                                'GROUP'   => 'PAYMENT',
                                'DEFAULT' => [
                                    'PROVIDER_KEY'   => 'PAYMENT',
                                    'PROVIDER_VALUE' => 'SUM',
                                ],
                            ],
                            'PS_CHANGE_STATUS_PAY' => [
                                'NAME'  => 'Automatic payment status change',
                                'SORT'  => '700',
                                'INPUT' => [
                                    'TYPE' => 'Y/N',
                                ],
                            ],
                            'PAYMENT_BUYER_ID'     => [
                                'NAME'    => 'Customer code',
                                'SORT'    => '1000',
                                'GROUP'   => 'PAYMENT',
                                'DEFAULT' => [
                                    'PROVIDER_KEY'   => 'ORDER',
                                    'PROVIDER_VALUE' => 'USER_ID',
                                ],
                            ],
                            'PS_WORK_MODE'        => [
                                'NAME'  => 'Payment system operating mode',
                                'SORT'  => '1100',
                                'INPUT' => [
                                    'TYPE'    => 'ENUM',
                                    'OPTIONS' => [
                                        'TEST'    => 'Test',
                                        'REGULAR' => 'Live',
                                    ],
                                ],
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding payment system handler: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.paysystem.handler.add",
        {
            "NAME": "Handler.Rest FORM",
            "CODE": "resthandlerform",
            "SORT": 100,
            "SETTINGS": {
                "CURRENCY": [
                    "USD"
                ],
                "CLIENT_TYPE": "b2c",
                "FORM_DATA": {
                    "ACTION_URI": "http://example.com/payment_form.php",
                    "METHOD": "POST",
                    "FIELDS": {
                        "phone": {
                            "VISIBLE": "Y",
                            "CODE": {
                                "NAME": "Phone number",
                                "TYPE": "STRING"
                            }
                        },
                        "selection": {
                            "VISIBLE": "Y",
                            "CODE": {
                                "NAME": "Selection illusion",
                                "INPUT": {
                                    "TYPE": "Y/N"
                                }
                            }
                        },
                        "paymentId": {
                            "CODE": "PAYMENT_ID",
                            "VISIBLE": "Y"
                        },
                        "serviceid": {
                            "CODE": "REST_SERVICE_ID"
                        }
                    }
                },
                "CODES": {
                    "REST_SERVICE_ID": {
                        "NAME": "Store number",
                        "DESCRIPTION": "Store number",
                        "SORT": "100"
                    },
                    "REST_SERVICE_KEY": {
                        "NAME": "Secret key",
                        "DESCRIPTION": "Secret key",
                        "SORT": "300"
                    },
                    "PAYMENT_ID": {
                        "NAME": "Payment number",
                        "SORT": "400",
                        "GROUP": "PAYMENT",
                        "DEFAULT": {
                            "PROVIDER_KEY": "PAYMENT",
                            "PROVIDER_VALUE": "ACCOUNT_NUMBER"
                        }
                    },
                    "PAYMENT_SHOULD_PAY": {
                        "NAME": "Payment amount",
                        "SORT": "600",
                        "GROUP": "PAYMENT",
                        "DEFAULT": {
                            "PROVIDER_KEY": "PAYMENT",
                            "PROVIDER_VALUE": "SUM"
                        }
                    },
                    "PS_CHANGE_STATUS_PAY": {
                        "NAME": "Automatic payment status change",
                        "SORT": "700",
                        "INPUT": {
                            "TYPE": "Y/N"
                        }
                    },
                    "PAYMENT_BUYER_ID": {
                        "NAME": "Customer code",
                        "SORT": "1000",
                        "GROUP": "PAYMENT",
                        "DEFAULT": {
                            "PROVIDER_KEY": "ORDER",
                            "PROVIDER_VALUE": "USER_ID"
                        }
                    },
                    "PS_WORK_MODE": {
                        "NAME": "Payment system operating mode",
                        "SORT": "1100",
                        "INPUT": {
                            "TYPE": "ENUM",
                            "OPTIONS": {
                                "TEST": "Test",
                                "REGULAR": "Live"
                            }
                        }
                    }
                }
            }
        }
        ,
        function (result) {
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
        'sale.paysystem.handler.add',
        [
            'NAME' => 'Handler.Rest FORM',
            'CODE' => 'resthandlerform',
            'SORT' => 100,
            'SETTINGS' => [
                'CURRENCY' => ['USD'],
                'CLIENT_TYPE' => 'b2c',
                'FORM_DATA' => [
                    'ACTION_URI' => 'http://example.com/payment_form.php',
                    'METHOD' => 'POST',
                    'FIELDS' => [
                        'phone' => [
                            'VISIBLE' => 'Y',
                            'CODE' => [
                                'NAME' => 'Phone number',
                                'TYPE' => 'STRING'
                            ]
                        },
                        'selection' => [
                            'VISIBLE' => 'Y',
                            'CODE' => [
                                'NAME' => 'Selection illusion',
                                'INPUT' => [
                                    'TYPE' => 'Y/N'
                                ]
                            ]
                        },
                        'paymentId' => [
                            'CODE' => 'PAYMENT_ID',
                            'VISIBLE' => 'Y'
                        },
                        'serviceid' => [
                            'CODE' => 'REST_SERVICE_ID'
                        ]
                    ]
                ],
                'CODES' => [
                    'REST_SERVICE_ID' => [
                        'NAME' => 'Store number',
                        'DESCRIPTION' => 'Store number',
                        'SORT' => '100'
                    ],
                    'REST_SERVICE_KEY' => [
                        'NAME' => 'Secret key',
                        'DESCRIPTION' => 'Secret key',
                        'SORT' => '300'
                    ],
                    'PAYMENT_ID' => [
                        'NAME' => 'Payment number',
                        'SORT' => '400',
                        'GROUP' => 'PAYMENT',
                        'DEFAULT' => [
                            'PROVIDER_KEY' => 'PAYMENT',
                            'PROVIDER_VALUE' => 'ACCOUNT_NUMBER'
                        ]
                    ],
                    'PAYMENT_SHOULD_PAY' => [
                        'NAME' => 'Payment amount',
                        'SORT' => '600',
                        'GROUP' => 'PAYMENT',
                        'DEFAULT' => [
                            'PROVIDER_KEY' => 'PAYMENT',
                            'PROVIDER_VALUE' => 'SUM'
                        ]
                    ],
                    'PS_CHANGE_STATUS_PAY' => [
                        'NAME' => 'Automatic payment status change',
                        'SORT' => '700',
                        'INPUT' => [
                            'TYPE' => 'Y/N'
                        ]
                    ],
                    'PAYMENT_BUYER_ID' => [
                        'NAME' => 'Customer code',
                        'SORT' => '1000',
                        'GROUP' => 'PAYMENT',
                        'DEFAULT' => [
                            'PROVIDER_KEY' => 'ORDER',
                            'PROVIDER_VALUE' => 'USER_ID'
                        ]
                    ],
                    'PS_WORK_MODE' => [
                        'NAME' => 'Payment system operating mode',
                        'SORT' => '1100',
                        'INPUT' => [
                            'TYPE' => 'ENUM',
                            'OPTIONS' => [
                                'TEST' => 'Test',
                                'REGULAR' => 'Live'
                            ]
                        ]
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Checkout Operating Mode {#checkout}

When adding a handler, the `CHECKOUT_DATA` must be passed in the `SETTINGS`.

A script that will process the received data, create the payment, and return the identifier of the created payment and the URL of the payment page must be located at the address specified in `ACTION_URI`.

Data for payment will be sent to `ACTION_URI` in the form of an array. It contains an array of system parameters in the `BX_SYSTEM_PARAMS` key and the `FIELDS` values from `CHECKOUT_DATA`, each as a separate key at the top level of the array.

Structure of the `BX_SYSTEM_PARAMS` array:

#|
|| **Name**
`type` | **Description** ||
|| **RETURN_URL**
[`string`](../data-types.md) | Current page ||
|| **PAYSYSTEM_ID**
[`sale_paysystem.ID`](../sale/data-types.md) | Identifier of the payment system ||
|| **PAYMENT_ID**
[`sale_order_payment.id`](../sale/data-types.md) | Identifier of the payment ||
|| **SUM**
[`double`](../data-types.md) | Payment amount ||
|| **CURRENCY**
[`string`](../data-types.md) | Currency ||
|| **EXTERNAL_PAYMENT_ID**
[`string`](../data-types.md) | Identifier of the payment in the payment system (if any). For example, if a request has already been sent to `ACTION_URI` for the current payment ||
|#

In response to a request to `ACTION_URI`, the script must return the identifier of the created payment and the URL of the payment page.

#|
|| **Name**
`type` | **Description** ||
|| **PAYMENT_URL**
[`string`](../data-types.md) | URL of the payment page ||
|| **PAYMENT_ID**
[`string`](../data-types.md) | Identifier of the payment in the payment system ||
|#

The customer will be redirected to the link from `PAYMENT_URL` automatically or by clicking the "Buy" button. If fields intended for filling through the form are passed among others in `FIELDS`, a form will be displayed to the customer.

As a result, an array of errors can be returned in the `PAYMENT_ERRORS` key. The manager will see the errors in the timeline or on the payment page (depending on the template used).

#|
|| **Name**
`type` | **Description** ||
|| **PAYMENT_ERRORS**
[`string[]`](../data-types.md) | List of errors that occurred during payment creation ||
|#

If nothing is returned, the default error will be used: `Error registering order in payment system`.

### Parameters Passed When Adding a Handler in the CHECKOUT_DATA Array

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ACTION_URI*** 
[`string`](../data-types.md) | URL to which the request for payment creation is sent ||
|| **FIELDS**
[`object`](../data-types.md) | Description of fields sent to `ACTION_URI`. The format is similar to the [FIELDS](#parametr-fields) field in the form operating mode (`FORM_DATA`) ||
|#

### Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"Handler.Rest CHECKOUT","CODE":"resthandlercheckout","SORT":100,"SETTINGS":{"CURRENCY":["USD"],"CLIENT_TYPE":"b2c","CHECKOUT_DATA":{"ACTION_URI":"http://example.com/payment_checkout.php","FIELDS":{"serviceKey":{"CODE":"REST_SERVICE_KEY_CHECKOUT"},"serviceid":{"CODE":"REST_SERVICE_ID_CHECKOUT"}}},"CODES":{"REST_SERVICE_ID_CHECKOUT":{"NAME":"Store number","DESCRIPTION":"Store number","SORT":"100"},"REST_SERVICE_KEY_CHECKOUT":{"NAME":"Secret key","DESCRIPTION":"Secret key","SORT":"300"},"PS_WORK_MODE_CHECKOUT":{"NAME":"Payment system operating mode","SORT":"1100","INPUT":{"TYPE":"ENUM","OPTIONS":{"TEST":"Test","REGULAR":"Live"}}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.handler.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"Handler.Rest CHECKOUT","CODE":"resthandlercheckout","SORT":100,"SETTINGS":{"CURRENCY":["USD"],"CLIENT_TYPE":"b2c","CHECKOUT_DATA":{"ACTION_URI":"http://example.com/payment_checkout.php","FIELDS":{"serviceKey":{"CODE":"REST_SERVICE_KEY_CHECKOUT"},"serviceid":{"CODE":"REST_SERVICE_ID_CHECKOUT"}}},"CODES":{"REST_SERVICE_ID_CHECKOUT":{"NAME":"Store number","DESCRIPTION":"Store number","SORT":"100"},"REST_SERVICE_KEY_CHECKOUT":{"NAME":"Secret key","DESCRIPTION":"Secret key","SORT":"300"},"PS_WORK_MODE_CHECKOUT":{"NAME":"Payment system operating mode","SORT":"1100","INPUT":{"TYPE":"ENUM","OPTIONS":{"TEST":"Test","REGULAR":"Live"}}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.handler.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.paysystem.handler.add",
    		{
    			"NAME": "Handler.Rest CHECKOUT",
    			"CODE": "resthandlercheckout",
    			"SORT": 100,
    			"SETTINGS": {
    				"CURRENCY": [
    					"USD"
    				],
    				"CLIENT_TYPE": "b2c",
    				"CHECKOUT_DATA": {
    					"ACTION_URI": "http://example.com/payment_checkout.php",
    					"FIELDS": {
    						"serviceKey": {
    							"CODE": "REST_SERVICE_KEY_CHECKOUT",
    						},
    						"serviceid": {
    							"CODE": "REST_SERVICE_ID_CHECKOUT"
    						}
    					}
    				},
    				"CODES": {
    					"REST_SERVICE_ID_CHECKOUT": {
    						"NAME": "Store number",
    						"DESCRIPTION": "Store number",
    						"SORT": "100"
    					},
    					"REST_SERVICE_KEY_CHECKOUT": {
    						"NAME": "Secret key",
    						"DESCRIPTION": "Secret key",
    						"SORT": "300"
    					},
    					"PS_WORK_MODE_CHECKOUT": {
    						"NAME": "Payment system operating mode",
    						"SORT": "1100",
    						"INPUT": {
    							"TYPE": "ENUM",
    							"OPTIONS": {
    								"TEST": "Test",
    								"REGULAR": "Live"
    							}
    						}
    					}
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info("Handler added with ID " + result);
    }
    catch( error )
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
                'sale.paysystem.handler.add',
                [
                    'NAME'     => 'Handler.Rest CHECKOUT',
                    'CODE'     => 'resthandlercheckout',
                    'SORT'     => 100,
                    'SETTINGS' => [
                        'CURRENCY'    => ['USD'],
                        'CLIENT_TYPE' => 'b2c',
                        'CHECKOUT_DATA' => [
                            'ACTION_URI' => 'http://example.com/payment_checkout.php',
                            'FIELDS'     => [
                                'serviceKey' => [
                                    'CODE' => 'REST_SERVICE_KEY_CHECKOUT',
                                ],
                                'serviceid'  => [
                                    'CODE' => 'REST_SERVICE_ID_CHECKOUT'
                                ]
                            ]
                        ],
                        'CODES' => [
                            'REST_SERVICE_ID_CHECKOUT' => [
                                'NAME'        => 'Store number',
                                'DESCRIPTION' => 'Store number',
                                'SORT'        => '100'
                            ],
                            'REST_SERVICE_KEY_CHECKOUT' => [
                                'NAME'        => 'Secret key',
                                'DESCRIPTION' => 'Secret key',
                                'SORT'        => '300'
                            ],
                            'PS_WORK_MODE_CHECKOUT'     => [
                                'NAME'  => 'Payment system operating mode',
                                'SORT'  => '1100',
                                'INPUT' => [
                                    'TYPE'    => 'ENUM',
                                    'OPTIONS' => [
                                        'TEST'    => 'Test',
                                        'REGULAR' => 'Live'
                                    ]
                                ]
                            ]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Handler added with ID ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding payment system handler: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.paysystem.handler.add",
        {
            "NAME": "Handler.Rest CHECKOUT",
            "CODE": "resthandlercheckout",
            "SORT": 100,
            "SETTINGS": {
                "CURRENCY": [
                    "USD"
                ],
                "CLIENT_TYPE": "b2c",
                "CHECKOUT_DATA": {
                    "ACTION_URI": "http://example.com/payment_checkout.php",
                    "FIELDS": {
                        "serviceKey": {
                            "CODE": "REST_SERVICE_KEY_CHECKOUT",
                        },
                        "serviceid": {
                            "CODE": "REST_SERVICE_ID_CHECKOUT"
                        }
                    }
                },
                "CODES": {
                    "REST_SERVICE_ID_CHECKOUT": {
                        "NAME": "Store number",
                        "DESCRIPTION": "Store number",
                        "SORT": "100"
                    },
                    "REST_SERVICE_KEY_CHECKOUT": {
                        "NAME": "Secret key",
                        "DESCRIPTION": "Secret key",
                        "SORT": "300"
                    },
                    "PS_WORK_MODE_CHECKOUT": {
                        "NAME": "Payment system operating mode",
                        "SORT": "1100",
                        "INPUT": {
                            "TYPE": "ENUM",
                            "OPTIONS": {
                                "TEST": "Test",
                                "REGULAR": "Live"
                            }
                        }
                    }
                }
            }
        }
        ,
        function (result) {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info("Handler added with ID " + result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paysystem.handler.add',
        [
            'NAME' => 'Handler.Rest CHECKOUT',
            'CODE' => 'resthandlercheckout',
            'SORT' => 100,
            'SETTINGS' => [
                'CURRENCY'    => ['USD'],
                'CLIENT_TYPE' => 'b2c',
                'CHECKOUT_DATA' => [
                    'ACTION_URI' => 'http://example.com/payment_checkout.php',
                    'FIELDS' => [
                        'serviceKey' => [
                            'CODE' => 'REST_SERVICE_KEY_CHECKOUT',
                        ],
                        'serviceid' => [
                            'CODE' => 'REST_SERVICE_ID_CHECKOUT'
                        ]
                    ]
                ],
                'CODES' => [
                    'REST_SERVICE_ID_CHECKOUT' => [
                        'NAME' => 'Store number',
                        'DESCRIPTION' => 'Store number',
                        'SORT' => '100'
                    ],
                    'REST_SERVICE_KEY_CHECKOUT' => [
                        'NAME' => 'Secret key',
                        'DESCRIPTION' => 'Secret key',
                        'SORT' => '300'
                    ],
                    'PS_WORK_MODE_CHECKOUT' => [
                        'NAME' => 'Payment system operating mode',
                        'SORT' => '1100',
                        'INPUT' => [
                            'TYPE' => 'ENUM',
                            'OPTIONS' => [
                                'TEST' => 'Test',
                                'REGULAR' => 'Live'
                            ]
                        ]
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## IFrame Operating Mode {#iframe}

When adding a handler, the `IFRAME_DATA` must be passed in the `SETTINGS`.

A page that will be loaded in the iframe on the seller's site must be located at the address specified in `ACTION_URI`.

When loading the iframe via the [Window.postMessage()](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) method, the `FIELDS` values from `IFRAME_DATA` will be sent to `ACTION_URI` (each as a separate key at the top level of the array), along with the following data:

#|
|| **Name**
`type` | **Description** ||
|| **BX_SYSTEM_PARAMS**
[`object`](../data-types.md) | System parameters ||
|| **BX_COMPUTED_STYLE**
[`object`](../data-types.md) | Styles of the parent iframe element obtained via the [window.getComputedStyle()](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle) method ||
|#

Data sent in `BX_SYSTEM_PARAMS`:

#|
|| **Name**
`type` | **Description** ||
|| **RETURN_URL**
[`string`](../data-types.md) | Current page ||
|| **PAYSYSTEM_ID**
[`sale_paysystem.ID`](../sale/data-types.md) | Identifier of the payment system ||
|| **PAYMENT_ID**
[`sale_order_payment.id`](../sale/data-types.md) | Identifier of the payment ||
|| **SUM**
[`string`](../data-types.md) | Payment amount ||
|| **CURRENCY**
[`string`](../data-types.md) | Currency ||
|#

You can obtain values in the iframe through the `message` event handler, for example:

```js
document.addEventListener("DOMContentLoaded", function() {
	window.addEventListener("message", function (event) {
		// receiving data from the site (from the payment system)
		var paymentData = event.data;
		// working with BX_SYSTEM_PARAMS
		if (paymentData.BX_SYSTEM_PARAMS)
		{
			// ...
		}
		// using site styles
		if (paymentData.BX_COMPUTED_STYLE)
		{
			document.body.style.background = paymentData.BX_COMPUTED_STYLE.background;
			document.body.style.color = paymentData.BX_COMPUTED_STYLE.color;
		}
	}, false);
});
```

By default, the width of the iframe is 100% of the parent element, and the height is 350px.

The dimensions of the iframe can be changed. To do this, the iframe must send the height and/or width to the seller's site. For example:

```js
document.addEventListener("DOMContentLoaded", function() {
	var size = {
		width: document.body.scrollWidth,
		height: document.body.scrollHeight
	};
	// sending data to the seller's site
	parent.postMessage(size, "*");
});
```
`width` and `height` are reserved variable names, and only they are processed on the seller's site.

### Parameters Passed When Adding a Handler in the IFRAME_DATA Array

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ACTION_URI*** 
[`string`](../data-types.md) | URL of the page that will be displayed in the iframe element ||
|| **FIELDS**
[`object`](../data-types.md) | Description of fields sent to the `iframe`. The format is similar to the [FIELDS](#parametr-fields) field in the form operating mode (`FORM_DATA`) ||
|#

### Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"Handler.Rest IFrame","CODE":"resthandleriframe","SORT":100,"SETTINGS":{"CURRENCY":["USD"],"CLIENT_TYPE":"b2c","IFRAME_DATA":{"ACTION_URI":"http://example.com/payment_iframe.php","FIELDS":{"serviceKey":{"CODE":"REST_SERVICE_KEY_IFRAME"},"serviceid":{"CODE":"REST_SERVICE_ID_IFRAME"}}},"CODES":{"REST_SERVICE_ID_IFRAME":{"NAME":"Store number","DESCRIPTION":"Store number","SORT":"100"},"REST_SERVICE_KEY_IFRAME":{"NAME":"Secret key","DESCRIPTION":"Secret key","SORT":"300"},"PS_WORK_MODE_IFRAME":{"NAME":"Payment system operating mode","SORT":"1100","INPUT":{"TYPE":"ENUM","OPTIONS":{"TEST":"Test","REGULAR":"Live"}}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.handler.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NAME":"Handler.Rest IFrame","CODE":"resthandleriframe","SORT":100,"SETTINGS":{"CURRENCY":["USD"],"CLIENT_TYPE":"b2c","IFRAME_DATA":{"ACTION_URI":"http://example.com/payment_iframe.php","FIELDS":{"serviceKey":{"CODE":"REST_SERVICE_KEY_IFRAME"},"serviceid":{"CODE":"REST_SERVICE_ID_IFRAME"}}},"CODES":{"REST_SERVICE_ID_IFRAME":{"NAME":"Store number","DESCRIPTION":"Store number","SORT":"100"},"REST_SERVICE_KEY_IFRAME":{"NAME":"Secret key","DESCRIPTION":"Secret key","SORT":"300"},"PS_WORK_MODE_IFRAME":{"NAME":"Payment system operating mode","SORT":"1100","INPUT":{"TYPE":"ENUM","OPTIONS":{"TEST":"Test","REGULAR":"Live"}}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.handler.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"sale.paysystem.handler.add",
    		{
    			"NAME": "Handler.Rest IFrame",
    			"CODE": "resthandleriframe",
    			"SORT": 100,
    			"SETTINGS": {
    				"CURRENCY": [
    					"USD"
    				],
    				"CLIENT_TYPE": "b2c",
    				"IFRAME_DATA": {
    					"ACTION_URI": "http://example.com/payment_iframe.php",
    					"FIELDS": {
    						"serviceKey": {
    							"CODE": "REST_SERVICE_KEY_IFRAME",
    						},
    						"serviceid": {
    							"CODE": "REST_SERVICE_ID_IFRAME"
    						}
    					}
    				},
    				"CODES": {
    					"REST_SERVICE_ID_IFRAME": {
    						"NAME": "Store number",
    						"DESCRIPTION": "Store number",
    						"SORT": "100"
    					},
    					"REST_SERVICE_KEY_IFRAME": {
    						"NAME": "Secret key",
    						"DESCRIPTION": "Secret key",
    						"SORT": "300"
    					},
    					"PS_WORK_MODE_IFRAME": {
    						"NAME": "Payment system operating mode",
    						"SORT": "1100",
    						"INPUT": {
    							"TYPE": "ENUM",
    							"OPTIONS": {
    								"TEST": "Test",
    								"REGULAR": "Live"
    							}
    						}
    					}
    				}
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
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
                'sale.paysystem.handler.add',
                [
                    'NAME'     => 'Handler.Rest IFrame',
                    'CODE'     => 'resthandleriframe',
                    'SORT'     => 100,
                    'SETTINGS' => [
                        'CURRENCY'    => ['USD'],
                        'CLIENT_TYPE' => 'b2c',
                        'IFRAME_DATA' => [
                            'ACTION_URI' => 'http://example.com/payment_iframe.php',
                            'FIELDS'     => [
                                'serviceKey' => [
                                    'CODE' => 'REST_SERVICE_KEY_IFRAME',
                                ],
                                'serviceid'  => [
                                    'CODE' => 'REST_SERVICE_ID_IFRAME'
                                ]
                            ]
                        ],
                        'CODES'      => [
                            'REST_SERVICE_ID_IFRAME' => [
                                'NAME'        => 'Store number',
                                'DESCRIPTION' => 'Store number',
                                'SORT'        => '100'
                            ],
                            'REST_SERVICE_KEY_IFRAME' => [
                                'NAME'        => 'Secret key',
                                'DESCRIPTION' => 'Secret key',
                                'SORT'        => '300'
                            ],
                            'PS_WORK_MODE_IFRAME'     => [
                                'NAME'  => 'Payment system operating mode',
                                'SORT'  => '1100',
                                'INPUT' => [
                                    'TYPE'    => 'ENUM',
                                    'OPTIONS' => [
                                        'TEST'    => 'Test',
                                        'REGULAR' => 'Live'
                                    ]
                                ]
                            ]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding payment system handler: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.paysystem.handler.add",
        {
            "NAME": "Handler.Rest IFrame",
            "CODE": "resthandleriframe",
            "SORT": 100,
            "SETTINGS": {
                "CURRENCY": [
                    "USD"
                ],
                "CLIENT_TYPE": "b2c",
                "IFRAME_DATA": {
                    "ACTION_URI": "http://example.com/payment_iframe.php",
                    "FIELDS": {
                        "serviceKey": {
                            "CODE": "REST_SERVICE_KEY_IFRAME",
                        },
                        "serviceid": {
                            "CODE": "REST_SERVICE_ID_IFRAME"
                        }
                    }
                },
                "CODES": {
                    "REST_SERVICE_ID_IFRAME": {
                        "NAME": "Store number",
                        "DESCRIPTION": "Store number",
                        "SORT": "100"
                    },
                    "REST_SERVICE_KEY_IFRAME": {
                        "NAME": "Secret key",
                        "DESCRIPTION": "Secret key",
                        "SORT": "300"
                    },
                    "PS_WORK_MODE_IFRAME": {
                        "NAME": "Payment system operating mode",
                        "SORT": "1100",
                        "INPUT": {
                            "TYPE": "ENUM",
                            "OPTIONS": {
                                "TEST": "Test",
                                "REGULAR": "Live"
                            }
                        }
                    }
                }
            }
        }
        ,
        function (result) {
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
        'sale.paysystem.handler.add',
        [
            'NAME' => 'Handler.Rest IFrame',
            'CODE' => 'resthandleriframe',
            'SORT' => 100,
            'SETTINGS' => [
                'CURRENCY'    => ['USD'],
                'CLIENT_TYPE' => 'b2c',
                'IFRAME_DATA' => [
                    'ACTION_URI' => 'http://example.com/payment_iframe.php',
                    'FIELDS' => [
                        'serviceKey' => [
                            'CODE' => 'REST_SERVICE_KEY_IFRAME',
                        ],
                        'serviceid' => [
                            'CODE' => 'REST_SERVICE_ID_IFRAME'
                        ]
                    ]
                ],
                'CODES' => [
                    'REST_SERVICE_ID_IFRAME' => [
                        'NAME' => 'Store number',
                        'DESCRIPTION' => 'Store number',
                        'SORT' => '100'
                    ],
                    'REST_SERVICE_KEY_IFRAME' => [
                        'NAME' => 'Secret key',
                        'DESCRIPTION' => 'Secret key',
                        'SORT' => '300'
                    ],
                    'PS_WORK_MODE_IFRAME' => [
                        'NAME' => 'Payment system operating mode',
                        'SORT' => '1100',
                        'INPUT' => [
                            'TYPE' => 'ENUM',
                            'OPTIONS' => [
                                'TEST' => 'Test',
                                'REGULAR' => 'Live'
                            ]
                        ]
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

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
[`sale_paysystem_handler.ID`](../sale/data-types.md#sale_paysystem_handler) | Identifier of the created handler, used later for updating and deleting it ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "ERROR_HANDLER_ALREADY_EXIST",
    "error_description": "Handler already exists!"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient rights to add the handler | 403 ||
|| `ERROR_CHECK_FAILURE` | A required field value is not specified or the value of one of the fields is specified incorrectly | 400 ||
|| `ERROR_HANDLER_ALREADY_EXIST` | A handler with the code specified in the `CODE` parameter already exists in the system | 400 ||
|| `ERROR_HANDLER_ADD` | Other errors. For detailed information about the error, see `error_description` | 400 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-pay-system-handler-update.md)
- [{#T}](./sale-pay-system-handler-list.md)
- [{#T}](./sale-pay-system-handler-delete.md)
- [{#T}](./sale-pay-system-add.md)
- [{#T}](./sale-pay-system-update.md)
- [{#T}](./sale-pay-system-list.md)
- [{#T}](./sale-pay-system-settings-get.md)
- [{#T}](./sale-pay-system-settings-update.md)
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-pay-payment.md)
- [{#T}](./sale-pay-system-pay-invoice.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)
- [{#T}](./sale-pay-system-settings-invoice-get.md)