# Implement a Simple Cash Register Using REST API

## Adding a Cash Register Handler

{% include [Note on Examples](../../_includes/examples.md) %}

Example of adding a cash register handler with the ability to configure authorization data, company information, and cash register operation mode.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "sale.cashbox.handler.add",
        {
            "CODE": "my_rest_cashbox",
            "NAME": "My REST Cash Register",
            "SORT": 100,
            "SETTINGS":
            {
                "PRINT_URL": "http://example.com/rest_print.php",
                "CHECK_URL": "http://example.com/rest_check.php",
                "CONFIG":
                {
                    "AUTH": {
                        "LABEL": "Authorization",
                        "ITEMS": {
                            "LOGIN": {
                                "TYPE": "STRING",
                                "REQUIRED": "Y",
                                "LABEL": "Login"
                            },
                            "PASSWORD": {
                                "TYPE": "STRING",
                                "REQUIRED": "Y",
                                "LABEL": "Password"
                            },
                        }
                    },
                    "COMPANY": {
                        "LABEL": "Company Information",
                        "ITEMS": {
                            "INN": {
                                "TYPE": "STRING",
                                "REQUIRED": "Y",
                                "LABEL": "Company INN"
                            }
                        }
                    },
                    "INTERACTION": {
                        "LABEL": "Cash Register Interaction Settings",
                        "ITEMS": {
                            "MODE": {
                                "TYPE": "ENUM",
                                "LABEL": "Cash Register Operation Mode",
                                "OPTIONS": {
                                    "ACTIVE": "live",
                                    "TEST": "test"
                                }
                            }
                        }
                    }
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call('sale.cashbox.handler.add', [
        'CODE' => 'my_rest_cashbox',
        'NAME' => 'My REST Cash Register',
        'SORT' => 100,
        'SETTINGS' => [
            'PRINT_URL' => 'http://example.com/rest_print.php',
            'CHECK_URL' => 'http://example.com/rest_check.php',
            'CONFIG' => [
                'AUTH' => [
                    'LABEL' => 'Authorization',
                    'ITEMS' => [
                        'LOGIN' => [
                            'TYPE' => 'STRING',
                            'REQUIRED' => 'Y',
                            'LABEL' => 'Login'
                        ],
                        'PASSWORD' => [
                            'TYPE' => 'STRING',
                            'REQUIRED' => 'Y',
                            'LABEL' => 'Password'
                        ],
                    ]
                ],
                'COMPANY' => [
                    'LABEL' => 'Company Information',
                    'ITEMS' => [
                        'INN' => [
                            'TYPE' => 'STRING',
                            'REQUIRED' => 'Y',
                            'LABEL' => 'Company INN'
                        ]
                    ]
                ],
                'INTERACTION' => [
                    'LABEL' => 'Cash Register Interaction Settings',
                    'ITEMS' => [
                        'MODE' => [
                            'TYPE' => 'ENUM',
                            'LABEL' => 'Cash Register Operation Mode',
                            'OPTIONS' => [
                                'ACTIVE' => 'live',
                                'TEST' => 'test'
                            ]
                        ]
                    ]
                ]
            ]
        ]
    ]);

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

The added cash register can be configured either through the store settings or Sales Center, or by calling the method [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md).


## Example of Adding a Cash Register

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "sale.cashbox.add",
        {
            "NAME": 'Rest Cash Register',
            "REST_CODE": 'my_rest_cashbox',
            "EMAIL": "user@example.com",
            "NUMBER_KKM": "123",
            "ACTIVE": "Y",
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
                    "LOGIN": "user123",
                    "PASSWORD": "top_secret!"
                },
                "COMPANY":
                {
                    "INN": "1234567890"
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.cashbox.add',
        [
            'FIELDS' => [
                'NAME' => 'Rest Cash Register',
                'REST_CODE' => 'my_rest_cashbox',
                'EMAIL' => 'user@example.com',
                'NUMBER_KKM' => '123',
                'ACTIVE' => 'Y',
                'OFD' => 'bx_ofdruofd',
                'OFD_SETTINGS' => [
                    'OFD_MODE' => [
                        'IS_TEST' => 'N'
                    ]
                ],
                'SETTINGS' => [
                    'AUTH' => [
                        'LOGIN' => 'user123',
                        'PASSWORD' => 'top_secret!'
                    ],
                    'COMPANY' => [
                        'INN' => '1234567890'
                    ],
                    'INTERACTION' => [
                        'MODE' => 'ACTIVE'
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

## PRINT_URL Page

The `PRINT_URL` page is the address to which data for printing the receipt is sent.

For the structure of POST parameters sent to the `PRINT_URL` address, see the description of the method [sale.cashbox.handler.add](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md#print_url).

At the `PRINT_URL`, incoming data is processed, the document is generated, and the print result is returned. The response in case of an error is a JSON array of the following format:

```json
{
"ERRORS": [
    "Error message",
    "Error message",
    ...
]
}
```

In case of successful printing, the array looks like this:

```json
{
    "UUID": "00112233-4455-6677-8899-aabbccddeeff"
}
```

## CHECK_URL Page

The `CHECK_URL` page is the address where the success of the receipt printing is checked.

A request to the `CHECK_URL` address is made either by the manager's request or automatically after some time following the successful printing of the receipt.

For the structure of POST parameters sent to the `CHECK_URL` address, see the description of the method [sale.cashbox.handler.add](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md#check_url).

A request to the `CHECK_URL` must return data about the receipt, error data during receipt printing, or a status of "waiting for print".

The format of data in case of a printing error:

```json
{ 
    "STATUS": "ERROR", 
    "ERROR": "Error message" 
}
```

The format of data if the receipt has not yet been printed:

```json
{ 
    "STATUS": "WAIT"
}
```

The format of data upon successful receipt submission:

```json
{
    "STATUS": "DONE",
    "UUID": "00112233-4455-6677-8899-aabbccddeeff",
    "REG_NUMBER_KKT": "000111222333",
    "FISCAL_DOC_ATTR": "33445500",
    "FISCAL_DOC_NUMBER": 123,
    "FISCAL_RECEIPT_NUMBER": 10,
    "FN_NUMBER": "0011223344556677",
    "SHIFT_NUMBER": 12,
    "PRINT_END_TIME": 1609452000
}
```

The composition of the fields is similar to the parameters of the method [sale.cashbox.check.apply](../../api-reference/sale/cashbox/sale-cashbox-check-apply.md).

The data returned by `CHECK_URL` is stored on the account and used to generate a link to the receipt.

In addition to automatic requests from the account to `CHECK_URL`, the application can send receipt data at any time by calling the method [sale.cashbox.check.apply](../../api-reference/sale/cashbox/sale-cashbox-check-apply.md).

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "sale.cashbox.check.apply",
        {
            'UUID':'00112233-4455-6677-8899-aabbccddeeff',
            'PRINT_END_TIME':'1609459200',
            'REG_NUMBER_KKT':'000111222333',
            'FISCAL_DOC_ATTR':'33445500',
            'FISCAL_DOC_NUMBER':'123',
            'FISCAL_RECEIPT_NUMBER':'10',
            'FN_NUMBER':'0011223344556677',
            'SHIFT_NUMBER':'12'
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.cashbox.check.apply',
        [
            'FIELDS' => [
                'UUID' => '00112233-4455-6677-8899-aabbccddeeff',
                'PRINT_END_TIME' => '1609459200',
                'REG_NUMBER_KKT' => '000111222333',
                'FISCAL_DOC_ATTR' => '33445500',
                'FISCAL_DOC_NUMBER' => '123',
                'FISCAL_RECEIPT_NUMBER' => '10',
                'FN_NUMBER' => '0011223344556677',
                'SHIFT_NUMBER' => '12'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}