# Get Contact by Id crm.contact.get

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "read" access permission for contacts

The method `crm.contact.get` returns a contact by its identifier.

{% note info "Getting a List of Companies" %}

To retrieve a list of companies associated with the contact, use the method [`crm.contact.company.items.get`](company/crm-contact-company-items-get.md)

{% endnote %}

## Method Parameters

{% include [Footnote on Parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **id^*^**
[`integer`][1] | Identifier of the contact. Can be obtained using the methods [`crm.contact.list`](crm-contact-list.md) or [`crm.contact.add`](crm-contact-add.md) ||
|#

## Code Examples

{% include [Footnote on Examples](../../../_includes/examples.md) %}

Get contact with `id = 23`

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
            'crm.contact.get',
            {
                id: 23,
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

HTTP Status: **200**

```json
{
  "result": {
    "ID": "43",
    "POST": "Administrator",
    "COMMENTS": "\nExample comment within the contact\n\n[B]Bold text[\/B]\n[I]Italic[\/I]\n[U]Underlined[\/U]\n[S]Strikethrough[\/S]\n[B][I][U][S]Mix[\/S][\/U][\/I][\/B]\n\n[LIST]\n[*]List item #1\n[*]List item #2\n[*]List item #3\n[\/LIST]\n\n[LIST=1]\n[*]Numbered list item #1\n[*]Numbered list item #2\n[*]Numbered list item #3\n[\/LIST]\n",
    "HONORIFIC": "HNR_RU_1",
    "NAME": "John",
    "SECOND_NAME": "Doe",
    "LAST_NAME": "Smith",
    "PHOTO": null,
    "LEAD_ID": null,
    "TYPE_ID": "PARTNER",
    "SOURCE_ID": "WEB",
    "SOURCE_DESCRIPTION": "*Additional information about the source*",
    "COMPANY_ID": "12",
    "BIRTHDATE": "2001-11-11T02:00:00+02:00",
    "EXPORT": "N",
    "HAS_PHONE": "Y",
    "HAS_EMAIL": "Y",
    "HAS_IMOL": "N",
    "DATE_CREATE": "2024-08-15T10:38:21+02:00",
    "DATE_MODIFY": "2024-08-15T10:38:21+02:00",
    "ASSIGNED_BY_ID": "6",
    "CREATED_BY_ID": "1",
    "MODIFY_BY_ID": "1",
    "OPENED": "Y",
    "ORIGINATOR_ID": null,
    "ORIGIN_ID": null,
    "ORIGIN_VERSION": null,
    "FACE_ID": null,
    "LAST_ACTIVITY_TIME": "2024-08-15T10:38:21+02:00",
    "ADDRESS": null,
    "ADDRESS_2": null,
    "ADDRESS_CITY": null,
    "ADDRESS_POSTAL_CODE": null,
    "ADDRESS_REGION": null,
    "ADDRESS_PROVINCE": null,
    "ADDRESS_COUNTRY": null,
    "ADDRESS_LOC_ADDR_ID": null,
    "UTM_SOURCE": "yandex",
    "UTM_MEDIUM": "CPC",
    "UTM_CAMPAIGN": "summer_sale",
    "UTM_CONTENT": "header_banner",
    "UTM_TERM": "discount",
    "PARENT_ID_1224": "12",
    "LAST_ACTIVITY_BY": "1",
    "UF_CRM_1720697698689": "Example value of a custom field with type \u0022String\u0022",
    "PHONE": [
      {
        "ID": "156",
        "VALUE_TYPE": "WORK",
        "VALUE": "+13333333555",
        "TYPE_ID": "PHONE"
      },
      {
        "ID": "157",
        "VALUE_TYPE": "HOME",
        "VALUE": "+15599888666",
        "TYPE_ID": "PHONE"
      }
    ],
    "EMAIL": [
      {
        "ID": "158",
        "VALUE_TYPE": "MAILING",
        "VALUE": "john.smith@example.mailing",
        "TYPE_ID": "EMAIL"
      },
      {
        "ID": "159",
        "VALUE_TYPE": "WORK",
        "VALUE": "john.smith@example.work",
        "TYPE_ID": "EMAIL"
      }
    ]
  },
  "time": {
    "start": 1723736139.883652,
    "finish": 1723736140.299369,
    "duration": 0.41571712493896484,
    "processing": 0.14158892631530762,
    "date_start": "2024-08-15T17:35:39+02:00",
    "date_finish": "2024-08-15T17:35:40+02:00"
  }
}
```

### Returned Values

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`contact`](#contact) | Root element of the response. Contains information about the contact fields. The structure is described [below](#contact) ||
|| **time**
[`time`][1] | Object containing information about the request execution time ||
|#

#### contact

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`][1] | Identifier of the contact ||
|| **POST**
[`string`][1] | Position ||
|| **COMMENTS**
[`text`][1] | Comment ||
|| **HONORIFIC**
[`crm_status`](../data-types.md) | Salutation ||
|| **NAME**
[`string`][1] | First name ||
|| **SECOND_NAME**
[`string`][1] | Middle name ||
|| **LAST_NAME**
[`string`][1] | Last name ||
|| **PHOTO**
[`file`][1] | Photo ||
|| **LEAD_ID**
[`crm_lead`](../data-types.md) | Identifier of the lead based on which the contact was created ||
|| **TYPE_ID**
[`crm_status`](../data-types.md) | Type of contact ||
|| **SOURCE_ID**
[`crm_status`](../data-types.md) | Source ||
|| **SOURCE_DESCRIPTION**
[`text`][1] | Additional information about the source ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the main company ||
|| **BIRTHDATE**
[`date`][1] | Birthdate ||
|| **EXPORT**
[`boolean`][1] | Participates in contact export

`Y` - Yes
`N` - No ||
|| **HAS_PHONE**
[`boolean`][1] | Phone is set

`Y` - Yes
`N` - No
||
|| **HAS_EMAIL**
[`boolean`][1] | E-mail is set

`Y` - Yes
`N` - No ||
|| **HAS_IMOL**
[`boolean`][1] | Open line is set

`Y` - Yes
`N` - No ||
|| **DATE_CREATE**
[`datetime`][1] | Creation date ||
|| **DATE_MODIFY**
[`datetime`][1] | Modification date ||
|| **ASSIGNED_BY_ID**
[`user`][1] | Responsible person ||
|| **CREATED_BY_ID**
[`user`][1] | Created by ||
|| **MODIFY_BY_ID**
[`user`][1] | Modified by ||
|| **OPENED**
[`boolean`][1] | Available to everyone

`Y` - Yes
`N` - No
||
|| **FACE_ID**
[`integer`][1] | Link to faces from the `faceid` module. ||
|| **LAST_ACTIVITY_TIME**
[`datetime`][1] | Last activity ||
|| **LAST_ACTIVITY_BY**
[`user`][1] | Who performed the last activity in the timeline ||
|| **UTM_SOURCE**
[`string`][1] | Advertising system. Yandex-Direct, Google-Adwords, and others ||
|| **UTM_MEDIUM**
[`string`][1] | Type of traffic. Possible values:
- CPC — ads
- CPM — banners ||
|| **UTM_CAMPAIGN**
[`string`][1] | Designation of the advertising campaign ||
|| **UTM_CONTENT**
[`string`][1] | Content of the campaign. For example, for contextual ads ||
|| **UTM_TERM**
[`string`][1] | Search condition of the campaign. For example, keywords of contextual advertising ||
|| **PHONE**
[`crm_multifield[]`](../data-types.md) | Phone ||
|| **EMAIL**
[`crm_multifield[]`](../data-types.md) | E-mail ||
|| **WEB**
[`crm_multifield[]`](../data-types.md) | Website ||
|| **IM**
[`crm_multifield[]`](../data-types.md) | Messenger ||
|| **LINK**
[`crm_multifield[]`](../data-types.md) | Links. Service. ||
|| {% note tip "Fields for External Data Sources" %}

If the contact was created by an external system, then:
- the field `ORIGINATOR_ID` stores the string identifier of that system
- the field `ORIGIN_ID` stores the string identifier of the contact in that external system
- the field `ORIGIN_VERSION` stores the version of the contact data in that external system

{% endnote %} |> ||
|| **ORIGINATOR_ID**
[`string`][1] | External source ||
|| **ORIGIN_ID**
[`string`][1] | Identifier of the item in the external source ||
|| **ORIGIN_VERSION**
[`string`][1] | Version of the original ||
|| {% note tip "Deprecated Fields" %}

Address fields in the contact are deprecated and are only used in compatibility mode. To work with the address, use [requisites](../requisites/index.md).

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
|| **ADDRESS_LOC_ADDR_ID**
[`integer`][1] | Location address identifier (deprecated) ||
|#

{% note tip "Fields of type `crm_multifield`" %}

Fields of type `crm_multifield` (`PHONE`, `EMAIL`, `WEB`, `IM`, `LINK`) are explicitly returned by this method only if the value of this field is not equal to `null`

{% endnote %}

## Error Handling

HTTP Status: **400**

```json
{
  "error": "",
  "error_description": "ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Description** | **Value** ||
|| ID is not defined or invalid. | The parameter `id` is not provided, or the provided value is not an integer greater than 0 ||
|| Access denied. | The user does not have permission for "Read" contact ||
|| Not found | Contact with the provided `id` was not found ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](crm-contact-fields.md)
- [{#T}](crm-contact-add.md)
- [{#T}](crm-contact-update.md)
- [{#T}](crm-contact-list.md)
- [{#T}](crm-contact-delete.md)

[1]: ../../data-types.md