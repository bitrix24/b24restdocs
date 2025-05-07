# How to Enter the Payment Date in the Deal Field

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify the CRM entity

In Bitrix24, the payment date is stored in payment documents. Sometimes, the payment date may be needed in the deal field:

- for integrations with external systems,
- BI Builder reports,
- automations through Automation rules and workflows.
  
To transfer the payment date information to the deal, we will sequentially execute three methods:

1. [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) — we will get the identifier of the deal field where we will save the date information
2. [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) — we will retrieve payment information
3. [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) — we will save the payment date in the deal field

## 1. Getting the Field Identifier {#field_id}

To get the identifier of the deal field, we use the method [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) with the parameters:

- `filter[LANG]` — we use this language filter to display field names in the desired language. Without this filter, the names will not be displayed.
- `filter[USER_TYPE_ID]` — we use this field type filter to get only fields of type "Date" in the result.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.deal.userfield.list',
        {
            filter: {
                LANG: 'de', 
                USER_TYPE_ID: 'date'
            }
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.userfield.list',
        [
            'filter' => [
                'LANG' => 'de',
                'USER_TYPE_ID' => 'date'
            ]
        }
    );
    ```

{% endlist %}

As a result, we will receive information about all deal fields of type "Date". We will identify the appropriate field by its name in the `EDIT_FORM_LABEL` parameter. We will take the field identifier from the `FIELD_NAME` field.

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
            "EDIT_FORM_LABEL": "Payment Date",
            "LIST_COLUMN_LABEL": "Payment Date",
            "LIST_FILTER_LABEL": "Payment Date",
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
            "EDIT_FORM_LABEL": "End of Campaign",
            "LIST_COLUMN_LABEL": "End of Campaign",
            "LIST_FILTER_LABEL": "End of Campaign",
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
            "EDIT_FORM_LABEL": "Start of Campaign",
            "LIST_COLUMN_LABEL": "Start of Campaign",
            "LIST_FILTER_LABEL": "Start of Campaign",
            "ERROR_MESSAGE": null,
            "HELP_MESSAGE": null
        }
    ],
    "total": 3,
}
```

## 2. Getting the Payment Date {#date}

We use the method [crm.item.payment.list](../../../api-reference/crm/universal/payment/crm-item-payment-list.md) with the parameters:

- `entityId` — `ID` of the deal for which we are retrieving the payment date
- `entityTypeId` — [object type](../../../api-reference/crm/data-types.md#object_type), we will specify `2` for the deal

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.item.payment.list', {
            entityId: 6917,
            entityTypeId: 2,
        },
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.payment.list',
        [
            'entityId' => 6917,
            'entityTypeId' => 2
        ]
    );
    ```

{% endlist %}

As a result, we will receive a list of payments with fields for the deal. We will take the payment date from the `datePaid` field.

```json
{
    "result": [
        {
            "id": 503,
            "accountNumber": "831/1",
            "paid": "Y",
            "datePaid": "2025-04-29T13:03:20+02:00",
            "empPaidId": 1,
            "paySystemId": 19,
            "sum": 15,
            "currency": "USD",
            "paySystemName": "YooKassa"
        }
    ],
}
```

## 3. Entering the Date in the Deal Field
   
To modify the deal field and record the payment date in it, we use the method [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) with the parameters:

- `id` — `ID` of the deal, required parameter
- `fields[UF_CRM_1723209318]` — we will specify the value from the `datePaid` field obtained in [step 2](#date). We will pass the `FIELD_NAME` of the field obtained in [step 1](#field_id) as the field identifier.

{% list tabs %}

- JS
    
    ```javascript
    BX24.callMethod(
        'crm.deal.update',
        {
            id: 6917,
            fields: {
                UF_CRM_1723209318: "2025-04-29T13:03:20+02:00",
            },
        },
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.update',
        [
            'id' => 6917,
            'fields' => [
                'UF_CRM_1723209318' => '2025-04-29T13:03:20+02:00'
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive `true`, indicating that the deal has been successfully updated. If you receive an `error` in the result, refer to the documentation for the method [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md#handling-errors) for possible error descriptions.

```json
{
    "result": true,
}
```

## Checking the Value of the Deal Field

The received result does not contain information about the deal fields. To check if the payment date field has been successfully updated, we will execute the method [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md) with the parameters:

- `id` — `ID` of the deal, required parameter

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.deal.get',
        {
            id: 6917,
        },
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.get',
        [
            'id' => 6917
        ]
    );
    ```

{% endlist %}

As a result, we will receive the values of all deal fields, including custom fields. The value of the "Payment Date" field `UF_CRM_1723209318`: `2025-04-29T03:00:00+02:00` has been successfully set.

```json
{
    "result": {
        "ID": "6917",
        "TITLE": "Deal #6531",
        "TYPE_ID": "SALE",
        "STAGE_ID": "C9:NEW",
        "PROBABILITY": "0",
        "CURRENCY_ID": "USD",
        "OPPORTUNITY": "30.00",
        "IS_MANUAL_OPPORTUNITY": "N",
        "TAX_VALUE": "0.00",
        "LEAD_ID": null,
        "COMPANY_ID": "0",
        "CONTACT_ID": "275",
        "QUOTE_ID": null,
        "BEGINDATE": "2024-08-20T03:00:00+02:00",
        "CLOSEDATE": "2024-08-27T03:00:00+02:00",
        "ASSIGNED_BY_ID": "1",
        "CREATED_BY_ID": "1",
        "MODIFY_BY_ID": "1",
        "DATE_CREATE": "2025-04-29T00:03:19+02:00",
        "DATE_MODIFY": "2025-05-05T10:17:08+02:00",
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
        "MOVED_TIME": "2025-04-29T00:03:19+02:00",
        "LAST_ACTIVITY_TIME": "2025-04-29T13:03:21+02:00",
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
        "UF_CRM_1723209318": "2025-04-29T03:00:00+02:00",
        "UF_CRM_1722577765": "",
        "UF_CRM_1723188121": ""
    },
}
```

## Code Example

{% list tabs %}

- JS
  
    ```javascript
    // Step 1: Get FIELD_NAME for the field with EDIT_FORM_LABEL "Payment Date"
    BX24.callMethod(
        'crm.deal.userfield.list',
        {
            filter: {
                LANG: 'de',
                USER_TYPE_ID: 'date'
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                const fields = result.data();
                const dateField = fields.find(field => field.EDIT_FORM_LABEL === "Payment Date");
                if (dateField) {
                    const fieldName = dateField.FIELD_NAME;
                    console.log("FIELD_NAME for 'Payment Date':", fieldName);

                    // Step 2: Request deal ID from the user and get the payment date
                    const dealId = prompt("Enter the deal ID:");
                    BX24.callMethod(
                        'crm.item.payment.list',
                        {
                            entityId: dealId,
                            entityTypeId: 2
                        },
                        function(result) {
                            if (result.error()) {
                                console.error(result.error());
                            } else {
                                const payments = result.data();
                                if (payments.length > 0) {
                                    const datePaid = payments[0].datePaid;
                                    console.log("Payment Date:", datePaid);

                                    // Step 3: Update the deal
                                    BX24.callMethod(
                                        'crm.deal.update',
                                        {
                                            id: dealId,
                                            fields: {
                                                [fieldName]: datePaid
                                            }
                                        },
                                        function(result) {
                                            if (result.error()) {
                                                console.error(result.error());
                                            } else {
                                                console.log("Deal successfully updated");
                                            }
                                        }
                                    );
                                }
                            }
                        }
                    );
                }
            }
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    // Step 1: Get FIELD_NAME for the field with EDIT_FORM_LABEL "Payment Date"
    $result = CRest::call(
        'crm.deal.userfield.list',
        [
            'filter' => [
                'LANG' => 'de',
                'USER_TYPE_ID' => 'date'
            ]
        ]
    );

    if (!empty($result['error'])) {
        echo "Error: " . $result['error_description'];
    } else {
        $fields = $result['result'];
        $dateField = null;

        foreach ($fields as $field) {
            if ($field['EDIT_FORM_LABEL'] === "Payment Date") {
                $dateField = $field;
                break;
            }
        }

        if ($dateField) {
            $fieldName = $dateField['FIELD_NAME'];
            echo "FIELD_NAME for 'Payment Date': " . $fieldName . "\n";

            // Step 2: Request deal ID from the user and get the payment date
            $dealId = readline("Enter the deal ID: ");
            $paymentResult = CRest::call(
                'crm.item.payment.list',
                [
                    'entityId' => $dealId,
                    'entityTypeId' => 2
                ]
            );

            if (!empty($paymentResult['error'])) {
                echo "Error: " . $paymentResult['error_description'];
            } else {
                $payments = $paymentResult['result'];
                if (count($payments) > 0) {
                    $datePaid = $payments[0]['datePaid'];
                    echo "Payment Date: " . $datePaid . "\n";

                    // Step 3: Update the deal
                    $updateResult = CRest::call(
                        'crm.deal.update',
                        [
                            'id' => $dealId,
                            'fields' => [
                                $fieldName => $datePaid
                            ]
                        ]
                    );

                    if (!empty($updateResult['error'])) {
                        echo "Error: " . $updateResult['error_description'];
                    } else {
                        echo "Deal successfully updated\n";
                    }
                }
            }
        }
    }
    ```

{% endlist %}