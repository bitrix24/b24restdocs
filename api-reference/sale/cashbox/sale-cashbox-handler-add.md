# Add Cash Register Handler sale.cashbox.handler.add

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow changing settings")

This method adds a REST handler for the cash register.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../../data-types.md) | Code of the REST handler. Must be unique among all handlers ||
|| **NAME***
[`string`](../../data-types.md) | Name of the REST handler ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting order. Default value: `100` ||
|| **SUPPORTS_FFD105**
[`string`](../../data-types.md) | Indicates whether the cash register supports fiscal data format version 1.05. Possible values:
- `Y` — yes
- `N` — no
  
Default value: `N` ||
|| **SETTINGS***
[`object`](../../data-types.md) | Settings of the handler (detailed description provided [below](#settings)) ||
|#

### SETTINGS Parameter {#settings}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PRINT_URL***
[`string`](../../data-types.md) | Address to which data for printing the receipt is sent ||
|| **CHECK_URL***
[`string`](../../data-types.md) | Address for checking the status of the receipt ||
|| **HTTP_VERSION**
[`string`](../../data-types.md) | Version of the HTTP protocol used for requests. Possible values: `1.0`, `1.1`. 

If the parameter is not filled, HTTP `1.0` is used for requests ||
|| **CONFIG***
[`object`](../../data-types.md) | Structure of settings (detailed description provided [below](#settingsconfig)), which the user can set and modify on the cash register editing page, as well as when adding or updating the cash register via REST. 

Each key in this parameter defines one section on the settings page, the key is the section code. The values of the object describe the section and the settings contained within it
||
|#

### SETTINGS[CONFIG] Parameter {#settingsconfig}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **LABEL***
[`string`](../../data-types.md) | Title of the section ||
|| **ITEMS***
[`object`](../../data-types.md) | List of settings in the section (detailed description provided [below](#settingsconfigitems)). 

The key is the setting code, and the value is the description of the setting 
||
|#

### SETTINGS[CONFIG][section code][ITEMS] Parameter {#settingsconfigitems}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE***
[`string`](../../data-types.md) | Type of the setting. Possible values:

- `STRING` — string
- `NUMBER` — floating-point number
- `Y/N` — string accepting values `Y` or `N`
- `ENUM` — list of string values
- `DATE` — date ||
|| **LABEL***
[`string`](../../data-types.md) | Name of the setting ||
|| **REQUIRED***
[`string`](../../data-types.md) | Indicates whether the setting is required. Possible values:

- `Y` — yes
- `N` — no
||
||  **DISABLED**
[`string`](../../data-types.md) | Indicates whether editing of the setting is disabled. Possible values:

- `Y` — yes
- `N` — no

Default value: `N` ||
||  **MULTIPLE**
[`string`](../../data-types.md) | Indicates whether the setting is multiple. Possible values:

- `Y` — yes
- `N` — no

Default value: `N` ||
||  **MULTILINE**
[`string`](../../data-types.md) | Indicates whether the field is multiline. Used for the `STRING` type. Possible values:

- `Y` — yes
- `N` — no

Default value: `N` ||
||  **OPTIONS***
[`object`](../../data-types.md) | List of possible values for the property. Used for the `ENUM` type. 

The key of the object is the property value, and the value of the key is the name of the value displayed in the interface ||
||  **TIME**
[`string`](../../data-types.md) | Indicates whether time selection is possible. Used for the `DATE` type. Possible values:

- `Y` — yes
- `N` — no

Default value: `N`
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"restcashbox01","NAME":"REST-Cash Register 01","SORT":100,"SUPPORTS_FFD105":"Y","SETTINGS":{"PRINT_URL":"http://example.com/rest_print.php","CHECK_URL":"http://example.com/rest_check.php","HTTP_VERSION":"1.1","CONFIG":{"AUTH":{"LABEL":"Authorization","ITEMS":{"KEYWORD":{"TYPE":"STRING","LABEL":"Keyword"},"PREFERENCE":{"TYPE":"ENUM","LABEL":"Multiple Choice","REQUIRED":"Y","OPTIONS":{"FIRST":"First","SECOND":"Second","THIRD":"Third"}}}},"INTERACTION":{"LABEL":"Interaction Settings with Cash Register","ITEMS":{"MODE":{"TYPE":"ENUM","LABEL":"Cash Register Operating Mode","OPTIONS":{"ACTIVE":"active","TEST":"test"}}}}}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.cashbox.handler.add
    ```

- cURL (OAuth)

    ```bash
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"restcashbox01","NAME":"REST-Cash Register 01","SORT":100,"SUPPORTS_FFD105":"Y","SETTINGS":{"PRINT_URL":"http://example.com/rest_print.php","CHECK_URL":"http://example.com/rest_check.php","HTTP_VERSION":"1.1","CONFIG":{"AUTH":{"LABEL":"Authorization","ITEMS":{"KEYWORD":{"TYPE":"STRING","LABEL":"Keyword"},"PREFERENCE":{"TYPE":"ENUM","LABEL":"Multiple Choice","REQUIRED":"Y","OPTIONS":{"FIRST":"First","SECOND":"Second","THIRD":"Third"}}}},"INTERACTION":{"LABEL":"Interaction Settings with Cash Register","ITEMS":{"MODE":{"TYPE":"ENUM","LABEL":"Cash Register Operating Mode","OPTIONS":{"ACTIVE":"active","TEST":"test"}}}}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.cashbox.handler.add
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.cashbox.handler.add",
        {
            "CODE": "restcashbox01",
            "NAME": "REST-Cash Register 01",
            "SORT": 100,
            "SUPPORTS_FFD105": "Y",
            "SETTINGS":
            {
                "PRINT_URL": "http://example.com/rest_print.php",
                "CHECK_URL": "http://example.com/rest_check.php",
                "HTTP_VERSION": "1.1",
                "CONFIG":
                {
                    "AUTH": {
                        "LABEL": "Authorization",
                        "ITEMS": {
                            "KEYWORD": {
                                "TYPE": "STRING",
                                "LABEL": "Keyword"
                            },
                            "PREFERENCE": {
                                "TYPE": "ENUM",
                                "LABEL": "Multiple Choice",
                                "REQUIRED": "Y",
                                "OPTIONS": {
                                    "FIRST": "First",
                                    "SECOND": "Second",
                                    "THIRD": "Third",
                                }
                            }
                        }
                    },
                    "INTERACTION": {
                        "LABEL": "Interaction Settings with Cash Register",
                        "ITEMS": {
                            "MODE": {
                                "TYPE": "ENUM",
                                "LABEL": "Cash Register Operating Mode",
                                "OPTIONS": {
                                    "ACTIVE": "active",
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

    $result = CRest::call(
        'sale.cashbox.handler.add',
        [
            'CODE' => 'restcashbox01',
            'NAME' => 'REST-Cash Register 01',
            'SORT' => 100,
            'SUPPORTS_FFD105' => 'Y',
            'SETTINGS' =>
            [
                'PRINT_URL' => 'http://example.com/rest_print.php',
                'CHECK_URL' => 'http://example.com/rest_check.php',
                'HTTP_VERSION' => '1.1',
                'CONFIG' =>
                [
                    'AUTH' =>
                    [
                        'LABEL' => 'Authorization',
                        'ITEMS' =>
                        [
                            'KEYWORD' =>
                            [
                                'TYPE' => 'STRING',
                                'LABEL' => 'Keyword'
                            ],
                            'PREFERENCE' =>
                            [
                                'TYPE' => 'ENUM',
                                'LABEL' => 'Multiple Choice',
                                'REQUIRED' => 'Y',
                                'OPTIONS' =>
                                [
                                    'FIRST' => 'First',
                                    'SECOND' => 'Second',
                                    'THIRD' => 'Third',
                                ]
                            ]
                        ]
                    },
                    'INTERACTION' =>
                    [
                        'LABEL' => 'Interaction Settings with Cash Register',
                        'ITEMS' =>
                        [
                            'MODE' =>
                            [
                                'TYPE' => 'ENUM',
                                'LABEL' => 'Cash Register Operating Mode',
                                'OPTIONS' =>
                                [
                                    'ACTIVE' => 'active',
                                    'TEST' => 'test'
                                ]
                            }
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

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../../tutorials/sale/cashbox-add-example.md)

{% endnote %}

## Response Handling

HTTP Status: **200**

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
[`sale_cashbox_handler.ID`](../data-types.md#sale_cashbox_handler) | Identifier of the created handler ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_HANDLER_ALREADY_EXIST",
    "error_description": "Handler already exists!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient rights to add the handler | 403 ||
|| `ERROR_CHECK_FAILURE` | A required field value is missing or one of the field values is incorrect | 400 ||
|| `ERROR_HANDLER_ALREADY_EXIST` | A handler with the code specified in the `CODE` parameter already exists in the system | 400 ||
|| `ERROR_HANDLER_ADD` | Other errors. More detailed information about the error can be found in `error_description` | 400 ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## PRINT_URL Page {#print_url}

The `PRINT_URL` page is the address to which data for printing the receipt is sent.

### Example Data Sent to PRINT_URL

```
{
    "type": "sell",
    "calculated_sign": "income",
    "unique_id": 85,
    "items": [
        {
            "name": "Product",
            "base_price": 12000.0,
            "price": 9600.0,
            "sum": 9600.0,
            "currency": "USD",
            "quantity": 1,
            "measure_code": "796",
            "vat": 3,
            "vat_sum": 1600.0,
            "payment_object": "commodity_marking",
            "nomenclature_code": "DM �Yߠ�Q:4H7/3f^7fZ1",
            "marking_code": "011390002199781321Q:4H7/3f^7fZ1",
            "barcode": "1234567890",
            "discount": {
                "discount": 2400.0
            },
            "payment_method": "full_payment"
        }
    ],
    "date_create": 1712235417,
    "payments": [
        {
            "type": "cash",
            "is_cash": "Y",
            "sum": 1000,
            "currency": "USD"
        }
    ],
    "client_email": "client@example.com",
    "client_phone": "+123456789",
    "total_sum": 9600.0,
    "uuid": "check|example.com|85",
    "operation": "income",
    "number_kkm": "",
    "service_email": "admin@example.com",
    "cashbox_params": {
        "AUTH": {
            "LOGIN": "user123",
            "PASSWORD": "top_secret!"
        },
        "COMPANY": {
            "INN": "123"
        },
        "INTERACTION": {
            "MODE": "ACTIVE"
        }
    }
}
```

### Structure of POST Parameters Sent to PRINT_URL

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Type of the receipt. Values:
- `sell` — full payment
- `sellreturncash` — full cash return
- `sellreturn` — full non-cash return
- `advancepayment` — advance payment
- `advancereturn` — non-cash advance return
- `advancereturncash` — cash advance return
- `creditpayment` — credit payment
- `creditpaymentreturn` — non-cash credit payment return
- `creditpaymentreturncash` — cash credit payment return
- `credit` — purchase on credit
- `creditreturn` — return of a credit purchase
- `prepayment` — partial advance payment
- `prepaymentreturn` — non-cash return of partial advance payment
- `prepaymentreturncash` — cash return of partial advance payment
- `fullprepayment` — 100% advance payment
- `fullprepaymentreturn` — non-cash return of 100% advance payment
- `fullprepaymentreturncash` — cash return of 100% advance payment ||
|| **operation**
[`string`](../../data-types.md) | Indicator of income/expenditure. Values:
- `income` — income
- `consumption` — expenditure ||
|| **calculated**
[`string`](../../data-types.md) | Analogous to `operation` (for compatibility) ||
|| **unique_id**
[`integer`](../../data-types.md) | ID of the receipt in the portal database ||
|| **items**
[`object`](../../data-types.md) | Array of items in the receipt (detailed description provided [below](#items)) ||
|| **date_create**
[`integer`](../../data-types.md) | Date of receipt creation (`timestamp`) ||
|| **payments**
[`object`](../../data-types.md) | Array of payments (detailed description provided [below](#payments)) ||
|| **client_email**
[`string`](../../data-types.md) | Client's e-mail (if available) ||
|| **client_phone**
[`string`](../../data-types.md) | Client's phone number (if available) ||
|| **total_sum**
[`float`](../../data-types.md) | Total amount of the receipt ||
|| **uuid**
[`string`](../../data-types.md) | Identifier of the document in the external system (portal *Bitrix24*) ||
|| **service_email**
[`string`](../../data-types.md) | Email (from cash register settings) ||
|#

#### items Parameter {#items}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Name of the product ||
|| **base_price**
[`float`](../../data-types.md) | Price of the product without discounts and surcharges ||
|| **price**
[`float`](../../data-types.md) | Selling price ||
|| **sum**
[`float`](../../data-types.md) | Position amount ||
|| **quantity**
[`float`](../../data-types.md) | Quantity of the product ||
|| **vat**
[`int`](../../data-types.md) | Tax identifier. It can be used in the [catalog.vat.get](../../catalog/vat/catalog-vat-get.md) method to obtain information about the tax ||
|| **vat_sum**
[`float`](../../data-types.md) | Tax amount ||
|| **barcode**
[`string`](../../data-types.md) | Barcode. Used when inventory accounting is enabled and the product has a unique barcode ||
|| **nomenclature_code**
[`string`](../../data-types.md) | Nomenclature code in binary representation (if available) ||
|| **marking_code**
[`string`](../../data-types.md) | Nomenclature code (if available) ||
|| **payment_object**
[`string`](../../data-types.md) | Indicator of the payment item. Standard values: 
- `commodity` — product
- `commodity_marking` — product subject to marking and having a marking code
- `service` — service
- `payment` — payment
||
|| **payment_method**
[`string`](../../data-types.md) | Indicator of the payment method. Values:
- `full_payment` — full payment
- `advance` — advance
- `prepayment` — advance payment
- `full_prepayment` — 100% advance payment
- `credit` — purchase on credit
- `credit_payment` — credit payment
||
|| **supplier_info**
[`array`](../../data-types.md) | Agency details when using agency schemes (detailed description provided [below](#supplier_info)) ||
|| **discount**
[`array`](../../data-types.md) | Discount on the product. The key is deprecated and is no longer used. 
The array transmits the `discount` parameter ([`float`](../../data-types.md)) — the amount of the discount in monetary terms ||
|#

##### supplier_info Parameter {#supplier_info}

#|
|| **Name**
`type` | **Description** ||
|| **phones**
[`string[]`](../../data-types.md) | Phone numbers ||
|| **name**
[`string`](../../data-types.md) | Name of the supplier ||
|| **inn**
[`string`](../../data-types.md) | Supplier's INN ||
|#

#### payments Parameter {#payments}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Payment type. Values:
- `cash` — cash payment
- `cashless` — non-cash payment ||
|| **is_cash**
[`string`](../../data-types.md) | Indicates whether the payment is made in cash (`Y/N`). The key is deprecated; it is recommended to use `type` instead ||
|| **sum**
[`float`](../../data-types.md) | Payment amount ||
|| **currency**
[`string`](../../data-types.md) | Payment currency ||
|#

## CHECK_URL Page {#check_url}

The `CHECK_URL` page is the address where the success of the receipt printing is checked.

A request to the `CHECK_URL` address is made either by the manager's request or automatically after some time following the successful printing of the receipt.

### Example Data Sent to CHECK_URL

```
{
    "uuid": "00112233-4455-6677-8899-aabbccddeeff"
}
```

### Structure of POST Parameters Sent to CHECK_URL

#|
|| **Name**
`type` | **Description** ||
|| **uuid**
[`string`](../../data-types.md) | Identifier of the receipt ||
|#

## Continue Learning

- [{#T}](./sale-cashbox-handler-update.md)
- [{#T}](./sale-cashbox-handler-list.md)
- [{#T}](./sale-cashbox-handler-delete.md)
- [{#T}](./sale-cashbox-add.md)
- [{#T}](./sale-cashbox-update.md)
- [{#T}](./sale-cashbox-list.md)
- [{#T}](./sale-cashbox-delete.md)
- [{#T}](./sale-cashbox-check-apply.md)