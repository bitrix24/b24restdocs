# How to Save the Payment Date in the Deal Field

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — permission to modify items of a CRM object
>
> - [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) — a user with permission to modify items of a CRM object
> - [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) and [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — a user with permission to read items of a CRM object
> - [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) — a user with permission to read the CRM object whose payments are selected

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 retains the payment date in the payment document, not in the deal itself. The deal card does not show this date, and the regular deal filter does not see it. That is why the payment date is often duplicated into a custom deal field: integrations with external systems, BI Builder reports, automation rules, and workflows take it from there.

The identifier of a custom field is different in every Bitrix24 and cannot be hardcoded as a constant. The field therefore has to be located by its name every time.

As a result of the scenario, the payment date appears in the "Payment Date" field of the deal card, and `crm.item.update` returns the deal with the new field value.

The scenario consists of three steps.

1. Find the identifier of the deal field using the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method
2. Retrieve the payment date using the [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) method
3. Write the date to the deal field using the [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method

## Before You Start

- The webhook is created on behalf of a user who has permission to modify deals in CRM

- The `crm` scope is selected in the webhook permissions

- The webhook URL grants full access within its scope. Retain the URL in an environment variable and never publish it in open code

- A custom field for the payment date is created in the deal card in advance. It is added in the deal card settings or with the [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method. How to choose the field type is described in the [Key Considerations](#date-type) section

- You know the `id` of the deal the date is transferred for. It can be found in the URL of the deal card or with the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method

- At least one payment is completed for this deal. If there are no payments, step 2 returns an empty array and there is nothing to write to the deal

The examples below use the deal `6917` and the field named "Payment Date".

## 1. Find the Deal Field Identifier {#field-name}

Use the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method with the following parameter:

- `entityTypeId` — the identifier of the [CRM object type](../../../api-reference/crm/data-types.md#object_type), a required parameter. Pass `2` — a deal

The method returns a `fields` object: the key is the field identifier, the value is its settings. Locate the required field by iterating over two attributes:

- `title` — the field name the user sees in the card. Look for "Payment Date"

- `type` — the field type. Check that it is `date` or `datetime`, so that a string field named something like "Payment Date" does not break the selection

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const resultFields = await $b24.actions.v2.call.make({
        method: 'crm.item.fields',
        params: {
            entityTypeId: 2 // 2 — deal
        },
        requestId: 'item-fields'
    });

    const fields = resultFields.getData().result.fields;
    const fieldName = Object.keys(fields).find(
        key => fields[key].title === 'Payment Date'
            && ['date', 'datetime'].includes(fields[key].type)
    );
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
        )
    )

    fields = client.crm.item.fields(
        2,  # 2 — deal
    ).response.result["fields"]

    field_name = next(
        (
            key
            for key, settings in fields.items()
            if settings["title"] == "Payment Date" and settings["type"] in ("date", "datetime")
        ),
        None,
    )
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // crm.item.fields has no wrapper in the SDK — call the method directly
    $resultFields = $sb->core->call(
        'crm.item.fields',
        [ 'entityTypeId' => 2 ] // 2 — deal
    );

    $fields = $resultFields->getResponseData()->getResult()['fields'];

    $fieldName = null;
    foreach ($fields as $key => $settings) {
        if ($settings['title'] === 'Payment Date' && in_array($settings['type'], ['date', 'datetime'], true)) {
            $fieldName = $key;
            break;
        }
    }
    ```
{% endlist %}

Retain the identifier you found — step 3 needs it. In the example it is `ufCrm_1746431727372`. The response is shortened to a single field: the method returns the entire set of deal fields.

```json
{
    "result": {
        "fields": {
            "ufCrm_1746431727372": {
                "type": "date",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": true,
                "title": "Payment Date",
                "listLabel": "Payment Date",
                "formLabel": "Payment Date",
                "filterLabel": "Payment Date",
                "settings": {
                    "DEFAULT_VALUE": {
                        "TYPE": "NONE",
                        "VALUE": ""
                    }
                },
                "upperName": "UF_CRM_1746431727372"
            }
        }
    }
}
```

The `isDynamic`: `true` attribute confirms that the field is custom rather than system. `upperName` holds the same field in the legacy spelling — `UF_CRM_1746431727372`. By default, the [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method understands only the variant from the key, so step 3 passes exactly `ufCrm_1746431727372`.

## 2. Retrieve the Payment Date {#date}

Use the [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) method with the following parameters:

- `entityTypeId` — the identifier of the [CRM object type](../../../api-reference/crm/data-types.md#object_type), a required parameter. Pass `2` — a deal

- `entityId` — the identifier of the deal the payments are retrieved for, a required parameter. `6917` in the example

{% list tabs %}

- JS

    ```javascript
    const resultPayments = await $b24.actions.v2.call.make({
        method: 'crm.item.payment.list',
        params: {
            entityTypeId: 2,
            entityId: 6917
        },
        requestId: 'payment-list'
    });

    const payments = resultPayments.getData().result;
    ```

- Python

    ```python
    payments = client.crm.item.payment.list(
        entity_type_id=2,
        entity_id=6917,
    ).response.result
    ```


- PHP

    ```php
    // crm.item.payment.list has no wrapper in the SDK — call the method directly
    $resultPayments = $sb->core->call(
        'crm.item.payment.list',
        [
            'entityTypeId' => 2,
            'entityId' => 6917
        ]
    );

    $payments = $resultPayments->getResponseData()->getResult();
    ```
{% endlist %}

The method returns an array of the deal payments. Take the payment date from the `datePaid` field, and use the `paid` field to check that the payment is actually completed: an unpaid document has `paid` equal to `N` and an empty `datePaid`.

```json
{
    "result": [
        {
            "id": 503,
            "accountNumber": "831/1",
            "paid": "Y",
            "datePaid": "2025-04-29T13:03:20+03:00",
            "empPaidId": 1,
            "paySystemId": 19,
            "sum": 15,
            "currency": "EUR",
            "paySystemName": "Card Payment"
        }
    ]
}
```

## 3. Write the Date to the Deal Field

Use the [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method with the following parameters:

- `entityTypeId` — `2` for a deal

- `id` — the identifier of the deal, `6917` in the example

- `fields[ufCrm_1746431727372]` — the field identifier from [step 1](#field-name). Pass `datePaid` from [step 2](#date) as the value

{% list tabs %}

- JS

    ```javascript
    const resultUpdate = await $b24.actions.v2.call.make({
        method: 'crm.item.update',
        params: {
            entityTypeId: 2,
            id: 6917,
            fields: {
                // the key is the field identifier from step 1, the value is datePaid from step 2
                [fieldName]: payments[0].datePaid
            }
        },
        requestId: 'item-update'
    });
    ```

- Python

    ```python
    result_update = client.crm.item.update(
        2,
        6917,
        {
            # the key is the field identifier from step 1, the value is datePaid from step 2
            field_name: payments[0]["datePaid"],
        },
    ).response.result["item"]
    ```


- PHP

    ```php
    $resultUpdate = $sb->getCRMScope()->item()->update(
        2,
        6917,
        [
            // the key is the field identifier from step 1, the value is datePaid from step 2
            $fieldName => $payments[0]['datePaid']
        ]
    );
    ```
{% endlist %}

The method returns the entire deal with the new field value, so a separate request to check the write is not required. The response is shortened to the fields that confirm the write.

```json
{
    "result": {
        "item": {
            "id": 6917,
            "title": "Deal #6531",
            "stageId": "C9:NEW",
            "opportunity": 30,
            "currencyId": "EUR",
            "updatedTime": "2026-08-20T09:14:13+03:00",
            "ufCrm_1746431727372": "2025-04-29T03:00:00+03:00"
        }
    }
}
```

{% note warning "" %}

`2025-04-29T13:03:20+03:00` was written, while `2025-04-29T03:00:00+03:00` came back in the response. This is not an error: the field has the "Date" type, so the time is not retained. The time in the response is technical and does not depend on what you sent: the values `2025-04-29`, `2025-04-29T00:00:00+03:00`, and `2025-04-29T23:59:00+03:00` all produce the same response. Compare only the date against the value you sent. If the payment time matters, create a field of the "Date/Time" type — it retains the value in full.

{% endnote %}

## Verify the Result

Open the deal card in CRM. The "Payment Date" field holds `04/29/2025` — the same date as in the payment document.

Over REST, the field value is returned by the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method with the following parameters:

- `entityTypeId` — `2` for a deal

- `id` — the identifier of the deal, `6917` in the example

{% list tabs %}

- JS

    ```javascript
    const checkResult = await $b24.actions.v2.call.make({
        method: 'crm.item.get',
        params: { entityTypeId: 2, id: 6917 },
        requestId: 'item-check'
    });

    console.log(checkResult.getData().result.item[fieldName]);
    ```

- Python

    ```python
    print(client.crm.item.get(2, 6917).response.result["item"][field_name])
    ```


- PHP

    ```php
    echo $sb->getCRMScope()->item()->get(2, 6917)->item()->{$fieldName};
    ```
{% endlist %}

The scenario is complete if the `ufCrm_1746431727372` field in the response holds the payment date rather than `null` or an empty string.

```json
{
    "result": {
        "item": {
            "id": 6917,
            "title": "Deal #6531",
            "ufCrm_1746431727372": "2025-04-29T03:00:00+03:00"
        }
    }
}
```

## Errors and Diagnostics

If the method returned an error, check the request data.

#|
|| **Code** | **Cause and Action** ||
|| `NOT_FOUND` | The item is not found. Check the `id` of the deal: it could have been deleted or be in a pipeline the user has no access to ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | `entityTypeId` holds a value that matches no CRM object. A deal requires `2` ||
|| `ACCESS_DENIED` | The webhook user does not have permission to modify deals. Check which user the webhook was created on behalf of ||
|| `allowed_only_intranet_user` | The webhook is created on behalf of an external user. The scenario is available to Bitrix24 employees only ||
|#

The [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method rarely returns an error. An unknown field identifier, an invalid date value, and extra fields are discarded, and the method responds with success.

Check separately the cases where the response is successful but the result differs from the expected one.

- Step 1 did not find the field and `fieldName` is empty — there is no field with such a name in Bitrix24, or the field has a different type. Compare the name with the deal card: the selection matches `title` exactly, so an extra space or a different letter case breaks it

- Step 2 returned an empty array — the deal has no payments, or `entityTypeId` holds the wrong object type. With an incorrect `entityTypeId`, the method does not refuse but returns an empty result

- Step 3 completed, but the field remained empty — the identifier was passed in the legacy spelling `UF_CRM_1746431727372`. Pass the identifier from step 1 or add `useOriginalUfNames`: `Y` to the request

- The field holds the wrong date — in a field of the "Date" type the time is discarded, see the [Key Considerations](#date-type) section for details

Steps 1 and 2 change nothing and can be repeated any number of times. If step 3 returned an error, check the current field value as described in the "Verify the Result" section, correct the request, and repeat step 3 only.

## Key Considerations {#date-type}

- The field type decides whether the payment time is retained: "Date" keeps the date only, "Date/Time" keeps the value in full. Choose the type before you run the transfer across the entire database: once the fields are filled, the time cannot be recovered

- The `useOriginalUfNames`: `Y` parameter changes both the accepted and the returned identifiers: with it, the response comes with the `UF_CRM_1746431727372` key. Read the value by the identifier the parameter sets, not by the one you sent

- The method accepts the date value in two formats: `2025-04-29T13:03:20+03:00` and the short date format of your Bitrix24, for example `04/29/2025`

- A deal can have several payments, and the method returns all of them. For brevity, the step fragments take the first entry of the array, which is not necessarily the latest one and not necessarily a completed one

- The correct selection is shown in the [Code Example](#full-example) section: the entries with `paid`: `Y`, and the maximum `datePaid` among them. If you need the first payment or the total of all of them, change the selection condition

- The deal field is a copy of the date, not a link to the payment document. If the payment is cancelled or completed again, the value in the deal does not update by itself. Rerun the scenario on a schedule or every time you change the payments of the deal

- The same scenario works for other CRM object types that have payments: change `entityTypeId` in all three steps. The type identifiers are listed in the [CRM object type reference](../../../api-reference/crm/data-types.md#object_type)

## Code Example {#full-example}

The script finds a custom deal field by its name, reads the date of a completed payment, and writes it to that field. The field name and the deal `id` are declared as variables at the beginning of the script.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const ENTITY_TYPE_ID = 2; // 2 — deal
    const DEAL_ID = 6917; // specify your own deal
    const FIELD_TITLE = 'Payment Date'; // the field name in the deal card

    async function call(method, params, requestId) {
        const result = await $b24.actions.v2.call.make({ method, params, requestId });
        if (!result.isSuccess) {
            throw new Error(result.getErrorMessages().join('; '));
        }
        return result.getData().result;
    }

    async function setPaidDate() {
        try {
            // Step 1: find the field identifier by its name and type
            const { fields } = await call('crm.item.fields', {
                entityTypeId: ENTITY_TYPE_ID
            }, 'item-fields');

            const fieldName = Object.keys(fields).find(
                key => fields[key].title === FIELD_TITLE
                    && ['date', 'datetime'].includes(fields[key].type)
            );
            if (!fieldName) {
                console.error(`The "${FIELD_TITLE}" field of the "Date" type is not found in the deal card`);
                return;
            }
            console.log('Field identifier:', fieldName);

            // Step 2: read the date of the completed payment
            const payments = await call('crm.item.payment.list', {
                entityTypeId: ENTITY_TYPE_ID,
                entityId: DEAL_ID
            }, 'payment-list');

            const paid = payments.filter(payment => payment.paid === 'Y' && payment.datePaid);
            if (paid.length === 0) {
                console.error(`Deal ${DEAL_ID} has no completed payments`);
                return;
            }
            // take the latest payment rather than the first one in the array
            const datePaid = paid.map(payment => payment.datePaid).sort().pop();
            console.log('Payment date:', datePaid);

            // Step 3: write the date to the deal field
            const updated = await call('crm.item.update', {
                entityTypeId: ENTITY_TYPE_ID,
                id: DEAL_ID,
                fields: { [fieldName]: datePaid }
            }, 'item-update');

            // a field of the "Date" type discards the time, so check the value in the response
            console.log('Written to the deal:', updated.item[fieldName]);
        } catch (error) {
            console.error('The payment date is not written:', error.message);
        }
    }

    setPaidDate();
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
        )
    )

    ENTITY_TYPE_ID = 2  # 2 — deal
    DEAL_ID = 6917  # specify your own deal
    FIELD_TITLE = "Payment Date"  # the field name in the deal card

    try:
        # Step 1: find the field identifier by its name and type
        fields = client.crm.item.fields(
            ENTITY_TYPE_ID,
        ).response.result["fields"]

        field_name = next(
            (
                key
                for key, settings in fields.items()
                if settings["title"] == FIELD_TITLE and settings["type"] in ("date", "datetime")
            ),
            None,
        )

        if field_name is None:
            print(f'The "{FIELD_TITLE}" field of the "Date" type is not found in the deal card')
        else:
            print(f"Field identifier: {field_name}")

            # Step 2: read the date of the completed payment
            payments = client.crm.item.payment.list(
                entity_type_id=ENTITY_TYPE_ID,
                entity_id=DEAL_ID,
            ).response.result

            dates = [
                payment["datePaid"]
                for payment in payments
                if payment["paid"] == "Y" and payment["datePaid"]
            ]

            if not dates:
                print(f"Deal {DEAL_ID} has no completed payments")
            else:
                # take the latest payment rather than the first one in the array
                date_paid = max(dates)
                print(f"Payment date: {date_paid}")

                # Step 3: write the date to the deal field
                updated = client.crm.item.update(
                    ENTITY_TYPE_ID,
                    DEAL_ID,
                    {field_name: date_paid},
                ).response.result["item"]

                # a field of the "Date" type discards the time, so check the value in the response
                print(f"Written to the deal: {updated[field_name]}")
    except BitrixAPIError as error:
        print(f"The payment date is not written: {error}")
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $entityTypeId = 2; // 2 — deal
    $dealId = 6917; // specify your own deal
    $fieldTitle = 'Payment Date'; // the field name in the deal card

    try {
        // Step 1: find the field identifier by its name and type
        // crm.item.fields has no wrapper in the SDK — call the method directly
        $resultFields = $sb->core->call(
            'crm.item.fields',
            [ 'entityTypeId' => $entityTypeId ]
        );
        $fields = $resultFields->getResponseData()->getResult()['fields'];

        $fieldName = null;
        foreach ($fields as $key => $settings) {
            if ($settings['title'] === $fieldTitle && in_array($settings['type'], ['date', 'datetime'], true)) {
                $fieldName = $key;
                break;
            }
        }
        if ($fieldName === null) {
            echo 'The "' . $fieldTitle . '" field of the "Date" type is not found in the deal card';
            return;
        }
        echo 'Field identifier: ' . $fieldName . PHP_EOL;

        // Step 2: read the date of the completed payment
        // crm.item.payment.list has no wrapper in the SDK — call the method directly
        $resultPayments = $sb->core->call(
            'crm.item.payment.list',
            [
                'entityTypeId' => $entityTypeId,
                'entityId' => $dealId
            ]
        );
        $payments = $resultPayments->getResponseData()->getResult();

        $dates = [];
        foreach ($payments as $payment) {
            if ($payment['paid'] === 'Y' && !empty($payment['datePaid'])) {
                $dates[] = $payment['datePaid'];
            }
        }
        if ($dates === []) {
            echo 'Deal ' . $dealId . ' has no completed payments';
            return;
        }
        // take the latest payment rather than the first one in the array
        sort($dates);
        $datePaid = end($dates);
        echo 'Payment date: ' . $datePaid . PHP_EOL;

        // Step 3: write the date to the deal field
        $updated = $sb->getCRMScope()->item()->update(
            $entityTypeId,
            $dealId,
            [ $fieldName => $datePaid ]
        );

        // a field of the "Date" type discards the time, so check the value in the response
        echo 'Written to the deal: ' . $updated->item()->{$fieldName};
    } catch (\Throwable $e) {
        echo 'The payment date is not written: ' . $e->getMessage();
    }
    ```
{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/universal/crm-item-fields.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-update.md)
- [{#T}](../../../api-reference/crm/universal/payment/crm-item-payment-list.md)
- [{#T}](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md)
- [{#T}](../../../api-reference/crm/data-types.md)
- [{#T}](../../../api-reference/sale/payment/sale-payment-list.md)
