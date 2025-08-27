# Get Company Fields Description crm.company.fields

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing response in case of success
- missing response in case of error
- links to pages not yet created are not specified
- some fields lack descriptions

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.fields` returns the description of [company fields](./crm-company-add.md), including [custom fields](./userfields/crm-company-userfield-add.md).

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.company.fields',
    		{}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
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
                'crm.company.fields',
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
        echo 'Error fetching company fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.fields",
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

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Fields

#|
|| **Field** | **Description** | **Note** ||
|| **ADDRESS**
[`string`](../../data-types.md) | Company address | ||
|| **ADDRESS_2**
[`string`](../../data-types.md) | Second line of the address | In some countries, it is customary to split the address into 2 parts ||
|| **ADDRESS_CITY**
[`string`](../../data-types.md) | City | ||
|| **ADDRESS_COUNTRY**
[`string`](../../data-types.md) | Country | ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../../data-types.md) | Country code | ||
|| **ADDRESS_LEGAL**
[`string`](../../data-types.md) | | ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../../data-types.md) | Postal code | ||
|| **ADDRESS_PROVINCE**
[`string`](../../data-types.md) | Province | ||
|| **ADDRESS_REGION**
[`string`](../../data-types.md) | Region | ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Linked to user by ID | ||
|| **BANKING_DETAILS**
[`string`](../../data-types.md) | Banking details | ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comments | ||
|| **COMPANY_TYPE**
[`crm_status`](../../data-types.md) | Company type | ||
|| **CREATED_BY_ID**
[`user`](../../data-types.md) | Created by | Read-only ||
|| **CURRENCY_ID**
[`crm_currency`](../../data-types.md) | Currency | ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Creation date | Read-only ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Modification date | Read-only ||
|| **EMAIL**
[`crm_multifield`](../../data-types.md) | Email address | Multiple ||
|| **EMPLOYEES**
[`crm_status`](../../data-types.md) | Number of employees | ||
|| **HAS_EMAIL**
[`char`](../../data-types.md) | Email field filled check | Read-only ||
|| **HAS_PHONE**
[`char`](../../data-types.md) | Phone field filled check | Read-only ||
|| **ID**
[`integer`](../../data-types.md) | Company ID | Read-only ||
|| **IM**
[`crm_multifield`](../../data-types.md) | Messengers | Multiple ||
|| **INDUSTRY**
[`crm_status`](../../data-types.md) | Industry | ||
|| **IS_MY_COMPANY**
[`char`](../../data-types.md) | | ||
|| **LEAD_ID**
[`crm_lead`](../../data-types.md) | Lead ID associated with the company | Read-only ||
|| **LOGO**
[`file`](../../data-types.md) | Logo | ||
|| **MODIFY_BY_ID**
[`user`](../../data-types.md) | ID of the last modification author | Read-only ||
|| **OPENED**
[`char`](../../data-types.md) | Available to everyone | ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Data source ID | Used only for linking to an external source. ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Element ID in the data source | Used only for linking to an external source. ||
|| **ORIGIN_VERSION**
[`string`](../../data-types.md) | Original version | Used to protect data from accidental overwriting by an external system. If the data was imported and not modified in the external system, such data can be edited in CRM without fear that the next export will overwrite the data ||
|| **PHONE**
[`crm_multifield`](../../data-types.md) | Company phone | Multiple ||
|| **REG_ADDRESS**
[`string`](../../data-types.md) | Legal address of the company | Deprecated, used for compatibility. ||
|| **REG_ADDRESS_2**
[`string`](../../data-types.md) | Second line of the legal address | In some countries, it is customary to split the address into 2 parts. Deprecated, used for compatibility. ||
|| **REG_ADDRESS_CITY**
[`string`](../../data-types.md) | City of the legal address | Deprecated, used for compatibility. ||
|| **REG_ADDRESS_COUNTRY**
[`string`](../../data-types.md) | Country of the legal address | Deprecated, used for compatibility. ||
|| **REG_ADDRESS_COUNTRY_CODE**
[`string`](../../data-types.md) | Country code of the legal address | Deprecated, used for compatibility. ||
|| **REG_ADDRESS_LEGAL**
[`string`](../../data-types.md) | | Deprecated, used for compatibility. ||
|| **REG_ADDRESS_POSTAL_CODE**
[`string`](../../data-types.md) | Postal code of the legal address | Deprecated, used for compatibility. ||
|| **REG_ADDRESS_PROVINCE**
[`string`](../../data-types.md) | Province of the legal address | Deprecated, used for compatibility. ||
|| **REG_ADDRESS_REGION**
[`string`](../../data-types.md) | Region of the legal address | Deprecated, used for compatibility. ||
|| **REVENUE**
[`double`](../../data-types.md) | Annual revenue | ||
|| **TITLE**
[`string`](../../data-types.md) | Title | Required ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Advertising campaign designation | ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Campaign content | For example, for contextual ads. ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Traffic type | CPC (ads), CPM (banners) ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system | Google-Adwords and others. ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Campaign search term | For example, keywords for contextual advertising. ||
|| **WEB**
[`crm_multifield`](../../data-types.md) | Company resource URLs | Multiple ||
|#