# Set Parameters for CRM Item Card crm.item.details.configuration.set

> Method name: **crm.item.details.configuration.set**
>
> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: access permission checks depend on the provided data:
>   - Any user can set their personal settings
>   - A user can set common and others' settings only if they are an administrator

The method sets the settings for the card of a specific CRM object. It records personal settings for the specified user or common settings for all users.

{% include [Extras Notice](./_includes/extras_notice.md) %}

## Method Parameters

{% include [Required Parameters Notice](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](./../../index.md) or [user-defined type](./../user-defined-object-types/index.md) of CRM objects ||
|| **data***
[`section[]`](#section)| List of `section` describing the configuration of field sections in the item card. The structure of `section` is described below ||
|| **userId**
[`user`][1] | Identifier of the user for whom you want to set the configuration.

If the parameter is not provided, the `userId` of the user calling this method will be used.

Required only when requesting personal settings
||
|| **scope**
[`string`][1] | Scope of the settings. Allowed values:
- `'P'` — personal settings
- `'C'` — common settings

By default, the value is `'P'`

||
|| **extras**
[`object`][1] | Additional parameters. Possible values and their structure are described [below](#extras) ||
|#

### section

Describes a specific section with fields within the item card.

{% include [Required Parameters Notice](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`][1] | Unique name of the section ||
|| **title***
[`string`][1] | Title of the section. Displayed in the item card ||
|| **type***
[`string`][1] | Type of the section. 

Currently, only the value `'section'` is available ||
|| **elements**
[`section_element[]`](#section_element) | Array of `section_element`, describing the configuration of fields in the section ||
|#

#### section_element

Configuration of a specific field within the section.

{% include [Required Parameters Notice](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`][1] | Field identifier. The list of available fields can be found using [`crm.item.fields`](../crm-item-fields.md) ||
|| **optionFlags**
[`integer`][1] | Should the field always be shown:
- `1` — yes
- `0` — no

By default, the value is `0` ||
|| **options**
[`object`][1] | Additional [options list](#section_elementoptions) for the field ||
|#

#### section_element.options

#|
|| **Name**
`type` | **Fields where the option is available** | **Description** | **Default** ||
|| **defaultAddressType**
[`integer`][1] | `ADDRESS` | Identifier of the default address type. To find possible address types, use [`crm.enum.addresstype`][2] | `Computed` ||
|| **defaultCountry** 
[`string`][1] | `PHONE`
`CLIENT`
`COMPANY`
`CONTACT`
`MYCOMPANY_ID` | Country code for the default phone number format — a string of two Latin letters. For example, `"GB"`              | `Computed` ||
|| **isPayButtonVisible**
[`boolean`][1] | `OPPORTUNITY_WITH_CURRENCY` | Is the payment acceptance button shown.

Possible values:
- `'true'` — shown
- `'false'` — hidden 
| `'true'` ||
|| **isPaymentDocumentsVisible**
[`boolean`][1] | `OPPORTUNITY_WITH_CURRENCY` | Is the "Payment and Delivery" block shown.

Possible values: 
- `'true'` — shown
- `'false'` — hidden 
| `'true'` ||
|#

### extras

The `extras` parameter depends on the CRM object.

#|
|| **CRM Object** | **Name** | **Description** ||
|| **SPA** | `categoryId` | Identifier of the SPA funnel. Can be obtained using [`crm.category.list`](./../category/crm-category-list.md).

If not specified, the default funnel identifier for this SPA will be used ||
|| **Deal** | `dealCategoryId` | Identifier of the deal funnel. Can be obtained using [`crm.category.list`](./../category/crm-category-list.md).

If not specified, the default funnel identifier for deals will be used ||
|| **Lead** | `leadCustomerType` | Type of leads. 

Possible values:
- `1` — simple leads
- `2` — repeat leads
||
|#

## Code Examples

{% include [Examples Notice](../../../../_includes/examples.md) %}

For the user with `id = 1`, set the following configuration for the item card

- Section 1 - **Personal Data**
    - **First Name**
        - Show always
    - **Last Name**
        - Show always
    - **Middle Name**
    - **Date of Birth**
    - **Phone**
        - Show always
        - Default country: **United Kingdom(+44)**
    - **Address**
        - Show always
        - Default address type: **Registration Address** (see [`crm.enum.addresstype`][2])
- Section 2 - **Basic Information**
    - **Contact Type**
    - **Source**
    - **Position**
- Section 3 - **Additional Information**
    - **Photo**
    - **Comment**
    - **Custom Field #1**

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":3,"userId":1,"data":[{"name":"section_1","title":"Personal Data","type":"section","elements":[{"name":"NAME","optionFlags":1},{"name":"LAST_NAME","optionFlags":1},{"name":"SECOND_NAME"},{"name":"BIRTHDATE"},{"name":"PHONE","optionFlags":1,"options":{"defaultCountry":"GB"}},{"name":"ADDRESS","optionFlags":1,"options":{"defaultAddressType":4}}]},{"name":"section_2","title":"Basic Information","type":"section","elements":[{"name":"TYPE_ID"},{"name":"SOURCE_ID"},{"name":"POST"}]},{"name":"section_3","title":"Additional Information","type":"section","elements":[{"name":"PHOTO"},{"name":"COMMENTS"},{"name":"UF_CRM_1720697698689"}]}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.details.configuration.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":3,"userId":1,"data":[{"name":"section_1","title":"Personal Data","type":"section","elements":[{"name":"NAME","optionFlags":1},{"name":"LAST_NAME","optionFlags":1},{"name":"SECOND_NAME"},{"name":"BIRTHDATE"},{"name":"PHONE","optionFlags":1,"options":{"defaultCountry":"GB"}},{"name":"ADDRESS","optionFlags":1,"options":{"defaultAddressType":4}}]},{"name":"section_2","title":"Basic Information","type":"section","elements":[{"name":"TYPE_ID"},{"name":"SOURCE_ID"},{"name":"POST"}]},{"name":"section_3","title":"Additional Information","type":"section","elements":[{"name":"PHOTO"},{"name":"COMMENTS"},{"name":"UF_CRM_1720697698689"}]}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.details.configuration.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.item.details.configuration.set',
    		{
    			entityTypeId: 3,
    			userId: 1,
    			data: [
    				{
    					name: "section_1",
    					title: "Personal Data",
    					type: "section",
    					elements: [
    						{
    							name: "NAME",
    							optionFlags: 1,
    						},
    						{
    							name: "LAST_NAME",
    							optionFlags: 1,
    						},
    						{
    							name: "SECOND_NAME",
    						},
    						{
    							name: "BIRTHDATE",
    						},
    						{
    							name: "PHONE",
    							optionFlags: 1,
    							options: {
    								defaultCountry: "GB",
    							},
    						},
    						{
    							name: "ADDRESS",
    							optionFlags: 1,
    							options: {
    								defaultAddressType: 4,
    							},
    						},
    					],
    				},
    				{
    					name: "section_2",
    					title: "Basic Information",
    					type: "section",
    					elements: [
    						{ name: "TYPE_ID" },
    						{ name: "SOURCE_ID" },
    						{ name: "POST" },
    					],
    				},
    				{
    					name: "section_3",
    					title: "Additional Information",
    					type: "section",
    					elements: [
    						{ name: "PHOTO" },
    						{ name: "COMMENTS" },
    						{ name: "UF_CRM_1720697698689" },
    					],
    				},
    			],
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'crm.item.details.configuration.set',
                [
                    'entityTypeId' => 3,
                    'userId'       => 1,
                    'data'         => [
                        [
                            'name'     => "section_1",
                            'title'    => "Personal Data",
                            'type'     => "section",
                            'elements' => [
                                [
                                    'name'        => "NAME",
                                    'optionFlags' => 1,
                                ],
                                [
                                    'name'        => "LAST_NAME",
                                    'optionFlags' => 1,
                                ],
                                [
                                    'name' => "SECOND_NAME",
                                ],
                                [
                                    'name' => "BIRTHDATE",
                                ],
                                [
                                    'name'     => "PHONE",
                                    'optionFlags' => 1,
                                    'options'  => [
                                        'defaultCountry' => "GB",
                                    ],
                                ],
                                [
                                    'name'     => "ADDRESS",
                                    'optionFlags' => 1,
                                    'options'  => [
                                        'defaultAddressType' => 4,
                                    ],
                                ],
                            ],
                        ],
                        [
                            'name'     => "section_2",
                            'title'    => "Basic Information",
                            'type'     => "section",
                            'elements' => [
                                ['name' => "TYPE_ID"],
                                ['name' => "SOURCE_ID"],
                                ['name' => "POST"],
                            ],
                        ],
                        [
                            'name'     => "section_3",
                            'title'    => "Additional Information",
                            'type'     => "section",
                            'elements' => [
                                ['name' => "PHOTO"],
                                ['name' => "COMMENTS"],
                                ['name' => "UF_CRM_1720697698689"],
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
        BX24.callMethod(
            'crm.item.details.configuration.set',
            {
                entityTypeId: 3,
                userId: 1,
                data: [
                    {
                        name: "section_1",
                        title: "Personal Data",
                        type: "section",
                        elements: [
                            {
                                name: "NAME",
                                optionFlags: 1,
                            },
                            {
                                name: "LAST_NAME",
                                optionFlags: 1,
                            },
                            {
                                name: "SECOND_NAME",
                            },
                            {
                                name: "BIRTHDATE",
                            },
                            {
                                name: "PHONE",
                                optionFlags: 1,
                                options: {
                                    defaultCountry: "GB",
                                },
                            },
                            {
                                name: "ADDRESS",
                                optionFlags: 1,
                                options: {
                                    defaultAddressType: 4,
                                },
                            },
                        ],
                    },
                    {
                        name: "section_2",
                        title: "Basic Information",
                        type: "section",
                        elements: [
                            { name: "TYPE_ID" },
                            { name: "SOURCE_ID" },
                            { name: "POST" },
                        ],
                    },
                    {
                        name: "section_3",
                        title: "Additional Information",
                        type: "section",
                        elements: [
                            { name: "PHOTO" },
                            { name: "COMMENTS" },
                            { name: "UF_CRM_1720697698689" },
                        ],
                    },
                ],
            },
            (result) => {
                if (result.error())
                {
                    console.error(result.error());

                    return;
                }

                console.info(result.data());
            },
        );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.details.configuration.set',
        [
            'entityTypeId' => 3,
            'userId' => 1,
            'data' => [
                [
                    'name' => "section_1",
                    'title' => "Personal Data",
                    'type' => "section",
                    'elements' => [
                        [
                            'name' => "NAME",
                            'optionFlags' => 1,
                        ],
                        [
                            'name' => "LAST_NAME",
                            'optionFlags' => 1,
                        ],
                        [
                            'name' => "SECOND_NAME",
                        ],
                        [
                            'name' => "BIRTHDATE",
                        ],
                        [
                            'name' => "PHONE",
                            'optionFlags' => 1,
                            'options' => [
                                'defaultCountry' => "GB",
                            ],
                        ],
                        [
                            'name' => "ADDRESS",
                            'optionFlags' => 1,
                            'options' => [
                                'defaultAddressType' => 4,
                            ],
                        ],
                    ],
                ],
                [
                    'name' => "section_2",
                    'title' => "Basic Information",
                    'type' => "section",
                    'elements' => [
                        ['name' => "TYPE_ID"],
                        ['name' => "SOURCE_ID"],
                        ['name' => "POST"],
                    ],
                ],
                [
                    'name' => "section_3",
                    'title' => "Additional Information",
                    'type' => "section",
                    'elements' => [
                        ['name' => "PHOTO"],
                        ['name' => "COMMENTS"],
                        ['name' => "UF_CRM_1720697698689"],
                    ],
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1720699969.76157,
        "finish": 1720699970.153406,
        "duration": 0.39183592796325684,
        "processing": 0.02178215980529785,
        "date_start": "2024-07-11T14:12:49+02:00",
        "date_finish": "2024-07-11T14:12:50+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response. Returns `true` on success ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Element at index 0 in section at index 1 does not have name."
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty value | Parameter 'entityTypeId' is not defined | Required parameter `entityTypeId` not provided ||
|| Empty value | The entity type '`entityTypeName`' is not supported in the current context. | The method does not support this entity type ||
|| Empty value | Access denied. | The user does not have administrative rights ||
|| Empty value | Parameter 'data' must be an array. | A non-array was provided in `data` ||
|| Empty value | The data must be an indexed array. | A non-indexed array was provided in `data` ||
|| Empty value | There are no data to write. | An empty array was provided in `data` ||
|| Empty value | Section at index `i` has type `data[i].type`. The expected type is 'section'. | The value in `data[i].type` is different from `'section'` || 
|| Empty value | Section at index `i` does not have a name. | An empty value was provided in `data[i].name` ||
|| Empty value | Section at index `i` does not have a title. | An empty value was provided in `data[i].title` ||
|| Empty value | Element at index `j` in section at index `i` does not have a name. | An empty value was provided in `data[i].elements[j].name` ||
|#

{% include [System Errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-item-details-configuration-get.md)
- [{#T}](./crm-item-details-configuration-reset.md)
- [{#T}](./crm-item-details-configuration-forceCommonScopeForAll.md)

[1]: ../../../data-types.md
[2]: ../../auxiliary/enum/crm-enum-address-type.md