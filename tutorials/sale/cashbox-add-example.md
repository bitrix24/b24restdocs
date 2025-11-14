# How to Connect a Cash Register to Bitrix24

> Scope: [`sale`](../../api-reference/scopes/permissions.md), [`cashbox`](../../api-reference/scopes/permissions.md)
>
> Who can execute methods: administrator

In Bitrix24, you can connect an external cash register and automatically print receipts. When a client pays for an order, the account sends the receipt data to a specified URL. The external service will generate and register the fiscal receipt.

To connect the cash register, we will sequentially execute the following methods:

1. [sale.cashbox.handler.add](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md) — we will add a cash register handler,

2. [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md) — we will create a cash register and link it to the handler.

## 1\. Add the Cash Register Handler

We will register the handler using [sale.cashbox.handler.add](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md). We will pass the handler settings and the addresses to which the account sends requests for printing and checking the receipt status.

- `CODE` — a unique code for the handler. We will specify `my_rest_cashbox`.

- `NAME` — the name of the handler, for example, `My REST Cash Register`.

- `SORT` — a number that determines the position of the handler in the list.

- `SETTINGS` — an object with the handler settings.

    - `PRINT_URL` — the address to which the account sends data for printing the receipt. We will specify `http://example.com/rest_print.php`.

    - `CHECK_URL` — the address for checking the receipt status. We will pass `http://example.com/rest_check.php`.

    - `CONFIG` — fields that need to be created for the handler. The administrator fills in these fields when setting up the cash register. We will create three blocks: `AUTH` — authorization by login and password, `COMPANY` — organization data, `INTERACTION` — cash register operation mode.

{% include [Note on Examples](../../_includes/examples.md) %}

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
                       "LABEL": "Organization Data",
                       "ITEMS": {
                           "INN": {
                               "TYPE": "STRING",
                               "REQUIRED": "Y",
                               "LABEL": "Organization INN"
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
                   'LABEL' => 'Organization Data',
                   'ITEMS' => [
                       'INN' => [
                           'TYPE' => 'STRING',
                           'REQUIRED' => 'Y',
                           'LABEL' => 'Organization INN'
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
                               'ACTIVE' => 'active',
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

If the handler is successfully added, the method will return its identifier. If you receive an `error`, check the description of possible errors in the documentation for the method [sale.cashbox.handler.add](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md).

```json
{
    "result": 1,
    "time": {
        "start":1761744611,
        "finish":1761744611.243273,
        "duration":0.24327301979064941,
        "processing":0,
        "date_start":"2025-10-29T16:30:11+02:00",
        "date_finish":"2025-10-29T16:30:11+02:00",
        "operating_reset_at":1761745211,
        "operating":0
    }
}
```

Now the handler can be used to create cash registers in the Bitrix24 interface. One handler can serve multiple cash registers with different details.

![Handler](_images/crm_cash_handler.png)

## 2\. Configure the Cash Register

We will add the cash register using [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md). We will pass the cash register settings and the values of the `CONFIG` parameter from the previous step.

- `REST_CODE` — the code of the handler. We will pass the value `my_rest_cashbox`, which we specified when adding the handler in the `CODE` parameter.

- `NAME` — the name of the cash register. We will specify `REST Cash Register`.

- `NUMBER_KKM` — the external identifier of the cash register, for example, `1`.

- `OFD` — the code of the OFD handler. We will pass `bx_firstofd`. A list of possible values can be found in the documentation for the method [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md).

- `EMAIL` — the email address for notifications. We will specify `owner@example.com`.

- `USE_OFFLINE` — a flag indicating whether the cash register is used offline. We will set the value to `Y`.

- `ACTIVE` — the activity status of the cash register. We will specify `Y`.

- `SETTINGS` — data for the fields created in the `CONFIG` parameter in the previous step. They need to be filled in exactly as described when registering the handler:

    - `AUTH` — login and password for authorization,

    - `COMPANY` — organization INN,

    - `INTERACTION` — operation mode, for example, `ACTIVE`.

{% list tabs %}

- JS

   ```js
   BX24.callMethod(
       "sale.cashbox.add",
       {
           "REST_CODE": "my_rest_cashbox",
           "NAME": "REST Cash Register",
           "NUMBER_KKM": "1",
           "OFD": "bx_firstofd",
           "EMAIL": "owner@example.com",
           "USE_OFFLINE": "Y",
           "ACTIVE": "Y",
           "SETTINGS": {
               "AUTH": {
                   "LOGIN": "rest_login",
                   "PASSWORD": "rest_password"
               },
               "COMPANY": {
                   "INN": "1234567890"
               },
               "INTERACTION": {
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
           'REST_CODE' => 'my_rest_cashbox',
           'NAME' => 'REST Cash Register',
           'NUMBER_KKM' => '1',
           'OFD' => 'bx_firstofd',
           'EMAIL' => 'owner@example.com',
           'USE_OFFLINE' => 'Y',
           'ACTIVE' => 'Y',
           'SETTINGS' => [
               'AUTH' => [
                   'LOGIN' => 'rest_login',
                   'PASSWORD' => 'rest_password'
               },
               'COMPANY' => [
                   'INN' => '1234567890'
               },
               'INTERACTION' => [
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

If the cash register is successfully added, the method will return its identifier. If you receive an `error`, check the description of possible errors in the documentation for the method [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md).

```json
{
    "result": 1,
    "time": {
        "start":1761771262,
        "finish":1761771262.111383,
        "duration":0.11138296127319336,
        "processing":0,
        "date_start":"2025-10-29T16:54:22+02:00",
        "date_finish":"2025-10-29T16:54:22+02:00",
        "operating_reset_at":1761771862,
        "operating":0
    }
}
```

In the Sales Center, you can check that the cash register is connected to Bitrix24.

![Cash Register](_images/add_cash.png)

## Printing Receipts

The cash register uses two addresses. The `PRINT_URL` is where the account sends data for printing. The `CHECK_URL` is where Bitrix24 checks if the receipt has been printed and if there are any errors.

### PRINT_URL Page

The `PRINT_URL` page is the address to which the account sends data for printing the receipt. The structure of the request can be found in the section [PRINT_URL Page](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md#print_url) of the method `sale.cashbox.handler.add`.

At the `PRINT_URL`, the incoming data is processed, the document is generated, and the print result is returned.

- If the print fails, the JSON array looks like this:

   ```json
   {
       "ERRORS": [
           "Error message",
           "Error message",
           ...
       ]
   }
   ```

- If the receipt has been sent for printing, the array looks like this:

   ```json
   {
       "UUID": "00112233-4455-6677-8899-aabbccddeeff"
   }
   ```

### CHECK_URL Page

The `CHECK_URL` page is the address where the account checks if the receipt is ready and if there are any errors.

The request to the `CHECK_URL` is executed by the manager's request or is automatically triggered after some time following the successful printing of the receipt. The structure of the request can be found in the section [CHECK_URL Page](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md#check_url) of the method `sale.cashbox.handler.add`.

The request to the `CHECK_URL` returns data about the receipt, error data if there was an error printing the receipt, or a status of "waiting for print".

- The data format in case of a printing error:

   ```json
   {
       "STATUS": "ERROR", 
       "ERROR": "Error message" 
   }
   ```

- The data format if the receipt has not been printed:

   ```json
   {
       "STATUS": "WAIT"
   }
   ```

- The data format upon successful receipt submission:

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

The complete list of fields matches the parameters of the method [sale.cashbox.check.apply](../../api-reference/sale/cashbox/sale-cashbox-check-apply.md).

Data from the `CHECK_URL` is saved on the account and used to generate a link to the receipt.

### Manually Submit Print Result

Receipt data can be submitted at any time using the method [sale.cashbox.check.apply](../../api-reference/sale/cashbox/sale-cashbox-check-apply.md).

Prepare the fields for `sale.cashbox.check.apply`.

- `UUID` — the unique identifier of the receipt returned by the handler in response to `PRINT_URL`.

- `PRINT_END_TIME` — the time the receipt printing ended.

- `REG_NUMBER_KKT` — the registration number of the cash register.

- `FISCAL_DOC_ATTR` — the fiscal attribute of the document generated by the cash register.

- `FISCAL_DOC_NUMBER` — the number of the fiscal document.

- `FISCAL_RECEIPT_NUMBER` — the receipt number within the shift.

- `FN_NUMBER` — the number of the fiscal accumulator.

- `SHIFT_NUMBER` — the shift number in which the receipt was included.

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
           'UUID' => '00112233-4455-6677-8899-aabbccddeeff',
           'PRINT_END_TIME' => '1609459200',
           'REG_NUMBER_KKT' => '000111222333',
           'FISCAL_DOC_ATTR' => '33445500',
           'FISCAL_DOC_NUMBER' => '123',
           'FISCAL_RECEIPT_NUMBER' => '10',
           'FN_NUMBER' => '0011223344556677',
           'SHIFT_NUMBER' => '12'
       ]
   );
   
   echo '<PRE>';
   print_r($result);
   echo '</PRE>';
   ```

{% endlist %}

If the receipt is successfully saved, the method will return `true`. If you receive an `error`, check the description of possible errors in the documentation for the method [sale.cashbox.check.apply](../../api-reference/sale/cashbox/sale-cashbox-check-apply.md).

```json
{
    "result": true,
    "time": {
        "start": 1712165362.026851,
        "finish": 1712165362.111383,
        "duration": 0.3808310031890869,
        "processing": 0.0336611270904541,
        "date_start": "2025-10-03T11:08:55+02:00",
        "date_finish": "2025-10-03T11:08:55+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```