# How to Save the Payment Date in the Deal Field

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify the CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In Bitrix24, the payment date is stored in payment documents. Sometimes, the payment date may be needed in the deal field:

- for integrations with external systems,
- for BI Builder reports,
- for automation via automation rules and workflows.
  
To transfer the payment date information to a deal, we will sequentially execute three methods:

1. [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) — retrieve the identifier of the deal field where the date information will be retained
2. [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) — retrieve payment information
3. [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) — retain the payment date in the deal field

## 1. Retrieve the Field Identifier {#field_id}

To retrieve the identifier of a deal field, use the [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) method with the following parameters:

- `filter[LANG]` — a language filter used to output field names in the required language. Without this filter, names will not be output.
- `filter[USER_TYPE_ID]` — a field type filter used to retrieve only fields of type "Date" in the result.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: 'crm.deal.userfield.list',
        params: {
            filter: {
                LANG: 'de',
                USER_TYPE_ID: 'date'
            }
        }
    });
    ```

- PHP
  
    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $logger = new Logger('b24');
    $logger->pushHandler(new StreamHandler('php://stdout'));

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $logger))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $serviceBuilder->getCRMScope()->dealUserfield()->list(
        [],
        [
            'LANG' => 'de',
            'USER_TYPE_ID' => 'date'
        ]
    );
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    result = client.crm.deal.userfield.list(
        filter={
            "LANG": "de",
            "USER_TYPE_ID": "date",
        }
    ).response.result
    ```

{% endlist %}

As a result, you will receive information about all deal fields of type "Date". Identify the appropriate field by the name in the `EDIT_FORM_LABEL` parameter. We will take the field identifier from the field `FIELD_NAME`.

```json
{
    "result": [
        {
            "ID": "6787",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1723209318",
            "USER_TYPE_ID": "date",
            "XML_ID": null,
            "SORT": "150",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "E",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "DEFAULT_VALUE": {
                    "TYPE": "NONE",
                    "VALUE": ""
                }
            },
            "EDIT_FORM_LABEL": "Payment date",
            "LIST_COLUMN_LABEL": "Payment date",
            "LIST_FILTER_LABEL": "Payment date",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "6795",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1723206732",
            "USER_TYPE_ID": "date",
            "XML_ID": null,
            "SORT": "150",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "E",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "DEFAULT_VALUE": {
                    "TYPE": "NONE",
                    "VALUE": ""
                }
            },
            "EDIT_FORM_LABEL": "End of campaign",
            "LIST_COLUMN_LABEL": "End of campaign",
            "LIST_FILTER_LABEL": "End of campaign",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        },
        {
            "ID": "6805",
            "ENTITY_ID": "CRM_DEAL",
            "FIELD_NAME": "UF_CRM_1723206709",
            "USER_TYPE_ID": "date",
            "XML_ID": null,
            "SORT": "150",
            "MULTIPLE": "N",
            "MANDATORY": "N",
            "SHOW_FILTER": "E",
            "SHOW_IN_LIST": "Y",
            "EDIT_IN_LIST": "Y",
            "IS_SEARCHABLE": "N",
            "SETTINGS": {
                "DEFAULT_VALUE": {
                    "TYPE": "NONE",
                    "VALUE": ""
                }
            },
            "EDIT_FORM_LABEL": "Start of campaign",
            "LIST_COLUMN_LABEL": "Start of campaign",
            "LIST_FILTER_LABEL": "Start of campaign",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        }
    ],
    "total": 3,
}
```

## 2. Retrieve the Payment Date {#date}

We use the method [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) with the following parameters:

- `entityId` — the `ID` of the deal for which we are retrieving the payment date
- `entityTypeId` — the [object type](../../../api-reference/crm/data-types.md#object_type), specify `2` for the deal

{% list tabs %}

- JS
  
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.item.payment.list',
        params: {
            entityId: 6917,
            entityTypeId: 2,
        }
    });
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->core->call(
        'crm.item.payment.list',
        [
            'entityId' => 6917,
            'entityTypeId' => 2
        ]
    );
    ```

- Python

    ```python
    result = client.crm.item.payment.list(
        entity_id=6917,
        entity_type_id=2,
    ).response.result
    ```

{% endlist %}

As a result, we will obtain a list of payments with fields for the deal. We will take the payment date from the `datePaid` field.

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
    ],
}
```

## 3. Retain the Date in a Deal Field
   
To update a deal field and write the payment date to it, use the [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) method with the following parameters:

- `id` — the `ID` of the deal, a required parameter
- `fields[UF_CRM_1723209318]` — specify the value from the `datePaid` field, obtained at [step 2](#date). As the field identifier, we will pass the `FIELD_NAME` of the field, obtained at [step 1](#field_id)

{% list tabs %}

- JS
    
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.deal.update',
        params: {
            id: 6917,
            fields: {
                UF_CRM_1723209318: "2025-04-29T13:03:20+03:00",
            },
        }
    });
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getCRMScope()->deal()->update(
        6917,
        [
            'UF_CRM_1723209318' => '2025-04-29T13:03:20+03:00'
        ]
    );
    ```

- Python

    ```python
    result = client.crm.deal.update(
        bitrix_id=6917,
        fields={
            "UF_CRM_1723209318": "2025-04-29T13:03:20+03:00",
        },
    ).response.result
    ```

{% endlist %}

As a result, you will receive `true`, the deal update was successful. If you received an error as a result `error`, study the description of possible errors in the [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md#error-handling) method documentation.

```json
{
    "result": true,
}
```

## Check the Value of the Deal Field

The received result does not contain information about deal fields. To verify whether the payment date field was updated successfully, execute the [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md) method with the following parameters:

- `id` — the `ID` of the deal, a required parameter

{% list tabs %}

- JS
  
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.deal.get',
        params: {
            id: 6917,
        }
    });
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getCRMScope()->deal()->get(6917);
    ```

- Python

    ```python
    result = client.crm.deal.get(
        bitrix_id=6917,
    ).response.result
    ```

{% endlist %}

As a result, you will receive the values of all deal fields, including user fields. The value of the "Payment Date" field `UF_CRM_1723209318`: `2025-04-29T03:00:00+03:00` was set successfully.

```json
{
    "result": {
        "ID": "6917",
        "TITLE": "Deal #6531",
        "TYPE_ID": "SALE",
        "STAGE_ID": "C9:NEW",
        "PROBABILITY": "0",
        "CURRENCY_ID": "EUR",
        "OPPORTUNITY": "30.00",
        "IS_MANUAL_OPPORTUNITY": "N",
        "TAX_VALUE": "0.00",
        "LEAD_ID": null,
        "COMPANY_ID": "0",
        "CONTACT_ID": "275",
        "QUOTE_ID": null,
        "BEGINDATE": "2024-08-20T03:00:00+03:00",
        "CLOSEDATE": "2024-08-27T03:00:00+03:00",
        "ASSIGNED_BY_ID": "1",
        "CREATED_BY_ID": "1",
        "MODIFY_BY_ID": "1",
        "DATE_CREATE": "2025-04-29T00:03:19+03:00",
        "DATE_MODIFY": "2025-05-05T10:17:08+03:00",
        "OPENED": "Y",
        "CLOSED": "N",
        "COMMENTS": "",
        "ADDITIONAL_INFO": null,
        "LOCATION_ID": null,
        "CATEGORY_ID": "9",
        "STAGE_SEMANTIC_ID": "P",
        "IS_NEW": "Y",
        "IS_RECURRING": "N",
        "IS_RETURN_CUSTOMER": "Y",
        "IS_REPEATED_APPROACH": "N",
        "SOURCE_ID": "",
        "SOURCE_DESCRIPTION": "",
        "ORIGINATOR_ID": null,
        "ORIGIN_ID": null,
        "MOVED_BY_ID": "0",
        "MOVED_TIME": "2025-04-29T00:03:19+03:00",
        "LAST_ACTIVITY_TIME": "2025-04-29T13:03:21+03:00",
        "UTM_SOURCE": null,
        "UTM_MEDIUM": null,
        "UTM_CAMPAIGN": null,
        "UTM_CONTENT": null,
        "UTM_TERM": null,
        "PARENT_ID_156": null,
        "PARENT_ID_177": null,
        "LAST_COMMUNICATION_TIME": null,
        "LAST_ACTIVITY_BY": "1",
        "UF_CRM_66976FE3B2425": [],
        "UF_CRM_1723206732": "",
        "UF_CRM_1723206709": "",
        "UF_CRM_1740471712": "",
        "UF_CRM_1723209318": "2025-04-29T03:00:00+03:00",
        "UF_CRM_1722577765": "",
        "UF_CRM_1723188121": ""
    },
}
```

## Code Example

{% list tabs %}

- JS
  
    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'
    import { createInterface } from 'node:readline/promises'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    async function call(method, params) {
        const result = await $b24.actions.v2.call.make({ method, params });
        if (!result.isSuccess) {
            throw new Error(result.getErrorMessages().join('; '));
        }
        return result.getData().result;
    }

    try {
        // Step 1: Get FIELD_NAME for the field with EDIT_FORM_LABEL "Payment date"
        const fields = await call('crm.deal.userfield.list', {
            filter: {
                LANG: 'de',
                USER_TYPE_ID: 'date'
            }
        });
        const dateField = fields.find(field => field.EDIT_FORM_LABEL === "Payment date");
        if (dateField) {
            const fieldName = dateField.FIELD_NAME;
            console.log("FIELD_NAME for 'Payment date':", fieldName);

            // Step 2: Request deal ID from user and get payment date
            const rl = createInterface({ input: process.stdin, output: process.stdout });
            const dealId = await rl.question("Enter deal ID: ");
            rl.close();

            const payments = await call('crm.item.payment.list', {
                entityId: dealId,
                entityTypeId: 2
            });
            if (payments.length > 0) {
                const datePaid = payments[0].datePaid;
                console.log("Payment date:", datePaid);

                // Step 3: Updating deal
                await call('crm.deal.update', {
                    id: dealId,
                    fields: {
                        [fieldName]: datePaid
                    }
                });
                console.log("Deal successfully updated");
            }
        }
    } catch (error) {
        console.error(error.message);
    }
    ```

- PHP
  
    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $logger = new Logger('b24');
    $logger->pushHandler(new StreamHandler('php://stdout'));

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $logger))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $crm = $serviceBuilder->getCRMScope();

    try {
        // Step 1: Get FIELD_NAME for the field with EDIT_FORM_LABEL "Payment date"
        $fields = $crm->dealUserfield()->list(
            [],
            [
                'LANG' => 'de',
                'USER_TYPE_ID' => 'date'
            ]
        )->getUserfields();

        $dateField = null;
        foreach ($fields as $field) {
            if ($field->EDIT_FORM_LABEL === "Payment date") {
                $dateField = $field;
                break;
            }
        }

        if ($dateField) {
            $fieldName = $dateField->FIELD_NAME;
            echo "FIELD_NAME for 'Payment date': " . $fieldName . "\n";

            // Step 2: Request deal ID from user and get payment date
            $dealId = readline("Enter deal ID: ");
            $payments = $serviceBuilder->core->call(
                'crm.item.payment.list',
                [
                    'entityId' => $dealId,
                    'entityTypeId' => 2
                ]
            )->getResponseData()->getResult();

            if (count($payments) > 0) {
                $datePaid = $payments[0]['datePaid'];
                echo "Payment date: " . $datePaid . "\n";

                // Step 3: Updating deal
                $crm->deal()->update(
                    (int)$dealId,
                    [
                        $fieldName => $datePaid
                    ]
                );
                echo "Deal successfully updated\n";
            }
        }
    } catch (\Throwable $e) {
        echo "Error: " . $e->getMessage();
    }
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
        user_fields = client.crm.deal.userfield.list(
            filter={
                "LANG": "de",
                "USER_TYPE_ID": "date",
            }
        ).response.result
    except BitrixAPIError as error:
        print(f"Error: {error}")
    else:
        date_field = next(
            (
                field
                for field in user_fields
                if field.get("EDIT_FORM_LABEL") == "Payment date"
            ),
            None,
        )

        if date_field:
            print("FIELD_NAME for 'Payment date':", date_field["FIELD_NAME"])

            deal_id = int(input("Enter deal ID: "))

            try:
                payments = client.crm.item.payment.list(
                    entity_type_id=2,
                    entity_id=deal_id,
                ).response.result
            except BitrixAPIError as error:
                print(f"Error: {error}")
            else:
                if payments:
                    date_paid = payments[0]["datePaid"]
                    print("Payment date:", date_paid)

                    try:
                        client.crm.deal.update(
                            bitrix_id=deal_id,
                            fields={
                                date_field["FIELD_NAME"]: date_paid,
                            },
                        ).response
                    except BitrixAPIError as error:
                        print(f"Error: {error}")
                    else:
                        print("Deal successfully updated")
    ```

{% endlist %}
