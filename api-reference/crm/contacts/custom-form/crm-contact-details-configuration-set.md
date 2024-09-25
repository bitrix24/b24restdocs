# Set Parameters for the Individual Card crm.contact.details.configuration.set

> Scope: [`crm`](../../../scopes/permissions.md)
> 
> Who can execute the method:
>  - Any user has the right to access their own and shared settings
>  - Only an administrator has the right to access others' settings

This method sets the contact card settings: it writes personal settings for the specified user or shared settings for all users.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings. 

Possible values:
- **P** — personal settings
- **C** — shared settings

Default — `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier. Required only when setting personal settings.

If not specified, the `id` of the current user is used
||
|| **data***
[`section[]`](#section) | The list `section` describes the configuration of the field sections in the entity card.

The structure is described [below](#section) ||
|#

### section

Describes an individual section with fields within the contact card.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Unique name of the section ||
|| **title***
[`string`](../../../data-types.md) | Title of the section.

Displayed in the entity card ||
|| **type***
[`string`](../../../data-types.md) | Type of the section.

Currently, only the value `'section'` is available ||
|| **elements**
[`section_element[]`](#section_element) | The array `section_element` describes the configuration of the fields in the section.

The structure is described [below](#section_element) ||
|#

#### section_element

Configuration of an individual field within the section.

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Field identifier.

The list of available fields can be found using [`crm.contact.fields`](../crm-contact-fields.md) ||
|| **optionFlags**
[`integer`](../../../data-types.md) | Whether to always show the field:
- `1` — yes
- `0` — no

Default — `0` ||
|| **options**
[`object`](../../../data-types.md) | Additional [list of options](#options) for the field ||
|#

#### section_element.options {#options}

#|
|| **Name**
`type` | **Fields where the option is available** | **Description** ||
|| **defaultAddressType**
[`integer`](../../../data-types.md) | `ADDRESS` | Identifier for the default address type. To find possible address types, use [`crm.enum.addresstype`](../../auxiliary/enum/crm-enum-address-type.md) ||
|| **defaultCountry**
[`string`](../../../data-types.md) | 
`PHONE`
`CLIENT`
`COMPANY`
`CONTACT`
`MYCOMPANY_ID` | Country code for the default phone number format — a string of two Latin letters.

For example, `"DE"` ||
|| **isPayButtonVisible**
[`boolean`](../../../data-types.md) | `OPPORTUNITY_WITH_CURRENCY` | Whether the payment acceptance button is shown.

Possible values:
- `'true'` — shown
- `'false'` — hidden

Default — `true` ||
|| **isPaymentDocumentsVisible**
[`boolean`](../../../data-types.md) | `OPPORTUNITY_WITH_CURRENCY` | Whether the "Payment and Delivery" block is shown.

Possible values:
- `'true'` — shown
- `'false'` — hidden

Default — `true`
||
|#


## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

For the user with `id = 1`, set the following configuration for the contact card elements:

- Section 1 — **Personal Data**
    - **First Name**
        - Always show
    - **Last Name**
        - Always show
    - **Middle Name**
    - **Date of Birth**
    - **Phone**
        - Always show
        - Default country: **United Kingdom (+44)**
    - **Address**
        - Always show
        - Default address type: **Registration Address** (see [`crm.enum.addresstype`](../../auxiliary/enum/crm-enum-address-type.md))
- Section 2 — **Main Information**
    - **Contact Type**
    - **Source**
    - **Position**
- Section 3 — **Additional Information**
    - **Photo**
    - **Comment**
    - **Custom Field #1**


{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"userId":1,"data":[{"name":"section_1","title":"Personal Data","type":"section","elements":[{"name":"NAME","optionFlags":1},{"name":"LAST_NAME","optionFlags":1},{"name":"SECOND_NAME"},{"name":"BIRTHDATE"},{"name":"PHONE","optionFlags":1,"options":{"defaultCountry":"GB"}},{"name":"ADDRESS","optionFlags":1,"options":{"defaultAddressType":4}}]},{"name":"section_2","title":"Main Information","type":"section","elements":[{"name":"TYPE_ID"},{"name":"SOURCE_ID"},{"name":"POST"}]},{"name":"section_3","title":"Additional Information","type":"section","elements":[{"name":"PHOTO"},{"name":"COMMENTS"},{"name":"UF_CRM_1720697698689"}]}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.details.configuration.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"userId":1,"data":[{"name":"section_1","title":"Personal Data","type":"section","elements":[{"name":"NAME","optionFlags":1},{"name":"LAST_NAME","optionFlags":1},{"name":"SECOND_NAME"},{"name":"BIRTHDATE"},{"name":"PHONE","optionFlags":1,"options":{"defaultCountry":"GB"}},{"name":"ADDRESS","optionFlags":1,"options":{"defaultAddressType":4}}]},{"name":"section_2","title":"Main Information","type":"section","elements":[{"name":"TYPE_ID"},{"name":"SOURCE_ID"},{"name":"POST"}]},{"name":"section_3","title":"Additional Information","type":"section","elements":[{"name":"PHOTO"},{"name":"COMMENTS"},{"name":"UF_CRM_1720697698689"}]}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.details.configuration.set
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.contact.details.configuration.set',
        {
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
                    title: "Main Information",
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
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.details.configuration.set',
        [
            'userId' => 1,
            'data' => [
                [
                    'name' => 'section_1',
                    'title' => 'Personal Data',
                    'type' => 'section',
                    'elements' => [
                        [
                            'name' => 'NAME',
                            'optionFlags' => 1,
                        ],
                        [
                            'name' => 'LAST_NAME',
                            'optionFlags' => 1,
                        ],
                        [
                            'name' => 'SECOND_NAME',
                        ],
                        [
                            'name' => 'BIRTHDATE',
                        ],
                        [
                            'name' => 'PHONE',
                            'optionFlags' => 1,
                            'options' => [
                                'defaultCountry' => 'GB',
                            ],
                        ],
                        [
                            'name' => 'ADDRESS',
                            'optionFlags' => 1,
                            'options' => [
                                'defaultAddressType' => 4,
                            ],
                        ],
                    ],
                ],
                [
                    'name' => 'section_2',
                    'title' => 'Main Information',
                    'type' => 'section',
                    'elements' => [
                        ['name' => 'TYPE_ID'],
                        ['name' => 'SOURCE_ID'],
                        ['name' => 'POST'],
                    ],
                ],
                [
                    'name' => 'section_3',
                    'title' => 'Additional Information',
                    'type' => 'section',
                    'elements' => [
                        ['name' => 'PHOTO'],
                        ['name' => 'COMMENTS'],
                        ['name' => 'UF_CRM_1720697698689'],
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

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1724677217.639681,
        "finish": 1724677217.986853,
        "duration": 0.3471717834472656,
        "processing": 0.01840806007385254,
        "date_start": "2024-08-26T15:00:17+02:00",
        "date_finish": "2024-08-26T15:00:17+02:00",
        "operating": 0
    }
}
```

### Returned Values

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response.

Returns `true` on success ||
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
|| Empty value | Access denied. | The user does not have administrative rights ||
|| Empty value | Parameter 'data' must be array. | An array was not passed in `data` ||
|| Empty value | The data must be indexed array. | An unindexed array was passed in `data` ||
|| Empty value | There are no data to write. | An empty array was passed in `data` ||
|| Empty value | Section at index `i` has type `data[i].type`. The expected type is 'section'. | The value in `data[i].type` is different from `'section'` ||
|| Empty value | Section at index `i` does not have name. | An empty value was passed in `data[i].name` ||
|| Empty value | Section at index `i` does not have title. | An empty value was passed in `data[i].title` ||
|| Empty value | Element at index `j` in section at index `i` does not have name. | An empty value was passed in `data[i].elements[j].name` ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-contact-details-configuration-get.md)
- [{#T}](./crm-contact-details-configuration-force-common-scope-for-all.md)
- [{#T}](./crm-contact-details-configuration-reset.md)