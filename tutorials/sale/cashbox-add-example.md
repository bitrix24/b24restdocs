# How to Connect a Cash Register to Bitrix24

> Scope: [`sale`](../../api-reference/scopes/permissions.md), [`cashbox`](../../api-reference/scopes/permissions.md)
>
> Who can execute methods: administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In Bitrix24, you can connect an external cash register and automatically print receipts. When a customer pays for an order, the account sends the receipt data to a specified URL. An external service then generates and registers the fiscal receipt.

To connect a cash register, perform the following methods in sequence:

1. [sale.cashbox.handler.add](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md) — add a cash register handler,

2. [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md) — create a cash register and link it to the handler.

## 1\. Add the Cash Register Handler

Register a handler using [sale.cashbox.handler.add](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md). Pass the handler configurations and the addresses to which the account sends requests for printing and checking the receipt status to the method.

- `CODE` — the unique code of the handler. We will specify `my_rest_cashbox`.

- `NAME` — the name of the handler, for example, My REST cash register.

- `SORT` — a number that determines the position of the handler in the list.

- `SETTINGS` — the handler settings object.

    - `PRINT_URL` — the address to which the account sends data for printing the receipt. We will specify `http://example.com/rest_print.php`.

    - `CHECK_URL` — the address used to check the receipt status. We will pass `http://example.com/rest_check.php`.

    - `CONFIG` — the fields that need to be created for the handler. An Administrator fills in these fields when configuring the cash register. We will create three blocks: `AUTH` — authorization via login and password, `COMPANY` — company details, `INTERACTION` — cash register operation mode.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

   ```js
   import { B24Hook } from '@bitrix24/b24jssdk'

   const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
   // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

   const response = await $b24.actions.v2.call.make({
       method: 'sale.cashbox.handler.add',
       params: {
           CODE: 'my_rest_cashbox',
           NAME: 'My REST cash register',
           SORT: 100,
           SETTINGS: {
               PRINT_URL: 'http://example.com/rest_print.php',
               CHECK_URL: 'http://example.com/rest_check.php',
               CONFIG: {
                   AUTH: {
                       LABEL: 'Authorization',
                       ITEMS: {
                           LOGIN: {
                               TYPE: 'STRING',
                               REQUIRED: 'Y',
                               LABEL: 'Login'
                           },
                           PASSWORD: {
                               TYPE: 'STRING',
                               REQUIRED: 'Y',
                               LABEL: 'Password'
                           }
                       }
                   },
                   COMPANY: {
                       LABEL: 'Organization data',
                       ITEMS: {
                           INN: {
                               TYPE: 'STRING',
                               REQUIRED: 'Y',
                               LABEL: 'Tax ID of the organization'
                           }
                       }
                   },
                   INTERACTION: {
                       LABEL: 'Cash register interaction settings',
                       ITEMS: {
                           MODE: {
                               TYPE: 'ENUM',
                               LABEL: 'Cash register operating mode',
                               OPTIONS: {
                                   ACTIVE: 'production',
                                   TEST: 'test'
                               }
                           }
                       }
                   }
               }
           }
       },
       requestId: 'cashbox-handler-add'
   })

   if (response.isSuccess) {
       console.dir(response.getData().result)
   } else {
       console.error(response.getErrorMessages().join('; '))
   }
   ```

- PHP

   ```php
   <?php
   // composer require bitrix24/b24phpsdk:"^3.0"
   require_once 'vendor/autoload.php';

   use Bitrix24\SDK\Services\ServiceBuilderFactory;
   use Symfony\Component\EventDispatcher\EventDispatcher;
   use Monolog\Logger;
   use Monolog\Handler\StreamHandler;

   $log = new Logger('b24');
   $log->pushHandler(new StreamHandler('php://stdout'));

   $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
       ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

   $result = $sb->getSaleScope()->cashboxHandler()->add(
       'my_rest_cashbox',
       'My REST cash register',
       [
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
                   'LABEL' => 'Organization data',
                   'ITEMS' => [
                       'INN' => [
                           'TYPE' => 'STRING',
                           'REQUIRED' => 'Y',
                           'LABEL' => 'Tax ID of the organization'
                       ]
                   ]
               ],
               'INTERACTION' => [
                   'LABEL' => 'Cash register interaction settings',
                   'ITEMS' => [
                       'MODE' => [
                           'TYPE' => 'ENUM',
                           'LABEL' => 'Cash register operating mode',
                           'OPTIONS' => [
                               'ACTIVE' => 'production',
                               'TEST' => 'test'
                           ]
                       ]
                   ]
               ]
           ]
       ],
       100
   );

   echo '<PRE>';
   print_r($result->getId());
   echo '</PRE>';
   ```

- Python

   ```python
   from b24pysdk import BitrixWebhook, Client
   from b24pysdk.errors import BitrixAPIError


   client = Client(
       BitrixWebhook(
           domain="your-domain.bitrix24.com",
           webhook_token="user_id/webhook_key",
       )
   )

   try:
       response = client.sale.cashbox.handler.add(
       code="my_rest_cashbox",
       name="My REST cash register",
       sort=100,
       settings={
           "PRINT_URL": "http://example.com/rest_print.php",
           "CHECK_URL": "http://example.com/rest_check.php",
           "CONFIG": {
               "AUTH": {
                   "LABEL": "Authorization",
                   "ITEMS": {
                       "LOGIN": {
                           "TYPE": "STRING",
                           "REQUIRED": "Y",
                           "LABEL": "Login",
                       },
                       "PASSWORD": {
                           "TYPE": "STRING",
                           "REQUIRED": "Y",
                           "LABEL": "Password",
                       },
                   },
               },
               "COMPANY": {
                   "LABEL": "Organization data",
                   "ITEMS": {
                       "INN": {
                           "TYPE": "STRING",
                           "REQUIRED": "Y",
                           "LABEL": "Tax ID of the organization",
                       }
                   },
               },
               "INTERACTION": {
                   "LABEL": "Cash register interaction settings",
                   "ITEMS": {
                       "MODE": {
                           "TYPE": "ENUM",
                           "LABEL": "Cash register operating mode",
                           "OPTIONS": {
                               "ACTIVE": "production",
                               "TEST": "test",
                           },
                       }
                   },
               },
           },
       },
       ).response
       print(response.result)
   except BitrixAPIError as error:
       print(error)
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
        "date_start":"2025-10-29T16:30:11+03:00",
        "date_finish":"2025-10-29T16:30:11+03:00",
        "operating_reset_at":1761745211,
        "operating":0
    }
}
```

The handler can now be used to create cash registers in the Bitrix24 interface. One handler can serve multiple cash registers with different company details.

![Handler](_images/crm_cash_handler.png)

## 2. Configure the Cashbox

Add a cashbox using [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md). Pass the cashbox configurations and the value of the `CONFIG` parameter from the previous step to the method.

- `REST_CODE` — handler code. Pass the value `my_rest_cashbox`, which you specified when adding a handler in the parameter `CODE`.

- `NAME` — cashbox name. Specify REST cash register.

- `NUMBER_KKM` — external cashbox identifier, for example, `1`.

- `OFD` — OFD handler code. Pass `bx_firstofd`. See the list of possible values in the [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md) method documentation.

- `EMAIL` — email address for notifications. Specify `owner@example.com`.

- `USE_OFFLINE` — flag indicating whether the cashbox is used offline. Set the value `Y`.

- `ACTIVE` — cashbox activity. Specify `Y`.

- `SETTINGS` — data for the fields created in the `CONFIG` parameter in the previous step. They must be filled out exactly as described during the handler registration:

    - `AUTH` — login and password for authorization,

    - `COMPANY` — organization Taxpayer ID,

    - `INTERACTION` — operating mode, for example, `ACTIVE`.

{% list tabs %}

- JS

   ```js
   const response = await $b24.actions.v2.call.make({
       method: 'sale.cashbox.add',
       params: {
           REST_CODE: 'my_rest_cashbox',
           NAME: 'REST cash register',
           NUMBER_KKM: '1',
           OFD: 'bx_firstofd',
           EMAIL: 'owner@example.com',
           USE_OFFLINE: 'Y',
           ACTIVE: 'Y',
           SETTINGS: {
               AUTH: {
                   LOGIN: 'rest_login',
                   PASSWORD: 'rest_password'
               },
               COMPANY: {
                   INN: '1234567890'
               },
               INTERACTION: {
                   MODE: 'ACTIVE'
               }
           }
       },
       requestId: 'cashbox-add'
   })

   if (response.isSuccess) {
       console.dir(response.getData().result)
   } else {
       console.error(response.getErrorMessages().join('; '))
   }
   ```

- PHP

   ```php
   $result = $sb->getSaleScope()->cashbox()->add([
       'REST_CODE' => 'my_rest_cashbox',
       'NAME' => 'REST cash register',
       'NUMBER_KKM' => '1',
       'OFD' => 'bx_firstofd',
       'EMAIL' => 'owner@example.com',
       'USE_OFFLINE' => 'Y',
       'ACTIVE' => 'Y',
       'SETTINGS' => [
           'AUTH' => [
               'LOGIN' => 'rest_login',
               'PASSWORD' => 'rest_password'
           ],
           'COMPANY' => [
               'INN' => '1234567890'
           ],
           'INTERACTION' => [
               'MODE' => 'ACTIVE'
           ]
       ]
   ]);

   echo '<PRE>';
   print_r($result->getId());
   echo '</PRE>';
   ```

- Python

   ```python
   try:
       response = client.sale.cashbox.add(
       rest_code="my_rest_cashbox",
       name="REST cash register",
       email="owner@example.com",
       number_kkm="1",
       ofd="bx_firstofd",
       use_offline=True,
       active=True,
       settings={
           "AUTH": {
               "LOGIN": "rest_login",
               "PASSWORD": "rest_password",
           },
           "COMPANY": {
               "INN": "1234567890",
           },
           "INTERACTION": {
               "MODE": "ACTIVE",
           },
       },
       ).response
       print(response.result)
   except BitrixAPIError as error:
       print(error)
   ```

{% endlist %}

If the cashbox is successfully added, the method returns its identifier. If you receive error `error`, review the description of possible errors in the [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md) method documentation.

```json
{
    "result": 1,
    "time": {
        "start":1761771262,
        "finish":1761771262.111383,
        "duration":0.11138296127319336,
        "processing":0,
        "date_start":"2025-10-29T16:54:22+03:00",
        "date_finish":"2025-10-29T16:54:22+03:00",
        "operating_reset_at":1761771862,
        "operating":0
    }
}
```

You can verify that the cashbox is connected to Bitrix24 in the Sales Center.

![Cash Register](_images/add_cash.png)

## Printing Receipts

The cash register uses two addresses. The account sends printing data to `PRINT_URL`. Bitrix24 uses `CHECK_URL` to check whether the receipt has been printed and if there are any errors.

### PRINT_URL Page

The `PRINT_URL` page is the address to which the account sends data to print a receipt. For the request structure, see the [PRINT_URL Page](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md#print_url) section of the `sale.cashbox.handler.add` method.

Input data processing, document generation, and the return of the printing result occur at `PRINT_URL`.

- If printing fails, the JSON array looks like this:

   ```json
   {
       "ERRORS": [
           "Error message",
           "Error message",
           ...
       ]
   }
   ```

- If the receipt is sent to print, the array looks like this:

   ```json
   {
       "UUID": "00112233-4455-6677-8899-aabbccddeeff"
   }
   ```

### CHECK_URL Page

The `CHECK_URL` page is the address used by the account to check if the receipt is ready and if there are any errors.

A request to `CHECK_URL` is performed upon a manager's request or is triggered automatically after a certain amount of time following a successful receipt print. For the request structure, see the [CHECK_URL Page](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md#check_url) section of the `sale.cashbox.handler.add` method.

A request to `CHECK_URL` returns data about the receipt, error data if a printing error occurred, or an "idle" status.

- Data format in case of a printing error:

   ```json
   {
       "STATUS": "ERROR", 
       "ERROR": "Error message" 
   }
   ```

- Data format if the receipt has not been printed:

   ```json
   {
       "STATUS": "WAIT"
   }
   ```

- Data format upon successful receipt submission:

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

The full list of fields matches the parameters of the [sale.cashbox.check.apply](../../api-reference/sale/cashbox/sale-cashbox-check-apply.md) method.

Data from `CHECK_URL` is retained on the account and used to generate a link to the receipt.

### Manually Submit Print Result

Receipt data can be submitted at any time using the [sale.cashbox.check.apply](../../api-reference/sale/cashbox/sale-cashbox-check-apply.md) method.

Prepare the fields for `sale.cashbox.check.apply`.

- `UUID` — the unique ID of the receipt returned by the handler in the response to `PRINT_URL`.

- `PRINT_END_TIME` — the receipt printing End Time.

- `REG_NUMBER_KKT` — the cash register registration number.

- `FISCAL_DOC_ATTR` — the fiscal attribute of the document generated by the cash register.

- `FISCAL_DOC_NUMBER` — the fiscal document number.

- `FISCAL_RECEIPT_NUMBER` — the receipt number within the shift.

- `FN_NUMBER` — the fiscal storage device number.

- `SHIFT_NUMBER` — the shift number in which the receipt was recorded.

{% list tabs %}

- JS

   ```js
   const response = await $b24.actions.v2.call.make({
       method: 'sale.cashbox.check.apply',
       params: {
           UUID: '00112233-4455-6677-8899-aabbccddeeff',
           PRINT_END_TIME: '1609459200',
           REG_NUMBER_KKT: '000111222333',
           FISCAL_DOC_ATTR: '33445500',
           FISCAL_DOC_NUMBER: '123',
           FISCAL_RECEIPT_NUMBER: '10',
           FN_NUMBER: '0011223344556677',
           SHIFT_NUMBER: '12'
       },
       requestId: 'cashbox-check-apply'
   })

   if (response.isSuccess) {
       console.dir(response.getData().result)
   } else {
       console.error(response.getErrorMessages().join('; '))
   }
   ```

- PHP

   ```php
   $result = $sb->getSaleScope()->cashbox()->checkApply([
       'UUID' => '00112233-4455-6677-8899-aabbccddeeff',
       'PRINT_END_TIME' => '1609459200',
       'REG_NUMBER_KKT' => '000111222333',
       'FISCAL_DOC_ATTR' => '33445500',
       'FISCAL_DOC_NUMBER' => '123',
       'FISCAL_RECEIPT_NUMBER' => '10',
       'FN_NUMBER' => '0011223344556677',
       'SHIFT_NUMBER' => '12'
   ]);

   echo '<PRE>';
   print_r($result->isSuccess());
   echo '</PRE>';
   ```

- Python

    ```python
    try:
        response = client.sale.cashbox.check.apply(
        uuid="00112233-4455-6677-8899-aabbccddeeff",
        print_end_time="1609459200",
        reg_number_kkt="000111222333",
        fiscal_doc_attr="33445500",
        fiscal_doc_number="123",
        fiscal_receipt_number="10",
        fn_number="0011223344556677",
        shift_number="12",
        ).response
        print(response.result)
    except BitrixAPIError as error:
        print(error)
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
