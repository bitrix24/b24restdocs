# Get invoice fields and the products included in it crm.invoice.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

The method is deprecated. It is recommended to use [`Universal methods for invoices`](../../universal/invoice.md)

{% endnote %}

The method returns a description of the fields of the [invoice](./crm-invoice-add.md), including [custom fields](./crm-invoice-user-field-add.md).

No parameters.

## Code examples

{% include [Note about examples](../../../../_includes/examples.md) %}

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
    try
    {
    	const response = await $b24.callMethod(
    		'crm.invoice.fields',
    		{}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.invoice.fields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching invoice fields: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

### Returned data

{% include [Note about required parameters](../../../../_includes/required.md) %}

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
[`datetime`](../../../data-types.md) | Rejection date | Specified if the invoice is rejected ||
|| **DATE_PAY_BEFORE**
[`date`](../../../data-types.md) | Payment due date | ||
|| **DATE_PAYED**
[`datetime`](../../../data-types.md) | Date changed to paid status | Read-only ||
|| **DATE_STATUS**
[`datetime`](../../../data-types.md) | Date of status change | Read-only ||
|| **DATE_UPDATE**
[`datetime`](../../../data-types.md) | Date of modification | Read-only ||
|| **EMP_PAYED_ID**
[`integer`](../../../data-types.md) | Identifier of the user who last marked the invoice as "paid" | Read-only ||
|| **EMP_STATUS_ID**
[`integer`](../../../data-types.md) | Identifier of the user who last changed the invoice status | Read-only ||
|| **LID**
[`integer`](../../../data-types.md) | Site identifier | Read-only ||
|| **XML_ID**
[`string`](../../../data-types.md) | External code | ||
|| **ORDER_TOPIC***
[`string`](../../../data-types.md) | Subject |  ||
|| **PAY_SYSTEM_ID***
[`integer`](../../../data-types.md) | Identifier of the printed form |  ||
|| **PAY_VOUCHER_DATE**
[`date`](../../../data-types.md) | Payment date | Specified if the invoice is paid ||
|| **PAY_VOUCHER_NUM**
[`string`](../../../data-types.md) | Payment document number | Specified if the invoice is paid ||
|| **PAYED**
[`char`](../../../data-types.md) | Payment status | Read-only ||
|| **PERSON_TYPE_ID***
[`integer`](../../../data-types.md) | Payer type identifier |  ||
|| **PRICE**
[`double`](../../../data-types.md) | Amount | Read-only ||
|| **REASON_MARKED**
[`string`](../../../data-types.md) | Status comment | Specified if the invoice is paid or rejected ||
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
[`integer`](../../../data-types.md) | Company identifier | Specified if the payer is a "Legal entity" ||
|| **UF_CONTACT_ID**
[`integer`](../../../data-types.md) | Contact identifier | Specified if the payer is an "Individual", or as a contact person for the company ||
|| **UF_MYCOMPANY_ID**
[`integer`](../../../data-types.md) | Identifier of your company | Specified as the company with the invoice details (link to the details is set separately) ||
|| **UF_DEAL_ID**
[`integer`](../../../data-types.md) | Identifier of the related deal | ||
|| **USER_DESCRIPTION**
[`string`](../../../data-types.md) | Comment | ||
|| **PR_LOCATION**
[`integer`](../../../data-types.md) | Location identifier | Required if using tax mode on the document ||
|| **INVOICE_PROPERTIES**
[`array`](../../../data-types.md) | List of properties | If the client is a company, the following keys can be specified (all values are of type string): 
- **COMPANY** - Company name;
- **COMPANY_ADR** - Address;
- **CONTACT_PERSON** - Contact person;
- **EMAIL** - E-mail;
- **PHONE** - Phone;
- **INN** - Tax ID;
- **KPP** - Tax Registration Reason Code.

If the client is a contact: 
- **FIO** - Full name;
- **ADDRESS** - Address;
- **EMAIL** - E-mail; 
- **PHONE** - Phone. ||
|| **PRODUCT_ROWS**
[`array`](../../../data-types.md) | List of product items | Fields of the product item:
- **ID** - Identifier (integer), specify 0 for a new record;
- **PRICE** - Price (double);
- **DISCOUNT_PRICE** - Discount per product unit (double);
- **PRODUCT_ID** - Identifier of the product in the catalog (integer), 0 if not from the catalog;
- **PRODUCT_NAME** - Name of the product item (string);
- **VAT_RATE** - VAT rate coefficient (double);
- **VAT_INCLUDED** - VAT included in the price ('Y' or 'N') (char);
- **MEASURE_CODE** - Measurement unit code (integer);
- **MEASURE_NAME** - Measurement unit designation (string);
- **CATALOG_XML_ID** - External catalog code (string), read-only;
- **PRODUCT_XML_ID** - External product item code (string), matches the external product code if it is from the catalog. Read-only.
- **QUANTITY** - Quantity. ||
|#