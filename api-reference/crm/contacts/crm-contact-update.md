# Update Contact crm.contact.update

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "edit" access permission for contacts

{% note warning "Method Development Stopped" %}

The method `crm.contact.update` continues to function, but there is a more relevant alternative [crm.item.update](../universal/crm-item-update.md).

{% endnote %}

The method `crm.contact.update` updates an existing contact.

It is recommended to pass the complete set of address fields when updating the address.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`][1] | Identifier of the contact to be changed.

The identifier can be obtained using the methods [`crm.contact.list`](crm-contact-list.md) or [`crm.contact.add`](crm-contact-add.md) ||
|| **fields***
[`object`][1] | Object in the format:

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

The list of available fields is described [below](#parameter-fields).

An incorrect field in `fields` will be ignored.

Only those fields that need to be changed should be passed in `fields`
||
|| **params**
[`object`][1] | Object containing a set of additional parameters.

The structure and possible values are described [below](#parameter-params)
|#

### Parameter fields {#parameter-fields}

#|
|| **Name**
`type` | **Description** ||
|| **HONORIFIC**
[`crm_status`](../data-types.md) | Salutation.

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
[`date`][1] | Date of birth ||
|| **TYPE_ID**
[`crm_status`](../data-types.md) | Contact type.
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
[`boolean`][1] | Is it available to everyone? Possible values:
- `Y` — yes
- `N` — no ||
|| **EXPORT**
[`boolean`][1] | Is the contact participating in the export? Possible values:
- `Y` — yes
- `N` — no ||
|| **ASSIGNED_BY_ID**
[`user`][1] | Identifier of the user responsible for the item ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the main company for the contact.

The list of companies can be obtained using the method [`crm.item.list`](../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **COMPANY_IDS**
[`crm_company[]`](../data-types.md) | Array of identifiers of companies to which the contact is linked.

The list of companies can be obtained using the method [`crm.item.list`](../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **UTM_SOURCE**
[`string`][1] | Advertising system (Google Ads, etc.) ||
|| **UTM_MEDIUM**
[`string`][1] | Type of traffic. Possible values:
- `CPC` — ads
- `CPM` — banners ||
|| **UTM_CAMPAIGN**
[`string`][1] | Advertising campaign designation ||
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
||**UF_...**  | Custom fields. For example, `UF_CRM_25534736`. 

Depending on the account settings, contacts may have a set of custom fields of specific types. 

To change file fields, it is recommended to use the method [crm.item.update](../universal/crm-item-update.md).

A custom field can be added to a contact using the method [crm.contact.userfield.add](./userfield/crm-contact-userfield-add.md) ||
||**PARENT_ID_...** | Relationship fields. 

If there are smart processes related to contacts in the account, there is a field for each such smart process that stores the relationship between this smart process and the contact. The field itself stores the identifier of the element of that smart process. 

For example, the field `PARENT_ID_153` — relationship with the smart process `entityTypeId=153`. It stores the identifier of the element of this smart process related to the current contact ||
|#

**Fields for external data sources**

If the contact was created by an external system, then:
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

If the data was imported and not changed in the external system, then such data can be edited in CRM without fear that the next export will lead to data overwriting ||
|| **ORIGIN_VERSION**
[`string`][1] | Version of the original ||
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
[`integer`][1] | Identifier of the location address ||
|#

### Parameter params {#parameter-params}

#|
|| **Name**
`type` | **Description** ||
|| **REGISTER_SONET_EVENT**
[`boolean`][1] | Should the update event of the contact be registered in the activity stream? Possible values:
- `Y` — yes
- `N` — no

Default is `N` ||
|| **REGISTER_HISTORY_EVENT**
[`boolean`][1] | Should the update of the contact be registered in the history? Possible values:
- `Y` — yes
- `N` — no

Default is `Y` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Update contact with `id = 43`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":43,"FIELDS":{"NAME":"John","BIRTHDATE":"11.11.1999","TYPE_ID":"RECOMMENDATION","SOURCE_ID":"WEB","POST":"Network Administrator","COMMENTS":"New comment","OPENED":"N","EXPORT":"Y","ASSIGNED_BY_ID":1,"COMPANY_ID":12,"COMPANY_IDS":[13,15],"UF_CRM_1720697698689":"Example of a new value for a custom field of type \"String\"","PARENT_ID_1224":14},"PARAMS":{"REGISTER_SONET_EVENT":"N","REGISTER_HISTORY_EVENT":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":43,"FIELDS":{"NAME":"John","BIRTHDATE":"11.11.1999","TYPE_ID":"RECOMMENDATION","SOURCE_ID":"WEB","POST":"Network Administrator","COMMENTS":"New comment","OPENED":"N","EXPORT":"Y","ASSIGNED_BY_ID":1,"COMPANY_ID":12,"COMPANY_IDS":[13,15],"UF_CRM_1720697698689":"Example of a new value for a custom field of type \"String\"","PARENT_ID_1224":14},"PARAMS":{"REGISTER_SONET_EVENT":"N","REGISTER_HISTORY_EVENT":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.contact.update',
    		{
    			id: 43,
    			fields: {
    				NAME: "John",
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
    				UF_CRM_1720697698689: "Example of a new value for a custom field of type \"String\"",
    				PARENT_ID_1224: 14,
    			},
    			params: {
    				REGISTER_SONET_EVENT: "N",
    				REGISTER_HISTORY_EVENT: "N",
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	result.error()
    		? console.error(result.error())
    		: console.info(result)
    	;
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php        
    try {
        $contactId = 123; // Example contact ID
        $fields = [
            'NAME' => 'John',
            'LAST_NAME' => 'Doe',
            'BIRTHDATE' => (new DateTime('1990-01-01'))->format(DateTime::ATOM),
            'PHONE' => '123456789',
            'EMAIL' => 'john.doe@example.com',
            'ADDRESS' => '123 Main St',
            'ADDRESS_CITY' => 'Anytown',
            'ADDRESS_COUNTRY' => 'USA',
        ];
        $params = [
            'REGISTER_SONET_EVENT' => 'Y',
        ];
        $result = $serviceBuilder
            ->getCRMScope()
            ->contact()
            ->update($contactId, $fields, $params);
        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print('Update failed.');
        }
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.contact.update',
        {
            id: 43,
            fields: {
                NAME: "John",
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
                UF_CRM_1720697698689: "Example of a new value for a custom field of type \"String\"",
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.update',
        [
            'ID' => 43,
            'FIELDS' => [
                'NAME' => 'John',
                'BIRTHDATE' => '11.11.1999',
                'TYPE_ID' => 'RECOMMENDATION',
                'SOURCE_ID' => 'WEB',
                'POST' => 'Network Administrator',
                'COMMENTS' => 'New comment',
                'OPENED' => 'N',
                'EXPORT' => 'Y',
                'ASSIGNED_BY_ID' => 1,
                'COMPANY_ID' => 12,
                'COMPANY_IDS' => [13, 15],
                'UF_CRM_1720697698689' => 'Example of a new value for a custom field of type "String"',
                'PARENT_ID_1224' => 14,
            ],
            'PARAMS' => [
                'REGISTER_SONET_EVENT' => 'N',
                'REGISTER_HISTORY_EVENT' => 'N',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Working with Multiple Fields

### Update a Multiple Field

To overwrite an existing value of a multiple field, pass the `ID` of the field you want to change and its new value/type.

Suppose there are the following values for the `PHONE` field:

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

To change the value of the phone with `ID = 223`, pass the following `fields` parameter:

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

### Delete a Single Value from a Multiple Field

To delete one of the values from a multiple field, pass their identifiers and either the parameter `DELETE = 'Y'` or an empty `VALUE`.

Suppose there are the following values for the `PHONE` field:

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

Let's consider ways to delete all values except for the phone with `ID = 225`:

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

As a result, the following will remain:

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

### Add a New Value to a Multiple Field

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

### Response Handling

HTTP status: **200**

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

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response, `true` in case of success ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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
|| `-`          | `Parameter 'fields' must be array` | The `fields` parameter is not an object ||
|| `-`          | `Parameter 'params' must be array` | The `params` parameter is not an object ||
|| `-`          | `Access denied` | The user does not have permission to "Edit" contacts ||
|| `-`          | Disk resource exhausted | ||
|| `ERROR_CORE` | The field `Work e-mail` contains an incorrect address | ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-add.md)
- [{#T}](./crm-contact-get.md)
- [{#T}](./crm-contact-list.md)
- [{#T}](./crm-contact-delete.md)
- [{#T}](./crm-contact-fields.md)
- [{#T}](../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)

[1]: ../../data-types.md
[2]: ../status/crm-status-list.md