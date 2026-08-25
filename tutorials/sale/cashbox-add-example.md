# How to Connect a Cash Register to Bitrix24

> Scope: [`cashbox`](../../api-reference/scopes/permissions.md)
>
> Who can execute methods: administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In Bitrix24, you can connect an external cash register and automatically print receipts. When a customer pays for an order, Bitrix24 sends the receipt data to the specified URL. An external service then generates and registers the fiscal receipt.

As a result of the scenario, a REST cash register will appear in the Sales Center, and the external service will be able to accept receipt-print requests and return the print status.

The scenario consists of three steps.

1. Add a cash register handler using the [sale.cashbox.handler.add](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md) method
2. Create a cash register and link it to the handler using the [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md) method
3. Pass the receipt print result using the [sale.cashbox.check.apply](../../api-reference/sale/cashbox/sale-cashbox-check-apply.md) method if the status must be retained manually

## Before You Start

Prepare the values used in the examples:

- an inbound webhook or an OAuth token of a user with administrator permissions
- a public HTTPS address of the receipt print page `PRINT_URL`
- a public HTTPS address of the receipt status page `CHECK_URL`
- a unique cash register handler code, for example `my_rest_cashbox`
- the authorization data of the external cash register that the administrator will enter in the cash register settings

Do not place login credentials, passwords, or access tokens in public code. Pass secrets through environment variables or secure application storage.

## 1. Add the Cash Register Handler

Register a handler using [sale.cashbox.handler.add](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md). Pass the handler configurations and the addresses to which the account sends requests for printing and checking the receipt status to the method.

- `CODE` — the unique code of the handler. We will specify `my_rest_cashbox`.

- `NAME` — the name of the handler, for example, My REST cash register.

- `SORT` — a number that determines the position of the handler in the list.

- `SETTINGS` — the handler settings object.

    - `PRINT_URL` — the address to which Bitrix24 sends the receipt data for printing. We will specify `https://example.com/rest_print.php`.

    - `CHECK_URL` — the address used to check the receipt status. We will pass `https://example.com/rest_check.php`.

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
               PRINT_URL: 'https://example.com/rest_print.php',
               CHECK_URL: 'https://example.com/rest_check.php',
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
                               REQUIRED: 'N',
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

- Python

   ```python
   from b24pysdk import BitrixWebhook, Client
   from b24pysdk.errors import BitrixAPIError


   token = BitrixWebhook(
       domain="your-domain.bitrix24.com",
       webhook_token="user_id/webhook_key",
   )
   client = Client(token)

   try:
       response = client.sale.cashbox.handler.add(
           code="my_rest_cashbox",
           name="My REST cash register",
           sort=100,
           settings={
               "PRINT_URL": "https://example.com/rest_print.php",
               "CHECK_URL": "https://example.com/rest_check.php",
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
                               "REQUIRED": "N",
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
           'PRINT_URL' => 'https://example.com/rest_print.php',
           'CHECK_URL' => 'https://example.com/rest_check.php',
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
                           'REQUIRED' => 'N',
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
{% endlist %}

If the handler is added successfully, the method returns its identifier. Store the `result` value: you will need it to find the handler in the list.

```json
{
    "result": 1
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
{% endlist %}

If the cashbox is added successfully, the method returns its identifier. Store the `result` value: you will need it to verify the cashbox in the list.

```json
{
    "result": 1
}
```

You can verify that the cashbox is connected to Bitrix24 in the Sales Center.

![Cash Register](_images/add_cash.png)

## Printing Receipts

The cash register uses two addresses. The account sends printing data to `PRINT_URL`. Bitrix24 uses `CHECK_URL` to check whether the receipt has been printed and if there are any errors.

### PRINT_URL Page

The `PRINT_URL` page is the address to which Bitrix24 sends the receipt data for printing. For the request structure, see the [PRINT_URL Page](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md#print_url) section of the `sale.cashbox.handler.add` method.

Input data processing, document generation, and the return of the printing result occur at `PRINT_URL`.

Minimum `PRINT_URL` page logic:

1. Receive receipt data from Bitrix24
2. Verify that the request contains receipt data
3. Pass the receipt to the external cash register
4. Return `UUID` if the receipt is accepted for printing, or an `ERRORS` array if printing is impossible
5. Store the relation between `UUID` and the order or receipt in the external system so that the `CHECK_URL` page can return the status

Example `PRINT_URL` handler in Node.js:

```js
import express from 'express'
import { randomUUID } from 'crypto'

const app = express()
app.use(express.json())
app.use(express.urlencoded({ extended: true }))

const checks = new Map()

app.post('/rest_print.php', async (req, res) => {
    const payload = req.body

    if (!payload || Object.keys(payload).length === 0) {
        res.json({
            ERRORS: ['Receipt data was not passed']
        })
        return
    }

    const uuid = randomUUID()

    checks.set(uuid, {
        status: 'WAIT',
        createdAt: Date.now(),
        payload
    })

    res.json({
        UUID: uuid
    })
})
```

In a production service, store `UUID`, the status, and the receipt data in a database rather than in process memory.

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

Minimum `CHECK_URL` page logic:

1. Receive the receipt `UUID`
2. Find the receipt in the external system
3. If the receipt is still printing, return `STATUS: WAIT`
4. If printing finished with an error, return `STATUS: ERROR` and the error text
5. If the receipt is printed, return `STATUS: DONE` and the fiscal details

Example `CHECK_URL` handler in Node.js:

```js
app.post('/rest_check.php', async (req, res) => {
    const uuid = req.body.UUID

    if (!uuid || !checks.has(uuid)) {
        res.json({
            STATUS: 'ERROR',
            ERROR: 'Receipt with this UUID was not found'
        })
        return
    }

    const check = checks.get(uuid)

    if (check.status === 'WAIT') {
        res.json({
            STATUS: 'WAIT'
        })
        return
    }

    if (check.status === 'ERROR') {
        res.json({
            STATUS: 'ERROR',
            ERROR: check.error
        })
        return
    }

    res.json({
        STATUS: 'DONE',
        UUID: uuid,
        REG_NUMBER_KKT: '000111222333',
        FISCAL_DOC_ATTR: '33445500',
        FISCAL_DOC_NUMBER: 123,
        FISCAL_RECEIPT_NUMBER: 10,
        FN_NUMBER: '0011223344556677',
        SHIFT_NUMBER: 12,
        PRINT_END_TIME: Math.floor(Date.now() / 1000)
    })
})
```

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

Data from `CHECK_URL` is retained in Bitrix24 and used to generate a link to the receipt.

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
{% endlist %}

If the receipt is successfully saved, the method returns `true`.

```json
{
    "result": true
}
```

## Check the Result

After configuring the cash register, open the Sales Center and verify that the REST cash register is available in the list of cash registers. After a test payment, verify that the external service received the request to `PRINT_URL`, returned `UUID`, and that Bitrix24 received the status through `CHECK_URL` or the `sale.cashbox.check.apply` method.

Through REST, check the handler and the cash register using the [sale.cashbox.handler.list](../../api-reference/sale/cashbox/sale-cashbox-handler-list.md) and [sale.cashbox.list](../../api-reference/sale/cashbox/sale-cashbox-list.md) methods.

{% list tabs %}

- JS

    ```js
    const handlerResponse = await $b24.actions.v2.call.make({
        method: 'sale.cashbox.handler.list',
        params: {},
        requestId: 'cashbox-handler-list',
    })

    const cashboxResponse = await $b24.actions.v2.call.make({
        method: 'sale.cashbox.list',
        params: {
            SELECT: ['ID', 'NAME', 'ACTIVE', 'EMAIL'],
            FILTER: { '=NAME': 'REST cash register' },
        },
        requestId: 'cashbox-list',
    })

    console.log(handlerResponse.getData().result)
    console.log(cashboxResponse.getData().result)
    ```

- Python

    ```python
    handlers = token.call_method("sale.cashbox.handler.list", {})
    cashboxes = token.call_method(
        "sale.cashbox.list",
        {
            "SELECT": ["ID", "NAME", "ACTIVE", "EMAIL"],
            "FILTER": {"=NAME": "REST cash register"},
        },
    )

    print(handlers)
    print(cashboxes)
    ```


- PHP

    ```php
    $handlerResponse = $sb->core->call('sale.cashbox.handler.list', []);

    $cashboxResponse = $sb->core->call('sale.cashbox.list', [
        'SELECT' => ['ID', 'NAME', 'ACTIVE', 'EMAIL'],
        'FILTER' => ['=NAME' => 'REST cash register'],
    ]);

    print_r($handlerResponse->getResponseData()->getResult());
    print_r($cashboxResponse->getResponseData()->getResult());
    ```
{% endlist %}

The scenario is successful if three results are confirmed:

- the `sale.cashbox.handler.add` method returned the handler identifier
- the `sale.cashbox.add` method returned the cash register identifier
- the `sale.cashbox.check.apply` method returned `true` if the print result was submitted manually

## Errors and Diagnostics

If a method returns an error, verify the request data and the external service state.

#|
|| **Error code or text** | **Cause and action** ||
|| `ACCESS_DENIED` | The method was called by a user without CRM administrator permissions to change settings ||
|| `ERROR_CHECK_FAILURE` | A required field is missing or a field value failed validation. Check `CODE`, `NAME`, `SETTINGS`, `REST_CODE`, `EMAIL`, and `UUID` ||
|| `ERROR_HANDLER_ALREADY_EXIST` | A handler with this `CODE` already exists. Specify a different code or reuse the existing handler ||
|| `ERROR_CHECK_NOT_FOUND` | The receipt with the specified `UUID` was not found. Check that `UUID` matches the value returned by the `PRINT_URL` page ||
|| `ERROR_HANDLER_ADD`, `ERROR_CASHBOX_ADD`, `ERROR_CHECK_APPLY` | An error occurred while adding the handler, creating the cash register, or saving the print result. Review `error_description` for details ||
|| Print error at `PRINT_URL` | Return the `ERRORS` array and retain the error text in the external service logs ||
|| `WAIT` status at `CHECK_URL` | The receipt has not been printed yet. Repeat the status check after the receipt is processed by the external cash register ||
|#

## What to Consider

- rerunning the example with the same `CODE` may fail because the handler code must be unique
- `PRINT_URL` and `CHECK_URL` must be publicly available over HTTPS
- store cash register settings data, such as login and password, on the application side and do not display it in the interface without masking

## Continue Learning

- [Cash Register Handlers](../../api-reference/sale/cashbox/sale-cashbox-handler-list.md)
- [Cash Registers](../../api-reference/sale/cashbox/sale-cashbox-list.md)
- [Saving the Receipt Print Result](../../api-reference/sale/cashbox/sale-cashbox-check-apply.md)
