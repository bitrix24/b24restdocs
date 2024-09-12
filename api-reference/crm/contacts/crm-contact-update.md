# Update Contact crm.contact.update

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "edit" access permission for contacts

The method `crm.contact.update` updates an existing contact.

{% note warning %}

It is highly recommended to pass the complete set of address fields when updating an address. The specifics of updating address fields are described [here](../data-types.md).

{% endnote %}

## Method Parameters

{% include [Parameter Note](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`][1] | The identifier of the contact we want to change.

Can be obtained using the methods [`crm.contact.list`](crm-contact-list.md) or [`crm.contact.add`](crm-contact-add.md) ||
|| **fields^*^**
[`object`][1] | An object in the format
```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where
- `field_n` — field name
- `value_n` — new field value

The list of available fields is described [below](#parametr-fields).

An incorrect field in `fields` will be ignored.

{% note info %}

Only the fields that need to be changed should be passed in `fields`.

{% endnote %} ||
|| **params**
[`object`][1] | An object containing a set of additional parameters.

The structure and possible values are [below](#parametr-params)
|#

### Parameter fields

#|
|| **Parameter**
[`type`][1] | **Description** ||
|| **HONORIFIC**
[`crm_status`](../data-types.md) | Salutation
The list of available salutation types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "HONORIFIC" }` ||
|| **NAME**
[`string`][1] | First name ||
|| **SECOND_NAME**
[`string`][1] | Middle name ||
|| **LAST_NAME**
[`string`][1] | Last name ||
|| **PHOTO**
[`file`][1] | Photo ||
|| **BIRTHDATE**
[`date`][1] | Birthdate ||
|| **TYPE_ID**
[`crm_status`](../data-types.md) | Contact type
The list of available contact types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "CONTACT_TYPE" }` ||
|| **SOURCE_ID**
[`crm_status`](../data-types.md) | Source
The list of available source types can be obtained using [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }` ||
|| **SOURCE_DESCRIPTION**
[`string`][1] | Additional information about the source ||
|| **POST**
[`string`][1] | Position ||
|| **COMMENTS**
[`string`][1] | Comment. Supports BB codes ||
|| **OPENED**
[`boolean`][1] | Available to everyone

Possible values:

- `Y` — yes
- `N` — no ||
|| **EXPORT**
[`boolean`][1] | Is the contact involved in export

Possible values:

- `Y` — yes
- `N` — no ||
|| **ASSIGNED_BY_ID**
[`user`][1] | Identifier of the user responsible for the entity ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the main company for the contact
The list of companies can be obtained using the method [`crm.item.list`](../universal/crm-item-list.md) with `entityTypeId = 4`. ||
|| **COMPANY_IDS**
[`crm_company[]`](../data-types.md) | Array of identifiers of companies associated with the contact
The list of companies can be obtained using the method [`crm.item.list`](../universal/crm-item-list.md) with `entityTypeId = 4`. ||
|| **UTM_SOURCE**
[`string`][1] | Advertising system. Yandex-Direct, Google-Adwords, and others ||
|| **UTM_MEDIUM**
[`string`][1] | Traffic type. Possible values:

- CPC — ads
- CPM — banners ||
|| **UTM_CAMPAIGN**
[`string`][1] | Advertising campaign designation ||
|| **UTM_CONTENT**
[`string`][1] | Content of the campaign. For example, for contextual ads ||
|| **UTM_TERM**
[`string`][1] | Campaign search condition. For example, keywords for contextual advertising ||
|| **PHONE**
[`crm_multifield[]`](../data-types.md) | Phone ||
|| **EMAIL**
[`crm_multifield[]`](../data-types.md) | E-mail ||
|| **WEB**
[`crm_multifield[]`](../data-types.md) | Website ||
|| **IM**
[`crm_multifield[]`](../data-types.md) | Messenger ||
|| **LINK**
[`crm_multifield[]`](../data-types.md) | Links. Service field. ||
||**UF_...**  | User-defined fields. For example, `UF_CRM_25534736`. Depending on the account settings, contacts may have a set of user-defined fields of specific types. A user-defined field can be added to a contact using the method [crm.contact.userfield.add](./userfield/crm-contact-userfield-add.md).  ||
||**PARENT_ID_...** | Relationship fields. If there are SPAs associated with contacts in the account, for each such SPA there is a field that stores the relationship between this SPA and the contact. The field itself stores the identifier of the element of that SPA. For example, the field `PARENT_ID_153` - relationship with the SPA `entityTypeId=153`, stores the identifier of the element of that SPA associated with the current contact. ||
|| {% note tip "Fields for relationships with external data sources" %}

If the contact was created by an external system, then:
- the field `ORIGINATOR_ID` stores the string identifier of that system
- the field `ORIGIN_ID` stores the string identifier of the contact in that external system
- the field `ORIGIN_VERSION` stores the version of the contact data in that external system

{% endnote %} |> ||
|| **ORIGINATOR_ID**
[`string`][1] | Identifier of the external system that is the source of data about this contact. ||
|| **ORIGIN_ID**
[`string`][1] | Version of the contact data in the external system. Used to protect data from accidental overwriting by the external system. If the data was imported and not changed in the external system, such data can be edited in CRM without fear that the next export will lead to data overwriting. ||
|| **ORIGIN_VERSION**
[`string`][1] | Version of the original data ||
|| {% note tip "Deprecated fields" %}

Address fields in the contact are deprecated and are only used for compatibility mode. To work with the address, use [requisites](../requisites/index.md).

{% endnote %} |> ||
|| **ADDRESS**
[`string`][1] | Address (deprecated) ||
|| **ADDRESS_2**
[`string`][1] | Second line of the address (deprecated) ||
|| **ADDRESS_CITY**
[`string`][1] | City (deprecated) ||
|| **ADDRESS_POSTAL_CODE**
[`string`][1] | Postal code (deprecated) ||
|| **ADDRESS_REGION**
[`string`][1] | Region (deprecated) ||
|| **ADDRESS_PROVINCE**
[`string`][1] | Province (deprecated) ||
|| **ADDRESS_COUNTRY**
[`string`][1] | Country (deprecated) ||
|| **ADDRESS_COUNTRY_CODE**
[`string`][1] | Country code (deprecated) ||
|| **ADDRESS_LOC_ADDR_ID**
[`integer`][1] | Location address identifier (deprecated) ||
|#

### Parameter params

#|
|| **Parameter**
`type` | **Description** ||
|| **REGISTER_SONET_EVENT**
[`boolean`][1] | Whether to register the contact update event in the activity stream

Possible values:
- `Y` - yes
- `N` - no

Default - `N` ||
|| **REGISTER_HISTORY_EVENT**
[`boolean`][1] | Whether to register the contact update in history

Possible values:
- `Y` - yes
- `N` - no

Default - `Y` ||
|#


## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

Example of updating a contact with `id = 43`

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

    ```js
    BX24.callMethod(
        'crm.contact.update',
        {
            id: 43,
            fields: {
                NAME: "Sergey",
                BIRTHDATE: '11.11.1999',
                TYPE_ID: "RECOMMENDATION",
                SOURCE_ID: "WEB",
                POST: "Network Administrator",
                COMMENTS: "New comment",
                OPENED: "N",
                EXPORT: "Y",
                ASSIGNED_BY_ID: 1,
                COMPANY_ID: 12,
                COMPANY_IDS: [13, 15],
                UF_CRM_1720697698689: "Example of a new value for a user-defined field of type \"String\"",
                PARENT_ID_1224: 14,
            },
            params: {
                REGISTER_SONET_EVENT: "N",
                REGISTER_HISTORY_EVENT: "N",
            },
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

### Working with Multiple Fields

#### Updating a Multiple Field
To overwrite an existing value of a multiple field, you need to pass the `ID` of the field you want to change and its new value/type.

Suppose we have the following values for the `PHONE` field:
```json
[
    {
        "ID": "222",
        "VALUE_TYPE": "WORK",
        "VALUE": "111111",
        "TYPE_ID": "PHONE"
    },
    {
        "ID": "223",
        "VALUE_TYPE": "WORK",
        "VALUE": "222222",
        "TYPE_ID": "PHONE"
    },
    {
        "ID": "224",
        "VALUE_TYPE": "WORK",
        "VALUE": "333333",
        "TYPE_ID": "PHONE"
    }
]
```

Then, to change the value of the phone with `ID = 223`, the `fields` parameter will be:

```json
{
    "fields": {
        "PHONE": [
            {
                "ID": 223,
                "VALUE": "444444",
                "VALUE_TYPE": "MOBILE"
            }
        ]
    }
}
```

#### Deleting a Single Value from a Multiple Field
To delete one of the values from a multiple field, you need to pass their identifiers and either the parameter `DELETE = 'Y'` or an empty `VALUE`.

Suppose we have the following values for the `PHONE` field:
```json
[
    {
        "ID": "222",
        "VALUE_TYPE": "WORK",
        "VALUE": "111111",
        "TYPE_ID": "PHONE"
    },
    {
        "ID": "223",
        "VALUE_TYPE": "WORK",
        "VALUE": "222222",
        "TYPE_ID": "PHONE"
    },
    {
        "ID": "224",
        "VALUE_TYPE": "WORK",
        "VALUE": "333333",
        "TYPE_ID": "PHONE"
    },
    {
      "ID": "225",
      "VALUE_TYPE": "WORK",
      "VALUE": "44444",
      "TYPE_ID": "PHONE"
    }
]
```

Let's consider deleting all values except for the phone with `ID = 225`, in all possible ways:

```json
{
    "fields": {
        "PHONE": [
            {
                "ID": 222,
                "DELETE": "Y"
            },
            {
                "ID": 223,
                "VALUE": ""
            },
            {
                "ID": 224
            }
        ]
    }
}
```

As a result, we will only have:

```json
[
    {
        "ID": "225",
        "VALUE_TYPE": "WORK",
        "VALUE": "44444",
        "TYPE_ID": "PHONE"
    }
]
```

#### Adding a New Value to a Multiple Field
To add a new value, simply enter it. Existing values will not be changed.

Example of adding a new value `55555`:
```json
{
    "fields": {
        "PHONE": [
            {
                "VALUE": "55555",
                "VALUE_TYPE": "WORK"
            }
        ]
    }
}
```

### Handling the Response

HTTP Status: **200**

```json
{
  "result": true,
  "time": {
    "start": 1723820393.459406,
    "finish": 1723820396.129949,
    "duration": 2.6705431938171387,
    "processing": 2.1193439960479736,
    "date_start": "2024-08-16T16:59:53+02:00",
    "date_finish": "2024-08-16T16:59:56+02:00"
  }
}
```

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response, `true` in case of success ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
  "error": "",
  "error_description": "Contact is not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code**      | **Description** | **Value** ||
|| `-`          | Parameter 'fields' must be an array. | The parameter `fields` is not an object ||
|| `-`          | Parameter 'params' must be an array. | The parameter `params` is not an object ||
|| `-`          | Access denied. | The user does not have permission to "Edit" contacts ||
|| `-`          | Exhausted allocated disk resource. | ||
|| `ERROR_CORE` | The field "Work e-mail" contains an invalid address. | ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](crm-contact-fields.md)
- [{#T}](crm-contact-add.md)
- [{#T}](crm-contact-get.md)
- [{#T}](crm-contact-list.md)
- [{#T}](crm-contact-delete.md)

[1]: ../../data-types.md
[2]: ../status/crm-status-list.md