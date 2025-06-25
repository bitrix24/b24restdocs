# Get Contact by Id crm.contact.get

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "read" access permission for contacts

The method `crm.contact.get` returns a contact by its identifier.

To get a list of companies associated with the contact, use the method [`crm.contact.company.items.get`](company/crm-contact-company-items-get.md).

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`][1] | Identifier of the contact. Can be obtained using the methods [`crm.contact.list`](crm-contact-list.md) or [`crm.contact.add`](crm-contact-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Get contact with `id = 23`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":23}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":23,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.get
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
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.get',
        [
            'ID' => 23
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)

    ```php        
    try {
        $contactId = 123; // Example contact ID
        $contactResult = $serviceBuilder
            ->getCRMScope()
            ->contact()
            ->get($contactId);
        $itemResult = $contactResult->contact();
        print("ID: " . $itemResult->ID . PHP_EOL);
        print("Name: " . $itemResult->NAME . PHP_EOL);
        print("Last Name: " . $itemResult->LAST_NAME . PHP_EOL);
        print("Birthday: " . $itemResult->BIRTHDATE?->format(DATE_ATOM) . PHP_EOL);
        print("Created Date: " . $itemResult->DATE_CREATE->format(DATE_ATOM) . PHP_EOL);
        print("Modified Date: " . $itemResult->DATE_MODIFY->format(DATE_ATOM) . PHP_EOL);
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage() . PHP_EOL);
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": "43",
        "POST": "Administrator",
        "COMMENTS": "\nExample comment within the contact\n\n[B]Bold text[\/B]\n[I]Italic[\/I]\n[U]Underlined[\/U]\n[S]Strikethrough[\/S]\n[B][I][U][S]Mix[\/S][\/U][\/I][\/B]\n\n[LIST]\n[*]List item #1\n[*]List item #2\n[*]List item #3\n[\/LIST]\n\n[LIST=1]\n[*]Numbered list item #1\n[*]Numbered list item #2\n[*]Numbered list item #3\n[\/LIST]\n",
        "HONORIFIC": "HNR_EN_1",
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
        "UTM_SOURCE": "google",
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
            "VALUE": "johnsmith@example.mailing",
            "TYPE_ID": "EMAIL"
        },
        {
            "ID": "159",
            "VALUE_TYPE": "WORK",
            "VALUE": "johnsmith@example.work",
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

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`contact`](#contact) | Root element of the response. Contains information about the contact fields. The structure is described [below](#contact) ||
|| **time**
[`time`][1] | Object containing information about the request execution time ||
|#

### contact

#|
|| **Name**
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
[`date`][1] | Date of birth ||
|| **EXPORT**
[`boolean`][1] | Whether the contact is included in the export. Possible values:
- `Y` — yes
- `N` — no ||
|| **HAS_PHONE**
[`boolean`][1] | Whether a phone number is provided. Possible values:
- `Y` — yes
- `N` — no ||
|| **HAS_EMAIL**
[`boolean`][1] | Whether an e-mail is provided. Possible values:
- `Y` — yes
- `N` — no ||
|| **HAS_IMOL**
[`boolean`][1] | Whether an open channel is provided. Possible values:
- `Y` — yes
- `N` — no ||
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
[`boolean`][1] | Available to everyone. Possible values:
- `Y` — yes
- `N` — no ||
|| **FACE_ID**
[`integer`][1] | Link to faces from the `faceid` module ||
|| **LAST_ACTIVITY_TIME**
[`datetime`][1] | Last activity ||
|| **LAST_ACTIVITY_BY**
[`user`][1] | Who performed the last activity in the timeline ||
|| **UTM_SOURCE**
[`string`][1] | Advertising system (Google Ads, etc.) ||
|| **UTM_MEDIUM**
[`string`][1] | Type of traffic. Possible values:
- `CPC` — ads
- `CPM` — banners ||
|| **UTM_CAMPAIGN**
[`string`][1] | Designation of the advertising campaign ||
|| **UTM_CONTENT**
[`string`][1] | Content of the campaign. For example, for contextual ads ||
|| **UTM_TERM**
[`string`][1] | Search condition of the campaign. For example, keywords for contextual advertising ||
|| **PHONE**
[`crm_multifield[]`](../data-types.md) | Phone ||
|| **EMAIL**
[`crm_multifield[]`](../data-types.md) | E-mail ||
|| **WEB**
[`crm_multifield[]`](../data-types.md) | Website ||
|| **IM**
[`crm_multifield[]`](../data-types.md) | Messenger ||
|| **LINK**
[`crm_multifield[]`](../data-types.md) | Links. Service field ||
|#

**Fields for connection with external data sources**

If the contact was created by an external system, then:
- the field `ORIGINATOR_ID` stores the string identifier of that system
- the field `ORIGIN_ID` stores the string identifier of the contact in that external system
- the field `ORIGIN_VERSION` stores the version of the contact data in that external system

#|
|| **Name**
`type` | **Description** ||
|| **ORIGINATOR_ID**
[`string`][1] | External source ||
|| **ORIGIN_ID**
[`string`][1] | Identifier of the element in the external source ||
|| **ORIGIN_VERSION**
[`string`][1] | Version of the original ||
|#

**Deprecated Fields**

Address fields in the contact are deprecated and are only used in compatibility mode. To work with the address, use [details](../requisites/index.md).

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
|| **ADDRESS_LOC_ADDR_ID**
[`integer`][1] | Identifier of the location address ||
|#

{% note tip "Fields of type `crm_multifield`" %}

Fields of type `crm_multifield` (`PHONE`, `EMAIL`, `WEB`, `IM`, `LINK`) are explicitly returned by this method only if the values of this field are not equal to `null`.

{% endnote %}

## Error Handling

HTTP status: **400**

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
|| `ID is not defined or invalid` | The `id` parameter is not provided or the provided value is not a positive integer ||
|| `Access denied` | The user does not have permission to "Read" the contact ||
|| `Not found` | The contact with the provided `id` was not found ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-add.md)
- [{#T}](./crm-contact-update.md)
- [{#T}](./crm-contact-list.md)
- [{#T}](./crm-contact-delete.md)
- [{#T}](./crm-contact-fields.md)
- [{#T}](../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-activity-to-contact.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-send-email.md)

[1]: ../../data-types.md