# Set Parameters for Individual CRM Contact Detail Configuration

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
> 
> Who can execute the method: permission checks when executing the method depend on the provided data:
>   - Any user has the right to set their personal settings.
>   - A user can set general and others' settings only if they are an administrator.

The method `crm.contact.details.configuration.set` sets the settings for contact cards. The method records personal settings for the specified user or general settings for all users.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings.

Possible values:
- **P** - personal settings,
- **C** - general settings

Default - `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier. If not specified, the current user is taken. Required only when setting personal settings. ||
|| **data^*^**
[`section[]`](#section) | A list of `section` describing the configuration of field sections in the entity card. The structure of `section` is described [below](#section) ||
|#

### section

Describes a specific section with fields within the contact card.

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name^*^**
[`string`](../../../data-types.md) | Unique name of the section ||
|| **title^*^**
[`string`](../../../data-types.md) | Title of the section. Displayed in the entity card ||
|| **type^*^**
[`string`](../../../data-types.md) | Type of the section.

Currently, only the value `'section'` is available ||
|| **elements**
[`section_element[]`](#section_element) | Array of `section_element`, describing the configuration of fields in the section ||
|#

#### section_element

Configuration of a specific field within the section.

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name^*^**
[`string`](../../../data-types.md) | Field identifier. A list of available fields can be found using [`crm.contact.fields`](../crm-contact-fields.md) ||
|| **optionFlags**
[`integer`](../../../data-types.md) | Should the field always be displayed:
- `1` — yes
- `0` — no

Default - `0` ||
|| **options**
[`object`](../../../data-types.md) | Additional [options list](#options) for the field ||
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
`MYCOMPANY_ID` | Country code for the default phone number format — a string of two Latin letters. For example, `"US"` ||
|| **isPayButtonVisible**
[`boolean`](../../../data-types.md) | `OPPORTUNITY_WITH_CURRENCY` | Is the payment acceptance button visible?

Possible values:
- `'true'` — visible
- `'false'` — hidden

Default - `true` ||
|| **isPaymentDocumentsVisible**
[`boolean`](../../../data-types.md) | `OPPORTUNITY_WITH_CURRENCY` | Is the "Payment and Delivery" block visible?

Possible values:
- `'true'` — visible
- `'false'` — hidden

Default - `true`
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

For the user with `id = 1`, set the following configuration for the contact card

- Section 1 - **Personal Data**
    - **First Name**
        - Always show
    - **Last Name**
        - Always show
    - **Middle Name**
    - **Date of Birth**
    - **Phone**
        - Always show
        - Default country: **United Kingdom (+1)**
    - **Address**
        - Always show
        - Default address type: **Registration Address** (see [`crm.enum.addresstype`](../../auxiliary/enum/crm-enum-address-type.md))
- Section 2 - **Main Information**
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
    todo
    ```

- cURL (OAuth)

    ```bash
    todo
    ```

- JS

    ```javascript
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
    todo
    ```

{% endlist %}


## Response Handling

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Returns `true` on success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| Empty Value | Access denied. | The user does not have administrative rights ||
|| Empty Value | Parameter 'data' must be an array. | A non-array was passed in `data` ||
|| Empty Value | The data must be an indexed array. | A non-indexed array was passed in `data` ||
|| Empty Value | There are no data to write. | An empty array was passed in `data` ||
|| Empty Value | Section at index `i` has type `data[i].type`. The expected type is 'section'. | The value in `data[i].type` is different from `'section'` ||
|| Empty Value | Section at index `i` does not have a name. | An empty value was passed in `data[i].name` ||
|| Empty Value | Section at index `i` does not have a title. | An empty value was passed in `data[i].title` ||
|| Empty Value | Element at index `j` in section at index `i` does not have a name. | An empty value was passed in `data[i].elements[j].name` ||
|#

{% include [System Errors](./../../../../_includes/system-errors.md) %}


## Continue Learning

TODO