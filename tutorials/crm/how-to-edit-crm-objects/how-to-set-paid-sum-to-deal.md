# How to Save the Paid Amount in the Deal Field

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — administrative access to the CRM section
>
> - [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) — a CRM administrator
> - [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) — a user with permission to read the deal the payments are selected from
> - [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) — a user with permission to modify items of a CRM object
> - [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) and [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) — a user with permission to read items of a CRM object
> - [crm.currency.base.get](../../../api-reference/crm/currency/crm-currency-base-get.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The deal amount shows the total cost rather than how much the customer has already transferred. A single deal can have several payments, some of them unpaid, and the amount field does not separate them. To let the manager see the amount already paid right in the deal card, create a separate Money field and store the total of the completed payments in it.

A Money field stores the amount and the currency in a single string: `1700|EUR`. The separator is a vertical bar, and the currency is written in three uppercase letters. If the format is broken, the method returns success while the field ends up empty, so build the value carefully.

The scenario continues the [{#T}](./how-to-set-paid-date-to-deal.md) tutorial. The earlier tutorial copies the payment date to the deal, this one copies the paid amount and the currency.

The scenario consists of four steps.

1. Create the Money field using the [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method
2. Retrieve the deal payments using the [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) method
3. Calculate the paid amount and write it to the field using the [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method
4. Compare the paid amount with the deal amount using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method

As a result, the deal card gets a field with the paid amount in the required currency, and the method response shows how much is still unpaid.

## Before You Start

Prepare the scenario data:

- **A deal with payments.** You will need its `id`. For deals, `entityTypeId` equals `2`
- **REST access.** A webhook or an application with the `crm` scope. Only a CRM administrator can create a Money field
- **Currency.** If the currency is not specified in the value, Bitrix24 substitutes the base currency. It is returned by the [crm.currency.base.get](../../../api-reference/crm/currency/crm-currency-base-get.md) method

The examples below use deal `8423` with three payments: `1000` is paid, `500` is unpaid, and `700` is paid. In your Bitrix24, substitute your own deal and your own payments.

For server-side JS examples with `B24Hook`, Node.js 18, 20, 22 or newer is required. For new projects, use version 22 or later. B24JsSDK is an ES module: save the code in an `.mjs` file or add `"type": "module"` to `package.json`. For examples with b24pysdk, Python 3.9 or newer is required.

Store the webhook URL in an environment variable and do not publish it in open code.

{% include [Example Note](../../../_includes/examples.md) %}

## 1. Create the Money Field

The [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method creates a custom field for all deals. Pass the parameters:

- `FIELD_NAME` — field code. The parameter is required. If the code does not start with `UF_CRM_`, the prefix is added automatically
- `USER_TYPE_ID` — field type, `money` for a Money field
- `MULTIPLE` — `N` for a single amount, `Y` if you need to store each payment as a separate string
- `EDIT_FORM_LABEL` — field title in the deal card, specified by language

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
    async function callMethod(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({
            method,
            params,
            requestId
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    const paidFieldId = await callMethod(
        'crm.deal.userfield.add',
        {
            fields: {
                FIELD_NAME: 'UF_CRM_MONEY_PAID',
                USER_TYPE_ID: 'money',
                MULTIPLE: 'N',
                EDIT_FORM_LABEL: { en: 'Paid amount' }
            }
        },
        'userfield-add-money'
    )

    console.log(paidFieldId)
    ```

- PHP

    ```php
    <?php

    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Psr\Log\NullLogger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
    function callMethod($serviceBuilder, string $method, array $params = []): mixed
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    $paidFieldId = callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_MONEY_PAID',
            'USER_TYPE_ID' => 'money',
            'MULTIPLE' => 'N',
            'EDIT_FORM_LABEL' => ['en' => 'Paid amount'],
        ],
    ]);

    print_r($paidFieldId);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    bitrix_token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )
    # B24_HOOK_TOKEN = 'USER_ID/TOKEN'

    def call_method(method, params=None):
        # A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
        return bitrix_token.call_method(
            api_method=method,
            params=params or {},
        )["result"]

    paid_field_id = call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_MONEY_PAID",
                "USER_TYPE_ID": "money",
                "MULTIPLE": "N",
                "EDIT_FORM_LABEL": {"en": "Paid amount"},
            },
        },
    )

    print(paid_field_id)
    ```

{% endlist %}

The response contains the field identifier:

```json
{
    "result": 6007785
}
```

The rest of the scenario uses universal methods, where the field code is written in a different case: `UF_CRM_MONEY_PAID` becomes `ufCrmMoneyPaid`. Check the exact spelling for your Bitrix24 in the response of the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method with `entityTypeId: 2`.

## 2. Retrieve the Deal Payments

The [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) method returns the payments of a single CRM object. Pass the parameters:

- `entityTypeId` — identifier of the [CRM object type](../../../api-reference/crm/data-types.md#object_type). For a deal, it is `2`
- `entityId` — deal identifier

Three fields of each payment are needed from the response:

- `paid` — payment flag, `Y` or `N`. Only payments with the `Y` value are summed up
- `sum` — payment amount as a number
- `currency` — payment currency, a three-letter code

{% list tabs %}

- JS

    ```js
    const payments = await callMethod(
        'crm.item.payment.list',
        {
            entityTypeId: 2,
            entityId: 8423
        },
        'payment-list'
    )

    const paidPayments = payments.filter((payment) => payment.paid === 'Y')

    console.table(paidPayments)
    ```

- PHP

    ```php
    $payments = callMethod($serviceBuilder, 'crm.item.payment.list', [
        'entityTypeId' => 2,
        'entityId' => 8423,
    ]);

    $paidPayments = array_values(
        array_filter($payments, static fn(array $payment): bool => $payment['paid'] === 'Y')
    );

    print_r($paidPayments);
    ```

- Python

    ```python
    payments = call_method(
        "crm.item.payment.list",
        {
            "entityTypeId": 2,
            "entityId": 8423,
        },
    )

    paid_payments = [payment for payment in payments if payment["paid"] == "Y"]

    print(paid_payments)
    ```

{% endlist %}

Abbreviated response:

```json
{
    "result": [
        {
            "id": 515,
            "accountNumber": "917/1",
            "paid": "Y",
            "datePaid": "2026-09-10T01:52:39+03:00",
            "sum": 1000,
            "currency": "EUR"
        },
        {
            "id": 517,
            "accountNumber": "917/2",
            "paid": "N",
            "datePaid": null,
            "sum": 500,
            "currency": "EUR"
        },
        {
            "id": 519,
            "accountNumber": "917/3",
            "paid": "Y",
            "datePaid": "2026-09-10T01:52:39+03:00",
            "sum": 700,
            "currency": "EUR"
        }
    ]
}
```

Two of the three payments are paid: `1000` and `700`. Their sum, `1700`, is the value for the Money field. The `500` payment is not included in the calculation.

## 3. Calculate the Paid Amount and Write It to the Field

The value of a Money field is built from the amount and the currency: first the number with a dot as the decimal separator, then a vertical bar, then the currency code in three uppercase letters. For this example, it is `1700|EUR`.

Only payments in the same currency can be summed up. If the currencies differ, calculate the amount for each of them separately and decide which one to write to the field or at which rate to convert.

The [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method writes the value to the deal. Pass the parameters:

- `entityTypeId` — `2` for a deal
- `id` — deal identifier
- `fields` — object with the field code in the format of universal methods, `ufCrmMoneyPaid` in the example

{% note warning "" %}

Check the format before sending. For the value `1700,00|EUR` with a comma, `1700.00|eur` in lowercase, or any other string that does not match the format, the method returns `true` while the field stays empty. There is no error.

{% endnote %}

{% list tabs %}

- JS

    ```js
    function buildMoneyValue(payments) {
        const currencies = new Set(payments.map((payment) => payment.currency))

        if (currencies.size > 1) {
            throw new Error(`Payments in different currencies: ${[...currencies].join(', ')}`)
        }

        const currency = [...currencies][0]
        const total = payments.reduce((sum, payment) => sum + Number(payment.sum), 0)

        return `${total.toFixed(2)}|${currency}`
    }

    const moneyValue = buildMoneyValue(paidPayments)

    await callMethod(
        'crm.item.update',
        {
            entityTypeId: 2,
            id: 8423,
            fields: { ufCrmMoneyPaid: moneyValue }
        },
        'item-update-money'
    )

    console.log(moneyValue)
    ```

- PHP

    ```php
    function buildMoneyValue(array $payments): string
    {
        $currencies = array_unique(array_column($payments, 'currency'));

        if (count($currencies) > 1) {
            throw new RuntimeException('Payments in different currencies: ' . implode(', ', $currencies));
        }

        $total = array_sum(array_map(
            static fn(array $payment): float => (float)$payment['sum'],
            $payments
        ));

        return number_format($total, 2, '.', '') . '|' . reset($currencies);
    }

    $moneyValue = buildMoneyValue($paidPayments);

    callMethod($serviceBuilder, 'crm.item.update', [
        'entityTypeId' => 2,
        'id' => 8423,
        'fields' => ['ufCrmMoneyPaid' => $moneyValue],
    ]);

    print_r($moneyValue);
    ```

- Python

    ```python
    def build_money_value(payments):
        currencies = {payment["currency"] for payment in payments}

        if len(currencies) > 1:
            raise RuntimeError(f"Payments in different currencies: {', '.join(sorted(currencies))}")

        total = sum(float(payment["sum"]) for payment in payments)

        return f"{total:.2f}|{currencies.pop()}"

    money_value = build_money_value(paid_payments)

    call_method(
        "crm.item.update",
        {
            "entityTypeId": 2,
            "id": 8423,
            "fields": {"ufCrmMoneyPaid": money_value},
        },
    )

    print(money_value)
    ```

{% endlist %}

The method returns the updated item. Abbreviated response:

```json
{
    "result": {
        "item": {
            "id": 8423,
            "ufCrmMoneyPaid": "1700|EUR"
        }
    }
}
```

The value comes back normalized: `1700.00` is stored as `1700`. Bitrix24 trims trailing zeros while a significant fractional part is kept, so `1750.25|EUR` stays unchanged.

## 4. Compare the Paid Amount with the Deal Amount

The [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method returns the entire deal. Three fields are of interest:

- `opportunity` — deal amount. It is calculated from the product rows and is not split into paid and unpaid parts
- `currencyId` — deal currency
- `ufCrmMoneyPaid` — the Money field with the paid amount

The difference between the deal amount and the paid amount is the outstanding balance.

{% list tabs %}

- JS

    ```js
    const item = await callMethod(
        'crm.item.get',
        { entityTypeId: 2, id: 8423 },
        'item-get-money'
    )

    const [paidSum, paidCurrency] = item.item.ufCrmMoneyPaid.split('|')
    const rest = Number(item.item.opportunity) - Number(paidSum)

    console.log(`Deal: ${item.item.opportunity} ${item.item.currencyId}`)
    console.log(`Paid: ${paidSum} ${paidCurrency}`)
    console.log(`Remaining: ${rest.toFixed(2)} ${item.item.currencyId}`)
    ```

- PHP

    ```php
    $item = callMethod($serviceBuilder, 'crm.item.get', [
        'entityTypeId' => 2,
        'id' => 8423,
    ]);

    [$paidSum, $paidCurrency] = explode('|', $item['item']['ufCrmMoneyPaid']);
    $rest = (float)$item['item']['opportunity'] - (float)$paidSum;

    echo 'Deal: ' . $item['item']['opportunity'] . ' ' . $item['item']['currencyId'] . PHP_EOL;
    echo 'Paid: ' . $paidSum . ' ' . $paidCurrency . PHP_EOL;
    echo 'Remaining: ' . number_format($rest, 2, '.', '') . ' ' . $item['item']['currencyId'] . PHP_EOL;
    ```

- Python

    ```python
    item = call_method("crm.item.get", {"entityTypeId": 2, "id": 8423})["item"]

    paid_sum, paid_currency = item["ufCrmMoneyPaid"].split("|")
    rest = float(item["opportunity"]) - float(paid_sum)

    print(f"Deal: {item['opportunity']} {item['currencyId']}")
    print(f"Paid: {paid_sum} {paid_currency}")
    print(f"Remaining: {rest:.2f} {item['currencyId']}")
    ```

{% endlist %}

Abbreviated response:

```json
{
    "result": {
        "item": {
            "id": 8423,
            "opportunity": 2000,
            "currencyId": "EUR",
            "ufCrmMoneyPaid": "1700|EUR"
        }
    }
}
```

The deal amount is `2000`, the paid amount is `1700`, and `300` is still outstanding. The `opportunity` field itself does not show this difference, which is exactly why a separate Money field is needed.

## Verify the Result

The scenario is complete if the Money field is filled in and its value matches the sum of the paid payments.

What to check in the responses:

- `ufCrmMoneyPaid` contains a string with the amount and the currency rather than `null` or an empty string
- the amount in the field equals the sum of the `sum` values of the payments where `paid` equals `Y`
- the currency code in the field matches the currency of the payments
- the difference between `opportunity` and the amount from the field equals the outstanding balance

Open the deal card in the interface: the Paid Amount field shows the amount with the currency symbol.

## Errors and Troubleshooting

If the method returns an error, check the request data.

#|
|| **Code or Error Text** | **Reason and Action** ||
|| `The 'FIELD_NAME' field is not found.` | The field code is not passed to [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md). Pass `FIELD_NAME` ||
|| `NOT_FOUND`, `Item not found` | The identifier of a nonexistent deal is passed to [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) or [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md). Check `id` and `entityTypeId` ||
|#

Format errors are silent: the method returns success while the value is lost. Check the field with the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method.

- The field comes back as `null` or an empty string: the value format did not match the expected one. Common reasons are a comma instead of a dot, a lowercase currency, spaces around the separator, or a currency symbol instead of a three-letter code
- The field contains the amount with the wrong currency: the currency was not passed, and the base currency of Bitrix24 was substituted. It can be retrieved with the [crm.currency.base.get](../../../api-reference/crm/currency/crm-currency-base-get.md) method
- The field contains an unfamiliar currency code: the code is not checked for existence. A string of three uppercase letters is stored even if there is no such currency in Bitrix24
- The amount in the field is larger than expected: unpaid payments got into the calculation. Filter for payments where `paid` is `Y`

To clear the field, pass an empty string to it.

## Key Points

- The value of a Money field is a string in the `amount|CURRENCY` format. The number is written with a dot, and the currency in three uppercase letters
- Trailing zeros are trimmed: `1700.00` is stored as `1700`, while `1750.25` stays unchanged
- Negative amounts are allowed, for example `-20|USD` for a refund
- If the currency is not specified, the base currency of Bitrix24 is substituted. In an integration, specify the currency explicitly: the base currency in another Bitrix24 can be different
- A Money field keeps a single setting when created through REST, `DEFAULT_VALUE`. Other keys in `SETTINGS` are not stored, so the value parsing rules cannot be changed through REST
- Payments in different currencies cannot be summed up. Calculate the amounts for each currency separately or convert them at your own rate
- A multiple Money field stores an array of such strings: `["1000|EUR", "700|EUR"]`. Each payment is stored as a separate string there, but the values have to be summed up on your side
- The `opportunity` deal amount is calculated from the product rows and does not answer the question of how much has already been paid
- The date of the last payment is transferred in the same way, as described in the [{#T}](./how-to-set-paid-date-to-deal.md) tutorial

## Code Example

The complete scenario in a single script: it creates the Money field, reads the deal payments, sums up the paid ones, writes the value, and compares it with the deal amount.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const DEAL_ID = 8423
    const DEAL_ENTITY_TYPE_ID = 2

    // A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
    async function callMethod(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    function buildMoneyValue(payments) {
        const currencies = new Set(payments.map((payment) => payment.currency))

        if (currencies.size === 0) {
            return ''
        }

        if (currencies.size > 1) {
            throw new Error(`Payments in different currencies: ${[...currencies].join(', ')}`)
        }

        const total = payments.reduce((sum, payment) => sum + Number(payment.sum), 0)

        return `${total.toFixed(2)}|${[...currencies][0]}`
    }

    async function main() {
        await callMethod('crm.deal.userfield.add', {
            fields: {
                FIELD_NAME: 'UF_CRM_MONEY_PAID',
                USER_TYPE_ID: 'money',
                MULTIPLE: 'N',
                EDIT_FORM_LABEL: { en: 'Paid amount' }
            }
        }, 'userfield-add-money')

        const payments = await callMethod('crm.item.payment.list', {
            entityTypeId: DEAL_ENTITY_TYPE_ID,
            entityId: DEAL_ID
        }, 'payment-list')

        const paidPayments = payments.filter((payment) => payment.paid === 'Y')
        const moneyValue = buildMoneyValue(paidPayments)

        await callMethod('crm.item.update', {
            entityTypeId: DEAL_ENTITY_TYPE_ID,
            id: DEAL_ID,
            fields: { ufCrmMoneyPaid: moneyValue }
        }, 'item-update-money')

        const item = await callMethod('crm.item.get', {
            entityTypeId: DEAL_ENTITY_TYPE_ID,
            id: DEAL_ID
        }, 'item-get-money')

        const [paidSum, paidCurrency] = (item.item.ufCrmMoneyPaid ?? '|').split('|')
        const rest = Number(item.item.opportunity) - Number(paidSum || 0)

        console.log(`Deal: ${item.item.opportunity} ${item.item.currencyId}`)
        console.log(`Paid: ${paidSum} ${paidCurrency}`)
        console.log(`Remaining: ${rest.toFixed(2)} ${item.item.currencyId}`)
    }

    main().catch((error) => console.error(error.message))
    ```

- PHP

    ```php
    <?php

    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Psr\Log\NullLogger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    const DEAL_ID = 8423;
    const DEAL_ENTITY_TYPE_ID = 2;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
    function callMethod($serviceBuilder, string $method, array $params = []): mixed
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    function buildMoneyValue(array $payments): string
    {
        $currencies = array_unique(array_column($payments, 'currency'));

        if ($currencies === []) {
            return '';
        }

        if (count($currencies) > 1) {
            throw new RuntimeException('Payments in different currencies: ' . implode(', ', $currencies));
        }

        $total = array_sum(array_map(
            static fn(array $payment): float => (float)$payment['sum'],
            $payments
        ));

        return number_format($total, 2, '.', '') . '|' . reset($currencies);
    }

    callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_MONEY_PAID',
            'USER_TYPE_ID' => 'money',
            'MULTIPLE' => 'N',
            'EDIT_FORM_LABEL' => ['en' => 'Paid amount'],
        ],
    ]);

    $payments = callMethod($serviceBuilder, 'crm.item.payment.list', [
        'entityTypeId' => DEAL_ENTITY_TYPE_ID,
        'entityId' => DEAL_ID,
    ]);

    $paidPayments = array_values(
        array_filter($payments, static fn(array $payment): bool => $payment['paid'] === 'Y')
    );

    $moneyValue = buildMoneyValue($paidPayments);

    callMethod($serviceBuilder, 'crm.item.update', [
        'entityTypeId' => DEAL_ENTITY_TYPE_ID,
        'id' => DEAL_ID,
        'fields' => ['ufCrmMoneyPaid' => $moneyValue],
    ]);

    $item = callMethod($serviceBuilder, 'crm.item.get', [
        'entityTypeId' => DEAL_ENTITY_TYPE_ID,
        'id' => DEAL_ID,
    ]);

    [$paidSum, $paidCurrency] = explode('|', (string)$item['item']['ufCrmMoneyPaid'] . '|');
    $rest = (float)$item['item']['opportunity'] - (float)$paidSum;

    echo 'Deal: ' . $item['item']['opportunity'] . ' ' . $item['item']['currencyId'] . PHP_EOL;
    echo 'Paid: ' . $paidSum . ' ' . $paidCurrency . PHP_EOL;
    echo 'Remaining: ' . number_format($rest, 2, '.', '') . ' ' . $item['item']['currencyId'] . PHP_EOL;
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    DEAL_ID = 8423
    DEAL_ENTITY_TYPE_ID = 2

    bitrix_token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )
    # B24_HOOK_TOKEN = 'USER_ID/TOKEN'

    def call_method(method, params=None):
        # A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
        return bitrix_token.call_method(
            api_method=method,
            params=params or {},
        )["result"]

    def build_money_value(payments):
        currencies = {payment["currency"] for payment in payments}

        if not currencies:
            return ""

        if len(currencies) > 1:
            raise RuntimeError(f"Payments in different currencies: {', '.join(sorted(currencies))}")

        total = sum(float(payment["sum"]) for payment in payments)

        return f"{total:.2f}|{currencies.pop()}"

    call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_MONEY_PAID",
                "USER_TYPE_ID": "money",
                "MULTIPLE": "N",
                "EDIT_FORM_LABEL": {"en": "Paid amount"},
            },
        },
    )

    payments = call_method(
        "crm.item.payment.list",
        {"entityTypeId": DEAL_ENTITY_TYPE_ID, "entityId": DEAL_ID},
    )

    paid_payments = [payment for payment in payments if payment["paid"] == "Y"]
    money_value = build_money_value(paid_payments)

    call_method(
        "crm.item.update",
        {
            "entityTypeId": DEAL_ENTITY_TYPE_ID,
            "id": DEAL_ID,
            "fields": {"ufCrmMoneyPaid": money_value},
        },
    )

    item = call_method(
        "crm.item.get",
        {"entityTypeId": DEAL_ENTITY_TYPE_ID, "id": DEAL_ID},
    )["item"]

    paid_sum, _, paid_currency = (item["ufCrmMoneyPaid"] or "").partition("|")
    rest = float(item["opportunity"]) - float(paid_sum or 0)

    print(f"Deal: {item['opportunity']} {item['currencyId']}")
    print(f"Paid: {paid_sum} {paid_currency}")
    print(f"Remaining: {rest:.2f} {item['currencyId']}")
    ```

{% endlist %}

## Continue Learning

- [{#T}](./how-to-set-paid-date-to-deal.md)
- [{#T}](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md)
- [{#T}](../../../api-reference/crm/universal/payment/crm-item-payment-list.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-update.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-get.md)
- [{#T}](../../../api-reference/crm/currency/crm-currency-base-get.md)
