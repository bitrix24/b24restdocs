# Get Invoice Fields and Associated Products crm.invoice.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method returns a description of the fields of the [invoice](./crm-invoice-add.md), including [custom](./crm-invoice-user-field-add.md).

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.invoice.fields
   ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.invoice.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.invoice.fields",
        {},
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
        'crm.invoice.fields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

### Returned Data

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** | **Note** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier | Read-only ||
|| **ACCOUNT_NUMBER***
[`string`](../../../data-types.md) | Number |  ||
|| **COMMENTS**
[`text`](../../../data-types.md) | Manager's comment | ||
|| **CREATED_BY**
[`integer`](../../../data-types.md) | Created by user | Read-only ||
|| **CURRENCY**
[`crm_currency`](../../../data-types.md) | Currency identifier | Read-only ||
|| **DATE_BILL**
[`date`](../../../data-types.md) | Invoice date | ||
|| **DATE_INSERT**
[`datetime`](../../../data-types.md) | Creation date | ||
|| **DATE_MARKED**
[`datetime`](../../../data-types.md) | Rejection date | Provided if the invoice is rejected ||
|| **DATE_PAY_BEFORE**
[`date`](../../../data-types.md) | Payment due date | ||
|| **DATE_PAYED**
[`datetime`](../../../data-types.md) | Date changed to paid status | Read-only ||
|| **DATE_STATUS**
[`datetime`](../../../data-types.md) | Date status changed | Read-only ||
|| **DATE_UPDATE**
[`datetime`](../../../data-types.md) | Date updated | Read-only ||
|| **EMP_PAYED_ID**
[`integer`](../../../data-types.md) | Identifier of the user who last marked the invoice as paid | Read-only ||
|| **EMP_STATUS_ID**
[`integer`](../../../data-types.md) | Identifier of the user who last changed the invoice status | Read-only ||
|| **LID**
[`integer`](../../../data-types.md) | Site identifier | Read-only ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code | ||
|| **ORDER_TOPIC***
[`string`](../../../data-types.md) | Subject |  ||
|| **PAY_SYSTEM_ID***
[`integer`](../../../data-types.md) | Identifier of the payment form |  ||
|| **PAY_VOUCHER_DATE**
[`date`](../../../data-types.md) | Payment date | Provided if the invoice is paid ||
|| **PAY_VOUCHER_NUM**
[`string`](../../../data-types.md) | Payment document number | Provided if the invoice is paid ||
|| **PAYED**
[`char`](../../../data-types.md) | Payment status | Read-only ||
|| **PERSON_TYPE_ID***
[`integer`](../../../data-types.md) | Payer type identifier |  ||
|| **PRICE**
[`double`](../../../data-types.md) | Amount | Read-only ||
|| **REASON_MARKED**
[`string`](../../../data-types.md) | Status comment | Provided if the invoice is paid or rejected ||
|| **RESPONSIBLE_EMAIL**
[`string`](../../../data-types.md) | Responsible person's e-mail | Read-only ||
|| **RESPONSIBLE_ID**
[`integer`](../../../data-types.md) | Identifier of the responsible person | ||
|| **RESPONSIBLE_LAST_NAME**
[`string`](../../../data-types.md) | Last name of the responsible person | Read-only ||
|| **RESPONSIBLE_LOGIN**
[`string`](../../../data-types.md) | Login of the responsible person | Read-only ||
|| **RESPONSIBLE_NAME**
[`string`](../../../data-types.md) | First name of the responsible person | Read-only ||
|| **RESPONSIBLE_PERSONAL_PHOTO**
[`integer`](../../../data-types.md) | Identifier of the responsible person's photo | Read-only ||
|| **RESPONSIBLE_SECOND_NAME**
[`string`](../../../data-types.md) | Middle name of the responsible person | Read-only ||
|| **RESPONSIBLE_WORK_POSITION**
[`string`](../../../data-types.md) | Position of the responsible person | Read-only ||
|| **STATUS_ID**
[`crm_status`](../../../data-types.md) | Status identifier | Identifier of the "INVOICE_STATUS" reference ||
|| **TAX_VALUE**
[`double`](../../../data-types.md) | Tax amount | Read-only ||
|| **UF_COMPANY_ID**
[`integer`](../../../data-types.md) | Company identifier | Provided if the payer is a "Legal entity" ||
|| **UF_CONTACT_ID**
[`integer`](../../../data-types.md) | Contact identifier | Provided if the payer is an "Individual" or as a contact person for the company ||
|| **UF_MYCOMPANY_ID**
[`integer`](../../../data-types.md) | Identifier of your company | Provided as the company with invoice details (link to details is set separately) ||
|| **UF_DEAL_ID**
[`integer`](../../../data-types.md) | Identifier of the associated deal | ||
|| **USER_DESCRIPTION**
[`string`](../../../data-types.md) | Comment | ||
|| **PR_LOCATION**
[`integer`](../../../data-types.md) | Location identifier | Required if using document tax mode ||
|| **INVOICE_PROPERTIES**
[`array`](../../../data-types.md) | List of properties | If the client is a company, the following keys may be specified (all values are of type string): 
- **COMPANY** - Company name;
- **COMPANY_ADR** - Address;
- **CONTACT_PERSON** - Contact person;
- **EMAIL** - E-mail;
- **PHONE** - Phone;
- **INN** - Tax ID;
- **KPP** - Tax registration reason code.

If the client is a contact: 
- **FIO** - Full name;
- **ADDRESS** - Address;
- **EMAIL** - E-mail; 
- **PHONE** - Phone. ||
|| **PRODUCT_ROWS**
[`array`](../../../data-types.md) | List of product items | Fields of the product item:
- **ID** - Identifier (integer), use 0 for a new record;
- **PRICE** - Price (double);
- **DISCOUNT_PRICE** - Discount per product unit (double);
- **PRODUCT_ID** - Identifier of the product in the catalog (integer), 0 if not from the catalog;
- **PRODUCT_NAME** - Name of the product item (string);
- **VAT_RATE** - VAT rate coefficient (double);
- **VAT_INCLUDED** - VAT included in the price ('Y' or 'N') (char);
- **MEASURE_CODE** - Unit of measure code (integer);
- **MEASURE_NAME** - Unit of measure designation (string);
- **CATALOG_XML_ID** - External catalog code (string), read-only;
- **PRODUCT_XML_ID** - External product item code (string), matches the external product code if it is from the catalog. Read-only.
- **QUANTITY** - Quantity. ||
|#