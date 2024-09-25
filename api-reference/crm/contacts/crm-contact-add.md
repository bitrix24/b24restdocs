# Create a New Contact crm.contact.add

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "add|import" permission for contacts

The method `crm.contact.add` creates a new contact.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`object`][1] | Object format:

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

The list of available fields is described [below](#parameter-fields).

An incorrect field in `fields` will be ignored ||
|| **params**
[`object`][1] | Object containing a set of additional parameters.

The structure and possible values are described [below](#parameter-params) ||
|#

### Parameter fields {#parameter-fields}

#|
|| **Name**
`type` | **Description** ||
|| **HONORIFIC**
[`crm_status`](../data-types.md) | Salutation.

The list of available salutation types can be obtained using the method [`crm.status.list`][2] with the filter `{ ENTITY_ID: "HONORIFIC" }`.

Default — the first available salutation type ||
|| **NAME**
[`string`][1] | First name ||
|| **SECOND_NAME**
[`string`][1] | Middle name ||
|| **LAST_NAME**
[`string`][1] | Last name ||
|| **PHOTO**
[`file`][1] | Photo ||
|| **BIRTHDATE**
[`date`][1] | Date of birth ||
|| **TYPE_ID**
[`crm_status`](../data-types.md) | Contact type.

The list of available contact types can be obtained using the method [`crm.status.list`][2] with the filter `{ ENTITY_ID: "CONTACT_TYPE" }`.

Default — the first available contact type ||
|| **SOURCE_ID**
[`crm_status`](../data-types.md) | Source.

The list of available source types can be obtained using the method [`crm.status.list`][2] with the filter `{ ENTITY_ID: "SOURCE" }`.

Default — the first available source type ||
|| **SOURCE_DESCRIPTION**
[`string`][1] | Additional information about the source ||
|| **POST**
[`string`][1] | Position ||
|| **COMMENTS**
[`string`][1] | Comment. Supports bb-codes ||
|| **OPENED**
[`boolean`][1] | Is it available to everyone? Possible values:
- `Y` — yes
- `N` — no

Default is `Y`. The default value can be changed in the CRM settings ||
|| **EXPORT**
[`boolean`][1] | Is the contact included in the export? Possible values:
- `Y` — yes
- `N` — no

Default is `Y` ||
|| **ASSIGNED_BY_ID**
[`user`][1] | Identifier of the user responsible for the element.

Default — the identifier of the user calling the method ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the main company for the contact.

The list of companies can be obtained using the method [`crm.item.list`](../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **COMPANY_IDS**
[`crm_company[]`](../data-types.md) | Array of identifiers of companies associated with the contact.

The list of companies can be obtained using the method [`crm.item.list`](../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **UTM_SOURCE**
[`string`][1] | Advertising system (Google Ads, etc.) ||
|| **UTM_MEDIUM**
[`string`][1] | Traffic type. Possible values:
- `CPC` — ads
- `CPM` — banners ||
|| **UTM_CAMPAIGN**
[`string`][1] | Advertising campaign designation ||
|| **UTM_CONTENT**
[`string`][1] | Content of the campaign. For example, for contextual ads ||
|| **UTM_TERM**
[`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising ||
|| **TRACE**
[`string`][1] | Information for [Sales Intelligence](../../../tutorials/crm/how-to-use-analitycs/use-analitics-for-add-contact.md) ||
|| **PHONE**
[`crm_multifield[]`](../data-types.md) | Phone ||
|| **EMAIL**
[`crm_multifield[]`](../data-types.md) | E-mail ||
|| **WEB**
[`crm_multifield[]`](../data-types.md) | Website ||
|| **IM**
[`crm_multifield[]`](../data-types.md) | Messenger ||
|| **LINK**
[`crm_multifield[]`](../data-types.md) | Links. System field ||
||**UF_...**  | Custom fields. For example, `UF_CRM_25534736`. 

Depending on the account settings, contacts may have a set of custom fields of specific types. 

You can add a custom field to a contact using the method [crm.contact.userfield.add](./userfield/crm-contact-userfield-add.md) ||
||**PARENT_ID_...** | Relationship fields. 

If there are SPAs associated with contacts in the account, there is a field for each such SPA that stores the relationship between this SPA and the contact. The field itself stores the identifier of the element of that SPA. 

For example, the field `PARENT_ID_153` — relationship with the SPA `entityTypeId=153`. It stores the identifier of the element of this SPA associated with the current contact ||
|#

**Fields for external data sources**

If the contact is created by an external system, then:
- the field `ORIGINATOR_ID` stores the string identifier of that system
- the field `ORIGIN_ID` stores the string identifier of the contact in that external system
- the field `ORIGIN_VERSION` stores the version of the contact data in that external system

#|
|| **Name**
`type` | **Description** ||
|| **ORIGINATOR_ID**
[`string`][1] | Identifier of the external system that is the source of data about this contact ||
|| **ORIGIN_ID**
[`string`][1] | Version of the contact data in the external system. Used to protect data from accidental overwriting by the external system. 

If the data was imported and not changed in the external system, such data can be edited in CRM without fear that the next export will lead to data overwriting ||
|| **ORIGIN_VERSION**
[`string`][1] | Version of the original ||
|#

**Import**

The fields are available for filling when the parameter `IMPORT = 'Y'` is passed in the `params` parameter.

#|
|| **Name**
`type` | **Description** ||
|| **DATE_CREATE**
[`datetime`][1] | Creation date.

Available when `IMPORT = Y` is passed in `params`.

Cannot be earlier than the creation date of the last created contact ||
|| **DATE_MODIFY**
[`datetime`][1] | Modification date.

Available when `IMPORT = Y` is passed in `params` ||
|| **CREATED_BY_ID**
[`user`][1] | Created by.

Available when `IMPORT = Y` is passed in `params` ||
|| **MODIFY_BY_ID**
[`user`][1] | Modified by.

Available when `IMPORT = Y` is passed in `params` ||
|#

**Deprecated fields**

Address fields in the contact are deprecated and are only used in compatibility mode. To work with the address, use [requisites](../requisites/index.md).

#|
|| **Name**
`type` | **Description** ||
|| **ADDRESS**
[`string`][1] | Address ||
|| **ADDRESS_2**
[`string`][1] | Second line of the address ||
|| **ADDRESS_CITY**
[`string`][1] | City ||
|| **ADDRESS_POSTAL_CODE**
[`string`][1] | Postal code ||
|| **ADDRESS_REGION**
[`string`][1] | Region ||
|| **ADDRESS_PROVINCE**
[`string`][1] | Province ||
|| **ADDRESS_COUNTRY**
[`string`][1] | Country ||
|| **ADDRESS_COUNTRY_CODE**
[`string`][1] | Country code ||
|| **ADDRESS_LOC_ADDR_ID**
[`integer`][1] | Location address identifier ||
|#

### Parameter params {#parameter-params}

#|
|| **Name**
`type` | **Description** ||
|| **REGISTER_SONET_EVENT**
[`boolean`][1] | Should the event of adding a contact be registered in the activity stream? Possible values:
- `Y` — yes
- `N` — no

Default is `N` ||
|| **IMPORT**
[`boolean`][1] | Is import mode enabled? Possible values:
- `Y` — yes

To pass the value `No`, you must either not pass the parameter at all or pass the value `0`, `''`

Default is `No` ||
|#

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FIELDS":{"HONORIFIC":"HNR_RU_1","NAME":"John","SECOND_NAME":"Doe","LAST_NAME":"Smith","PHOTO":{"fileData":"**put_photo_data_here**"},"BIRTHDATE":"11.11.2001","TYPE_ID":"PARTNER","SOURCE_ID":"WEB","SOURCE_DESCRIPTION":"*Additional information about the source*","POST":"Administrator","COMMENTS":"**put_comment_here**","OPENED":"Y","EXPORT":"N","ASSIGNED_BY_ID":6,"COMPANY_ID":12,"COMPANY_IDS":[12,13,15],"UTM_SOURCE":"google","UTM_MEDIUM":"CPC","UTM_CAMPAIGN":"summer_sale","UTM_CONTENT":"header_banner","UTM_TERM":"discount","PHONE":[{"VALUE":"+1333333555","VALUE_TYPE":"WORK"},{"VALUE":"+15599888666","VALUE_TYPE":"HOME"}],"EMAIL":[{"VALUE":"john@example.mailing","VALUE_TYPE":"MAILING"},{"VALUE":"john@example.work","VALUE_TYPE":"WORK"}],"UF_CRM_1720697698689":"Example value of a custom field with type \"String\"","PARENT_ID_1224":12}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FIELDS":{"HONORIFIC":"HNR_RU_1","NAME":"John","SECOND_NAME":"Doe","LAST_NAME":"Smith","PHOTO":{"fileData":"**put_photo_data_here**"},"BIRTHDATE":"11.11.2001","TYPE_ID":"PARTNER","SOURCE_ID":"WEB","SOURCE_DESCRIPTION":"*Additional information about the source*","POST":"Administrator","COMMENTS":"**put_comment_here**","OPENED":"Y","EXPORT":"N","ASSIGNED_BY_ID":6,"COMPANY_ID":12,"COMPANY_IDS":[12,13,15],"UTM_SOURCE":"google","UTM_MEDIUM":"CPC","UTM_CAMPAIGN":"summer_sale","UTM_CONTENT":"header_banner","UTM_TERM":"discount","PHONE":[{"VALUE":"+1333333555","VALUE_TYPE":"WORK"},{"VALUE":"+15599888666","VALUE_TYPE":"HOME"}],"EMAIL":[{"VALUE":"john@example.mailing","VALUE_TYPE":"MAILING"},{"VALUE":"john@example.work","VALUE_TYPE":"WORK"}],"UF_CRM_1720697698689":"Example value of a custom field with type \"String\"","PARENT_ID_1224":12},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.add
    ```

- JS

    ```js
    const comment = `
    Example comment inside the contact

    [B]Bold text[/B]
    [I]Italic[/I]
    [U]Underlined[/U]
    [S]Strikethrough[/S]
    [B][I][U][S]Mix[/S][/U][/I][/B]

    [LIST]
    [*]List item #1
    [*]List item #2
    [*]List item #3
    [/LIST]

    [LIST=1]
    [*]Numbered list item #1
    [*]Numbered list item #2
    [*]Numbered list item #3
    [/LIST]
    `;

    BX24.callMethod(
        'crm.contact.add',
        {
            fields: {
                HONORIFIC: "HNR_RU_1",
                NAME: "John",
                SECOND_NAME: "Doe",
                LAST_NAME: "Smith",
                PHOTO: {
                    fileData: document.getElementById('photo'),
                },
                BIRTHDATE: '11.11.2001',
                TYPE_ID: "PARTNER",
                SOURCE_ID: "WEB",
                SOURCE_DESCRIPTION: "*Additional information about the source*",
                POST: "Administrator",
                COMMENTS: comment,
                OPENED: "Y",
                EXPORT: "N",
                ASSIGNED_BY_ID: 6,
                COMPANY_ID: 12,
                COMPANY_IDS: [12, 13, 15],
                UTM_SOURCE: "google",
                UTM_MEDIUM: "CPC",
                UTM_CAMPAIGN: "summer_sale",
                UTM_CONTENT: "header_banner",
                UTM_TERM: "discount",
                PHONE: [
                    {
                        VALUE: "+1333333555",
                        VALUE_TYPE: "WORK",
                    },
                    {
                        VALUE: "+15599888666",
                        VALUE_TYPE: "HOME",
                    }
                ],
                EMAIL: [
                    {
                        VALUE: "john@example.mailing",
                        VALUE_TYPE: "MAILING",
                    },
                    {
                        VALUE: "john@example.work",
                        VALUE_TYPE: "WORK",
                    }
                ],
                UF_CRM_1720697698689: "Example value of a custom field with type \"String\"",
                PARENT_ID_1224: 12,
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
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.add',
        [
            'FIELDS' => [
                'HONORIFIC' => 'HNR_RU_1',
                'NAME' => 'John',
                'SECOND_NAME' => 'Doe',
                'LAST_NAME' => 'Smith',
                'PHOTO' => [
                    'fileData' => $_FILES['photo']
                ],
                'BIRTHDATE' => '11.11.2001',
                'TYPE_ID' => 'PARTNER',
                'SOURCE_ID' => 'WEB',
                'SOURCE_DESCRIPTION' => '*Additional information about the source*',
                'POST' => 'Administrator',
                'COMMENTS' => $comment,
                'OPENED' => 'Y',
                'EXPORT' => 'N',
                'ASSIGNED_BY_ID' => 6,
                'COMPANY_ID' => 12,
                'COMPANY_IDS' => [12, 13, 15],
                'UTM_SOURCE' => 'google',
                'UTM_MEDIUM' => 'CPC',
                'UTM_CAMPAIGN' => 'summer_sale',
                'UTM_CONTENT' => 'header_banner',
                'UTM_TERM' => 'discount',
                'PHONE' => [
                    [
                        'VALUE' => '+1333333555',
                        'VALUE_TYPE' => 'WORK',
                    ],
                    [
                        'VALUE' => '+15599888666',
                        'VALUE_TYPE' => 'HOME',
                    ]
                ],
                'EMAIL' => [
                    [
                        'VALUE' => 'john@example.mailing',
                        'VALUE_TYPE' => 'MAILING',
                    ],
                    [
                        'VALUE' => 'john@example.work',
                        'VALUE_TYPE' => 'WORK',
                    ]
                ],
                'UF_CRM_1720697698689' => 'Example value of a custom field with type "String"',
                'PARENT_ID_1224' => 12,
            ]
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
    "result": 46,
    "time": {
        "start": 1723713732.235658,
        "finish": 1723713733.742049,
        "duration": 1.5063910484313965,
        "processing": 1.1416668891906738,
        "date_start": "2024-08-15T11:22:12+02:00",
        "date_finish": "2024-08-15T11:22:13+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`][1] | Root element of the response, contains the identifier of the created contact ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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
|| `-`     | `Parameter 'fields' must be array` | The parameter `fields` is not an object ||
|| `-`     | `Parameter 'params' must be array` | The parameter `params` is not an object ||
|| `-`     | `Access denied` | The user does not have permission to "Add" or "Import" contacts ||
|| `-`     | Disk resource exhausted | ||
|| `ERROR_CORE` | The field `Work e-mail` contains an invalid address | ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-update.md)
- [{#T}](./crm-contact-get.md)
- [{#T}](./crm-contact-list.md)
- [{#T}](./crm-contact-delete.md)
- [{#T}](./crm-contact-fields.md)

[1]: ../../data-types.md
[2]: ../status/crm-status-list.md