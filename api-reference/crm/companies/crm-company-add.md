# Create a New Company crm.company.add

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Add|Import" access permission for companies

{% note warning "Method Development Stopped" %}

The method `crm.company.add` continues to function, but there is a more relevant alternative [crm.item.add](../universal/crm-item-add.md).

{% endnote %}

The method `crm.company.add` creates a new company.

## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — field name
- `value_n` — field value

The list of available fields is described in the method [crm.company.fields](crm-company-fields.md).

An incorrect field in `fields` will be ignored.

{% note info " " %}

To find out the list of required fields, execute the method [crm.company.fields](./crm-company-fields.md).

{% endnote %}
 ||
|| **params**
[`object`](../../data-types.md) | An object containing a set of additional parameters:

- `REGISTER_SONET_EVENT` — register the event of adding a company and send a notification to the responsible person
- `IMPORT` — import mode. Possible values:
  - `Y` — yes
  - `N` — no
||
|#

### Parameter fields {#parameter-fields}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../data-types.md) | Company name ||
|| **COMPANY_TYPE**
[`crm_status`](../data-types.md) | Company type.

The list of available types can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "COMPANY_TYPE" }`.

Default — the first available company type ||
|| **INDUSTRY**
[`crm_status`](../data-types.md) | Industry.

The list of available values can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "INDUSTRY" }`.

Default — the first available industry ||
|| **EMPLOYEES**
[`crm_status`](../data-types.md) | Number of employees.

The list of available values can be obtained using the method [crm.status.list](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "EMPLOYEES" }`.

Default — the first available value ||
|| **CURRENCY_ID**
[`crm_currency`](../data-types.md) | Currency.

The list of available currencies can be obtained using the method [crm.currency.list](../currency/crm-currency-list.md) ||
|| **REVENUE**
[`double`](../../data-types.md) | Annual revenue ||
|| **LOGO**
[`file`](../../data-types.md) | Company logo ||
|| **OPENED**
[`char`](../../data-types.md) | Is the company available to everyone? Possible values:
- `Y` — yes
- `N` — no

Default `Y`. The default value can be changed in the CRM settings ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user responsible for the element.

Default — the identifier of the user calling the method ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comment ||
|| **PHONE**
[`crm_multifield[]`](../data-types.md) | Phone ||
|| **EMAIL**
[`crm_multifield[]`](../data-types.md) | E-mail ||
|| **WEB**
[`crm_multifield[]`](../data-types.md) | Website ||
|| **IM**
[`crm_multifield[]`](../data-types.md) | Messenger ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system, e.g., Google Ads ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Traffic type. Possible values:
- `CPC` — ads
- `CPM` — banners ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Advertising campaign designation ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Campaign content. For example, for contextual ads ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Campaign search condition. For example, keywords for contextual advertising ||
|| **IS_MY_COMPANY**
[`char`](../../data-types.md) | Is the company "my company"? Possible values:
- `Y` — yes
- `N` — no ||
||**UF_...**  | Custom fields. For example, `UF_CRM_25534736`.

Depending on the account settings, companies may have a set of custom fields of specific types.

You can add a custom field to a company using the method [crm.company.userfield.add](./userfields/crm-company-userfield-add.md) ||
||**PARENT_ID_...** | Relationship fields.

If there are SPAs related to companies on the account, there is a field for each such SPA that stores the relationship between that SPA and the company. The field itself stores the identifier of the element of that SPA ||
|#

**Fields for connections with external data sources**

If the company is created by an external system, then:
- the field `ORIGINATOR_ID` stores the string identifier of that system
- the field `ORIGIN_ID` stores the string identifier of the company in that external system
- the field `ORIGIN_VERSION` stores the version of the company's data in that external system

#|
|| **Name**
`type` | **Description** ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the external system that is the source of data about this company ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the company in the external system ||
|| **ORIGIN_VERSION**
[`string`](../../data-types.md) | Version of the data in the external system. Used to protect data from accidental overwriting ||
|#

**Import**

These fields are available for filling when the parameter `IMPORT = 'Y'` is passed in the `params` parameter.

#|
|| **Name**
`type` | **Description** ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Creation date ||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Modification date ||
|| **CREATED_BY_ID**
[`user`](../../data-types.md) | Created by ||
|| **MODIFY_BY_ID**
[`user`](../../data-types.md) | Modified by ||
|#

{% note info " " %}

To add the company's address and banking details, use the methods [requisites](../requisites/index.md).

{% endnote %}

## Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"LLC Smith","COMPANY_TYPE":"CUSTOMER","INDUSTRY":"MANUFACTURING","EMPLOYEES":"EMPLOYEES_2","CURRENCY_ID":"USD","REVENUE":3000000,"OPENED":"Y","ASSIGNED_BY_ID":1,"PHONE":[{"VALUE":"555888","VALUE_TYPE":"WORK"}]},"params":{"REGISTER_SONET_EVENT":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"LLC Smith","COMPANY_TYPE":"CUSTOMER","INDUSTRY":"MANUFACTURING","EMPLOYEES":"EMPLOYEES_2","CURRENCY_ID":"USD","REVENUE":3000000,"OPENED":"Y","ASSIGNED_BY_ID":1,"PHONE":[{"VALUE":"555888","VALUE_TYPE":"WORK"}]},"params":{"REGISTER_SONET_EVENT":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.company.add",
    		{
    			fields:
    			{
    				"TITLE": "LLC Smith",
    				"COMPANY_TYPE": "CUSTOMER",
    				"INDUSTRY": "MANUFACTURING",
    				"EMPLOYEES": "EMPLOYEES_2",
    				"CURRENCY_ID": "USD",
    				"REVENUE" : 3000000,
    				"LOGO": { "fileData": document.getElementById('logo') },
    				"OPENED": "Y",
    				"ASSIGNED_BY_ID": 1,
    				"PHONE": [ { "VALUE": "555888", "VALUE_TYPE": "WORK" } ]     
    			},
    			params: { "REGISTER_SONET_EVENT": "Y" }        
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info("Company created with ID " + result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.add',
                [
                    'fields' => [
                        'TITLE'         => 'LLC Smith',
                        'COMPANY_TYPE'  => 'CUSTOMER',
                        'INDUSTRY'      => 'MANUFACTURING',
                        'EMPLOYEES'     => 'EMPLOYEES_2',
                        'CURRENCY_ID'   => 'USD',
                        'REVENUE'       => 3000000,
                        'LOGO'          => ['fileData' => $_POST['logo']],
                        'OPENED'        => 'Y',
                        'ASSIGNED_BY_ID' => 1,
                        'PHONE'         => [['VALUE' => '555888', 'VALUE_TYPE' => 'WORK']],
                    ],
                    'params' => ['REGISTER_SONET_EVENT' => 'Y'],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Company created with ID ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.add",
        {
            fields:
            {
                "TITLE": "LLC Smith",
                "COMPANY_TYPE": "CUSTOMER",
                "INDUSTRY": "MANUFACTURING",
                "EMPLOYEES": "EMPLOYEES_2",
                "CURRENCY_ID": "USD",
                "REVENUE" : 3000000,
                "LOGO": { "fileData": document.getElementById('logo') },
                "OPENED": "Y",
                "ASSIGNED_BY_ID": 1,
                "PHONE": [ { "VALUE": "555888", "VALUE_TYPE": "WORK" } ]     
            },
            params: { "REGISTER_SONET_EVENT": "Y" }        
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Company created with ID " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.company.add',
        [
            'fields' => [
                'TITLE' => 'LLC Smith',
                'COMPANY_TYPE' => 'CUSTOMER',
                'INDUSTRY' => 'MANUFACTURING',
                'EMPLOYEES' => 'EMPLOYEES_2',
                'CURRENCY_ID' => 'USD',
                'REVENUE' => 3000000,
                'OPENED' => 'Y',
                'ASSIGNED_BY_ID' => 1,
                'PHONE' => [['VALUE' => '555888', 'VALUE_TYPE' => 'WORK']],
            ],
            'params' => ['REGISTER_SONET_EVENT' => 'Y'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 2921,
    "time": {
        "start": 1769500710,
        "finish": 1769500711.551784,
        "duration": 1.5517840385437012,
        "processing": 1,
        "date_start": "2026-01-27T10:58:30+02:00",
        "date_finish": "2026-01-27T10:58:31+02:00",
        "operating_reset_at": 1769501310,
        "operating": 0.6509370803833008
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Root element of the response, contains the identifier of the created company ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter 'fields' must be array."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | Parameter 'fields' must be array | The `fields` parameter is not an object ||
|| `-`     | Parameter 'params' must be array | The `params` parameter is not an object ||
|| `-`     | Access denied | The user does not have permission for "Add" or "Import" companies ||
|| `-`     | Disk resource exhausted | ||
|| `ERROR_CORE` | The `E-mail` field contains an invalid address | The `E-mail` field contains an invalid address ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-company.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-company-with-requisite.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-deal-with-choice-of-requisite.md)