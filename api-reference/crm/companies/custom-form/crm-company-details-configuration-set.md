# Set Parameters for crm.company.details.configuration.set

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
>  - any user can set their own settings,
>  - a user with the "Allow changing settings" access permission in CRM can set others' and common settings

{% note warning "Method development has been halted" %}

The method `crm.company.details.configuration.set` continues to function, but there is a more relevant alternative [crm.item.details.configuration.set](../../universal/item-details-configuration/crm-item-details-configuration-set.md).

{% endnote %}

The method `crm.company.details.configuration.set` sets the settings for company cards: it records personal settings for the specified user or common settings for all users.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings.

Possible values:
- `P` — personal settings
- `C` — common settings

Default — `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier, which can be obtained using the [user.get](../../../user/user-get.md) method. Required only when setting personal settings.

If not specified — the `id` of the current user is used
||
|| **data***
[`section[]`](#section) | The list `section` describes the configuration of field sections in the company card.

The structure is described [below](#section) ||
|#

### section

Describes a specific section with fields within the company card.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Unique name of the section ||
|| **title***
[`string`](../../../data-types.md) | Title of the section.

Displayed in the item card ||
|| **type***
[`string`](../../../data-types.md) | Type of the section.

Currently, only the value `'section'` is available ||
|| **elements**
[`section_element[]`](#section_element) | The array `section_element` describes the configuration of fields in the section.

The structure is described [below](#section_element) ||
|#

#### section_element

Configuration of a specific field within the section.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Field identifier.

The list of available fields can be found using [`crm.company.fields`](../crm-company-fields.md) ||
|| **optionFlags**
[`integer`](../../../data-types.md) | Whether to always show the field:
- `1` — yes
- `0` — no

Default — `0` ||
|| **options**
[`object`](../../../data-types.md) | Additional [options list](#options) for the field ||
|#

#### section_element.options {#options}

#|
|| **Name**
`type` | **Fields where the option is available** | **Description** ||
|| **defaultAddressType**
[`integer`](../../../data-types.md) | `ADDRESS` | Identifier for the default address type. To find out possible address types, use [`crm.enum.addresstype`](../../auxiliary/enum/crm-enum-address-type.md) ||
|| **defaultCountry**
[`string`](../../../data-types.md) | 
`PHONE`
`CLIENT`
`COMPANY`
`CONTACT`
`MYCOMPANY_ID` | Country code for the default phone number format — a string of two Latin letters.

For example, `"DE"` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

For a user with `id = 1`, set the configuration for the company card:

- Section 1 — **About the Company**
    - Title
    - Logo
    - Company Type
    - Position
    - Phone
        - Default Country: **Germany(+1)**
    - E-mail
    - Contact
- Section 2 — **Additional**
    - Industry
    - Is the company available to everyone
    - Responsible**
    - Comment


{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"userId":1,"data":[{"name":"main","title":"About the Company","type":"section","elements":[{"name":"TITLE"},{"name":"LOGO"},{"name":"COMPANY_TYPE"},{"name":"POST"},{"name":"PHONE","options":{"defaultCountry":"DE"}},{"name":"EMAIL"},{"name":"CONTACT"}]},{"name":"additional","title":"Additional","type":"section","elements":[{"name":"INDUSTRY"},{"name":"OPENED"},{"name":"ASSIGNED_BY_ID"},{"name":"COMMENTS"}]}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.details.configuration.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"userId":1,"data":[{"name":"main","title":"About the Company","type":"section","elements":[{"name":"TITLE"},{"name":"LOGO"},{"name":"COMPANY_TYPE"},{"name":"POST"},{"name":"PHONE","options":{"defaultCountry":"DE"}},{"name":"EMAIL"},{"name":"CONTACT"}]},{"name":"additional","title":"Additional","type":"section","elements":[{"name":"INDUSTRY"},{"name":"OPENED"},{"name":"ASSIGNED_BY_ID"},{"name":"COMMENTS"}]}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.details.configuration.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.company.details.configuration.set",
    		{
    			userId: 1,
    			data:
    			[
    				{
    					name: "main",
    					title: "About the Company",
    					type: "section",
    					elements:
    					[
    						{ name: "TITLE" },
    						{ name: "LOGO" },
    						{ name: "COMPANY_TYPE" },
    						{ name: "POST" },
    					{
    						name: "PHONE",
    						options: {
    							defaultCountry: "DE",
    						},
    					},
    						{ name: "EMAIL" },
    						{ name: "CONTACT" }
    					]
    				},
    				{
    					name: "additional",
    					title: "Additional",
    					type: "section",
    					elements:
    					[
    						{ name: "INDUSTRY" },
    						{ name: "OPENED" },
    						{ name: "ASSIGNED_BY_ID" },
    						{ name: "COMMENTS" }
    					]
    				}
    			]
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
    }
    catch(error)
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
                'crm.company.details.configuration.set',
                [
                    'userId' => 1,
                    'data' => [
                        [
                            'name' => 'main',
                            'title' => 'About the Company',
                            'type' => 'section',
                            'elements' => [
                                ['name' => 'TITLE'],
                                ['name' => 'LOGO'],
                                ['name' => 'COMPANY_TYPE'],
                                ['name' => 'POST'],
                                [
                                    'name' => 'PHONE',
                                    'options' => [
                                        'defaultCountry' => 'DE',
                                    ],
                                ],
                                ['name' => 'EMAIL'],
                                ['name' => 'CONTACT']
                            ]
                        ],
                        [
                            'name' => 'additional',
                            'title' => 'Additional',
                            'type' => 'section',
                            'elements' => [
                                ['name' => 'INDUSTRY'],
                                ['name' => 'OPENED'],
                                ['name' => 'ASSIGNED_BY_ID'],
                                ['name' => 'COMMENTS']
                            ]
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting company details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.company.details.configuration.set",
        {
            userId: 1,
            data:
            [
                {
                    name: "main",
                    title: "About the Company",
                    type: "section",
                    elements:
                    [
                        { name: "TITLE" },
                        { name: "LOGO" },
                        { name: "COMPANY_TYPE" },
                        { name: "POST" },
                        {
                            name: "PHONE",
                            options: {
                                defaultCountry: "DE",
                            },
                        },
                        { name: "EMAIL" },
                        { name: "CONTACT" }
                    ]
                },
                {
                    name: "additional",
                    title: "Additional",
                    type: "section",
                    elements:
                    [
                        { name: "INDUSTRY" },
                        { name: "OPENED" },
                        { name: "ASSIGNED_BY_ID" },
                        { name: "COMMENTS" }
                    ]
                }
            ]
        },
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
        'crm.company.details.configuration.set',
        [
            'userId' => 1,
            'data' => [
                [
                    'name' => 'main',
                    'title' => 'About the Company',
                    'type' => 'section',
                    'elements' => [
                        ['name' => 'TITLE'],
                        ['name' => 'LOGO'],
                        ['name' => 'COMPANY_TYPE'],
                        ['name' => 'POST'],
                        [
                            'name' => 'PHONE',
                            'options' => [
                                'defaultCountry' => 'DE',
                            },
                        ],
                        ['name' => 'EMAIL'],
                        ['name' => 'CONTACT'],
                    ]
                ],
                [
                    'name' => 'additional',
                    'title' => 'Additional',
                    'type' => 'section',
                    'elements' => [
                        ['name' => 'INDUSTRY'],
                        ['name' => 'OPENED'],
                        ['name' => 'ASSIGNED_BY_ID'],
                        ['name' => 'COMMENTS'],
                    ]
                ],
            ]
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
        "start": 1769419970,
        "finish": 1769419970.57774,
        "duration": 0.577739953994751,
        "processing": 0,
        "date_start": "2026-01-26T12:32:50+01:00",
        "date_finish": "2026-01-26T12:32:50+01:00",
        "operating_reset_at": 1769420570,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Element at index 0 in section at index 1 does not have name."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | Access denied | The user does not have permission to change CRM settings ||
|| `-` | Parameter 'data' must be array | A non-array was passed in `data` ||
|| `-` | The data must be indexed array | A non-indexed array was passed in `data` ||
|| `-` | There are no data to write | An empty array was passed in `data` ||
|| `-` | Section at index `#` have type `data[i].type`. The expected type is 'section' | The value in `data[i].type` is different from `'section'` ||
|| `-` | Section at index `#` does not have name | An empty value was passed in `data[i].name` ||
|| `-` | Section at index `#` does not have title | An empty value was passed in `data[i].title` ||
|| `-` | Element at index `#` in section at index `#` does not have name | An empty value was passed in `data[i].elements[j].name` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-company-details-configuration-get.md)
- [{#T}](./crm-company-details-configuration-force-common-scope-for-all.md)
- [{#T}](./crm-company-details-configuration-reset.md)