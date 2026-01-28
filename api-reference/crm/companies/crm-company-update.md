# Update Existing Company crm.company.update

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Edit" access permission for companies

{% note warning "Method Development Stopped" %}

The method `crm.company.update` is still operational, but there is a more current equivalent, [crm.item.update](../universal/crm-item-update.md).

{% endnote %}

The method `crm.company.update` updates an existing company.

## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Company identifier ||
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
- `value_n` — new field value

The list of available fields is described in the method [crm.company.fields](crm-company-fields.md).

An incorrect field in `fields` will be ignored
||
|| **params**
[`object`](../../data-types.md) | Object containing a set of additional parameters:

- `REGISTER_SONET_EVENT` — send notification to the responsible person
- `REGISTER_HISTORY_EVENT` — register event in history. Possible values:
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
[`crm_status`](../data-types.md) | Company type. Values can be obtained using the method [crm.status.list](../status/crm-status-list.md) with a filter for `ENTITY_ID=COMPANY_TYPE` ||
|| **LOGO**
[`file`](../../data-types.md) | Logo ||
|| **INDUSTRY**
[`crm_status`](../data-types.md) | Industry. Values can be obtained using the method [crm.status.list](../status/crm-status-list.md) with a filter for `ENTITY_ID=INDUSTRY` ||
|| **EMPLOYEES**
[`crm_status`](../data-types.md) | Number of employees. Values can be obtained using the method [crm.status.list](../status/crm-status-list.md) with a filter for `ENTITY_ID=EMPLOYEES` ||
|| **CURRENCY_ID**
[`crm_currency`](../data-types.md) | Currency ||
|| **REVENUE**
[`double`](../../data-types.md) | Annual revenue ||
|| **OPENED**
[`char`](../../data-types.md) | Is the company available to everyone. Possible values:
- `Y` — yes
- `N` — no ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comment ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Responsible person ||
|| **CONTACT_ID**
[`crm_contact`](../../data-types.md) | Contact. Multiple ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Data source identifier. Used only for linking to an external source ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the element in the data source. Used only for linking to an external source ||
|| **ORIGIN_VERSION**
[`string`](../../data-types.md) | Original version. Used to protect data from accidental overwriting by an external system ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system. Google Ads, Bing Ads, etc. ||
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
||**PARENT_ID_...** | Relationship fields.

If there are SPAs related to companies in the account, for each such SPA there is a field that stores the relationship between this SPA and the company. The field itself stores the identifier of the element of that SPA ||
|| **PHONE**
[`crm_multifield[]`](../data-types.md) | Phone. Multiple ||
|| **EMAIL**
[`crm_multifield[]`](../data-types.md) | E-mail. Multiple ||
|| **WEB**
[`crm_multifield[]`](../data-types.md) | Website. Multiple ||
|| **IM**
[`crm_multifield[]`](../data-types.md) | Messenger. Multiple ||
|| **LINK**
[`crm_multifield[]`](../data-types.md) | LINK. Multiple ||
||**UF_...**  | Custom fields. For example, `UF_CRM_25534736`.

Depending on the account settings, companies may have a set of custom fields of specific types.

You can add a custom field to a company using the method [crm.company.userfield.add](./userfields/crm-company-userfield-add.md) ||
|#

{% note info " " %}

To change the address and banking details of the company, use the methods for [requisites](../requisites/index.md)

{% endnote %}

## Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":43,"FIELDS":{"CURRENCY_ID":"USD","REVENUE":500000,"EMPLOYEES":"EMPLOYEES_3"},"PARAMS":{"REGISTER_SONET_EVENT":"Y","REGISTER_HISTORY_EVENT":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":43,"FIELDS":{"CURRENCY_ID":"USD","REVENUE":500000,"EMPLOYEES":"EMPLOYEES_3"},"PARAMS":{"REGISTER_SONET_EVENT":"Y","REGISTER_HISTORY_EVENT":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.update
    ```

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.company.update",
    		{
    			id: id,
    			fields:
    			{
    				"CURRENCY_ID": "USD",
    				"REVENUE" : 500000,
    				"EMPLOYEES": "EMPLOYEES_3"
    			},
    			params: { "REGISTER_SONET_EVENT": "Y" }
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.info(result);
    	}
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    $id = readline("Enter ID");
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.update',
                [
                    'id' => $id,
                    'fields' => [
                        'CURRENCY_ID' => 'USD',
                        'REVENUE' => 500000,
                        'EMPLOYEES' => 'EMPLOYEES_3',
                    ],
                    'params' => ['REGISTER_SONET_EVENT' => 'Y'],
                ]
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
        echo 'Error updating company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.company.update",
        {
            id: id,
            fields:
            {
                "CURRENCY_ID": "USD",
                "REVENUE" : 500000,
                "EMPLOYEES": "EMPLOYEES_3"
            },
            params: { "REGISTER_SONET_EVENT": "Y" }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.company.update',
        [
            'id' => 43,
            'fields' => [
                'CURRENCY_ID' => 'USD',
                'REVENUE' => 500000,
                'EMPLOYEES' => 'EMPLOYEES_3',
            ],
            'params' => [
                'REGISTER_SONET_EVENT' => 'Y',
                'REGISTER_HISTORY_EVENT' => 'Y',
            ],
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
    "result": true,
    "time": {
        "start": 1769499930,
        "finish": 1769499931.074515,
        "duration": 1.0745151042938232,
        "processing": 1,
        "date_start": "2026-01-27T10:45:30+01:00",
        "date_finish": "2026-01-27T10:45:31+01:00",
        "operating_reset_at": 1769500530,
        "operating": 0.2604348659515381
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, returns `true` on success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Company is not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code**      | **Description** | **Value** ||
|| `-`          | Parameter 'fields' must be array | The parameter `fields` is not an object ||
|| `-`          | Parameter 'params' must be array | The parameter `params` is not an object ||
|| `-`          | Access denied | User does not have "Edit" access permission for companies ||
|| `-`          | Disk resource exhausted | ||
|| `ERROR_CORE` | Field `E-mail` contains an invalid address | Field `E-mail` contains an invalid address ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-add.md)
- [{#T}](./crm-company-get.md)
- [{#T}](./crm-company-list.md)
- [{#T}](./crm-company-delete.md)
- [{#T}](./crm-company-fields.md)
- [{#T}](../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)