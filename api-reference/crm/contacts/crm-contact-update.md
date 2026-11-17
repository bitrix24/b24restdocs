# Update Contact crm.contact.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "edit" access permission for contacts

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.contact.update](../rest-v3/items/crm-entity-update-rest-v3.md).

{% endnote %}

The method `crm.contact.update` updates an existing contact.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | The identifier of the contact to be changed.

The identifier can be obtained using the methods [`crm.contact.list`](crm-contact-list.md) or [`crm.contact.add`](crm-contact-add.md) ||
|| **fields**
[`object`](../../data-types.md) | An object in the following format:

```text
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

Pass only the fields you want to change in `fields`. When updating an address, pass all address fields: address fields omitted from the request are cleared.

If `fields` is omitted, the method returns `true` without changing anything
||
|| **params**
[`object`](../../data-types.md) | An object containing a set of additional parameters.

The structure and possible values are described [below](#parameter-params)
|#

### Parameter fields {#parameter-fields}

#|
|| **Name**
`type` | **Description** ||
|| **HONORIFIC**
[`crm_status`](../data-types.md) | Salutation.

The list of available salutation types can be obtained using [`crm.status.list`](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "HONORIFIC" }` ||
|| **NAME**
[`string`](../../data-types.md) | First name ||
|| **SECOND_NAME**
[`string`](../../data-types.md) | Middle name ||
|| **LAST_NAME**
[`string`](../../data-types.md) | Last name ||
|| **PHOTO**
[`file`](../../data-types.md) | Photograph ||
|| **BIRTHDATE**
[`date`](../../data-types.md) | Date of birth ||
|| **TYPE_ID**
[`crm_status`](../data-types.md) | Contact type.
The list of available contact types can be obtained using [`crm.status.list`](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "CONTACT_TYPE" }` ||
|| **SOURCE_ID**
[`crm_status`](../data-types.md) | Source. The list of available source types can be obtained using [`crm.status.list`](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }` ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the source ||
|| **POST**
[`string`](../../data-types.md) | Position ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comment. Supports BB codes ||
|| **OPENED**
[`boolean`](../../data-types.md) | Is it available to everyone? Possible values:
- `Y` — yes
- `N` — no ||
|| **EXPORT**
[`boolean`](../../data-types.md) | Is the contact participating in the export? Possible values:
- `Y` — yes
- `N` — no ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user responsible for the contact ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the main company for the contact.

The list of companies can be obtained using the method [`crm.item.list`](../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **COMPANY_IDS**
[`crm_company[]`](../data-types.md) | Array of identifiers of companies to which the contact is linked.

The list of companies can be obtained using the method [`crm.item.list`](../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system (Google Ads, Microsoft Advertising, etc.) ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Traffic type. For example, `CPC` — ads, `CPM` — banners ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Advertising campaign designation ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Content of the campaign. For example, for contextual ads ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Search condition of the campaign. For example, keywords for contextual advertising ||
|| **PHONE**
[`crm_multifield[]`](../data-types.md#crm_multifield) | Phone ||
|| **EMAIL**
[`crm_multifield[]`](../data-types.md#crm_multifield) | E-mail ||
|| **WEB**
[`crm_multifield[]`](../data-types.md#crm_multifield) | Website ||
|| **IM**
[`crm_multifield[]`](../data-types.md#crm_multifield) | Messenger ||
|| **LINK**
[`crm_multifield[]`](../data-types.md#crm_multifield) | Links. System field ||
||**UF_...**
[`any`](../../data-types.md) | Custom fields. For example, `UF_CRM_25534736`.

Depending on the Bitrix24 settings, contacts may have a set of custom fields of defined types.

To change file fields, it is recommended to use the method [crm.item.update](../universal/crm-item-update.md).

A custom field can be added to a contact using the method [crm.contact.userfield.add](./userfield/crm-contact-userfield-add.md) ||
||**PARENT_ID_...**
[`crm_entity`](../data-types.md) | Relationship fields.

If a smart process is linked to contacts as a parent, the `PARENT_ID_...` field stores the identifier of its item. For example, `PARENT_ID_153` is the identifier of an item in the parent smart process with `entityTypeId=153` ||
|#

**Fields for external data sources**

These fields are populated if the contact was created by an external system.

#|
|| **Name**
`type` | **Description** ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the external system that is the source of data about this contact ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Identifier of the contact in the external system ||
|| **ORIGIN_VERSION**
[`string`](../../data-types.md) | Version of the contact data in the external system ||
|#

**Deprecated fields**

Address fields in the contact are deprecated and are only used in compatibility mode. To work with the address, use [requisites](../requisites/index.md).

#|
|| **Name**
`type` | **Description** ||
|| **ADDRESS**
[`string`](../../data-types.md) | Address ||
|| **ADDRESS_2**
[`string`](../../data-types.md) | Second line of the address ||
|| **ADDRESS_CITY**
[`string`](../../data-types.md) | City ||
|| **ADDRESS_POSTAL_CODE**
[`string`](../../data-types.md) | Postal code ||
|| **ADDRESS_REGION**
[`string`](../../data-types.md) | Region ||
|| **ADDRESS_PROVINCE**
[`string`](../../data-types.md) | Province ||
|| **ADDRESS_COUNTRY**
[`string`](../../data-types.md) | Country ||
|| **ADDRESS_COUNTRY_CODE**
[`string`](../../data-types.md) | Country code ||
|| **ADDRESS_LOC_ADDR_ID**
[`integer`](../../data-types.md) | Location address identifier ||
|#

### Parameter params {#parameter-params}

#|
|| **Name**
`type` | **Description** ||
|| **REGISTER_SONET_EVENT**
[`boolean`](../../data-types.md) | The parameter is accepted for compatibility and does not affect contact updates: the method ignores it ||
|| **REGISTER_HISTORY_EVENT**
[`boolean`](../../data-types.md) | Whether to record the contact update in the history. Possible values:
- `Y` — yes
- `N` — no

Default is `Y` ||
|#

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

Update contact with `id = 43`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":43,"FIELDS":{"NAME":"John","BIRTHDATE":"11.11.1999","TYPE_ID":"RECOMMENDATION","SOURCE_ID":"WEB","POST":"Network Administrator","COMMENTS":"New comment","OPENED":"N","EXPORT":"Y","ASSIGNED_BY_ID":1,"COMPANY_ID":13,"COMPANY_IDS":[13,15],"UF_CRM_1720697698689":"Example of a new value for a custom field of type \"String\"","PARENT_ID_1224":14},"PARAMS":{"REGISTER_SONET_EVENT":"N","REGISTER_HISTORY_EVENT":"N"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":43,"FIELDS":{"NAME":"John","BIRTHDATE":"11.11.1999","TYPE_ID":"RECOMMENDATION","SOURCE_ID":"WEB","POST":"Network Administrator","COMMENTS":"New comment","OPENED":"N","EXPORT":"Y","ASSIGNED_BY_ID":1,"COMPANY_ID":13,"COMPANY_IDS":[13,15],"UF_CRM_1720697698689":"Example of a new value for a custom field of type \"String\"","PARENT_ID_1224":14},"PARAMS":{"REGISTER_SONET_EVENT":"N","REGISTER_HISTORY_EVENT":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.contact.update',
        params: {
          id: 43,
          fields: {
            NAME: 'Sergey',
            BIRTHDATE: '11.11.1999',
            TYPE_ID: 'RECOMMENDATION',
            SOURCE_ID: 'WEB',
            POST: 'Computer network administrator',
            COMMENTS: 'New comment',
            OPENED: 'N',
            EXPORT: 'Y',
            ASSIGNED_BY_ID: 1,
            COMPANY_ID: 13,
            COMPANY_IDS: [13, 15],
            UF_CRM_1720697698689: 'Example new value of a custom field of type "String"',
            PARENT_ID_1224: 14,
          },
          params: {
            REGISTER_SONET_EVENT: 'N',
            REGISTER_HISTORY_EVENT: 'N',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Contact updated:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function updateContact() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.contact.update',
            params: {
              id: 43,
              fields: {
                NAME: 'Sergey',
                BIRTHDATE: '11.11.1999',
                TYPE_ID: 'RECOMMENDATION',
                SOURCE_ID: 'WEB',
                POST: 'Computer network administrator',
                COMMENTS: 'New comment',
                OPENED: 'N',
                EXPORT: 'Y',
                ASSIGNED_BY_ID: 1,
                COMPANY_ID: 13,
                COMPANY_IDS: [13, 15],
                UF_CRM_1720697698689: 'Example new value of a custom field of type "String"',
                PARENT_ID_1224: 14,
              },
              params: {
                REGISTER_SONET_EVENT: 'N',
                REGISTER_HISTORY_EVENT: 'N',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Contact updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateContact)
    </script>
    ```

- PHP

    ```php        
    try {
        $contactId = 43; // Example contact ID
        $fields = [
            'NAME' => 'John',
            'LAST_NAME' => 'Doe',
            'BIRTHDATE' => (new DateTime('1990-01-01'))->format(DateTime::ATOM),
            'PHONE' => [['VALUE' => '123456789', 'VALUE_TYPE' => 'WORK']],
            'EMAIL' => [['VALUE' => 'john.doe@example.com', 'VALUE_TYPE' => 'WORK']],
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
            var_export($result->getCoreResponse()->getResponseData()->getResult());
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
                COMPANY_ID: 13,
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
                'COMPANY_ID' => 13,
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.contact.update", b24.Params{
    	"id": 43,
    	"fields": b24.Params{
    		"NAME":                 "John",
    		"BIRTHDATE":            "11.11.1999",
    		"TYPE_ID":              "RECOMMENDATION",
    		"SOURCE_ID":            "WEB",
    		"POST":                 "Network Administrator",
    		"COMMENTS":             "New comment",
    		"OPENED":               "N",
    		"EXPORT":               "Y",
    		"ASSIGNED_BY_ID":       1,
		"COMPANY_ID":           13,
    		"COMPANY_IDS":          []int{13, 15},
    		"UF_CRM_1720697698689": "Example of a new value for a custom field of type \"String\"",
    		"PARENT_ID_1224":       14,
    	},
    	"params": b24.Params{
    		"REGISTER_SONET_EVENT":   "N",
    		"REGISTER_HISTORY_EVENT": "N",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.contact.update: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Working with Multiple Fields

### Update a Multiple Field

To overwrite an existing value of a multiple field, pass the `ID` of the value you want to change and the new `VALUE` and `VALUE_TYPE`. If `VALUE_TYPE` is omitted, the value type is reset to `WORK`.

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

To delete a single value from a multiple field, pass its identifier and the parameter `DELETE = 'Y'`, an empty `VALUE`, or omit `VALUE`.

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

Let’s consider ways to delete all values except the phone with `ID = 225`:

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

To add a value, pass it without an `ID`. Existing values will not change.

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

## Response Handling

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
[`boolean`](../../data-types.md) | Root element of the response, `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `ID is not defined or invalid.` | The `id` parameter is missing or its value is no greater than 0 after conversion to an integer ||
|| `400` | Empty value | `Parameter 'fields' must be array.` | The `fields` parameter is not an object ||
|| `400` | Empty value | `Parameter 'params' must be array.` | The `params` parameter is not an object ||
|| `400` | Empty value | `Contact is not found` | The contact was not found ||
|| `400` | Empty value | `Access denied.` | The user does not have permission to "Edit" contacts ||
|| `400` | Empty value | Disk resource exhausted | Insufficient Drive space to retain the file ||
|| `400` | Empty value | The "First Name" and "Last Name" fields cannot both be empty. Please fill in at least one of them | After the update, the contact has neither `NAME` nor `LAST_NAME`. For example, an empty `NAME` was passed and the last name is not filled in ||
|| `400` | `ERROR_CORE` | The field "Work e-mail" contains an invalid address.<br> | An invalid address was passed in `EMAIL`. The field name in the text depends on `VALUE_TYPE` ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-add.md)
- [{#T}](./crm-contact-get.md)
- [{#T}](./crm-contact-list.md)
- [{#T}](./crm-contact-delete.md)
- [{#T}](./crm-contact-fields.md)
- [{#T}](../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)
