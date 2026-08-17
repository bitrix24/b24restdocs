# Create a New Contact crm.contact.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "add|import" access permission for contacts

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.add](../universal/crm-item-add.md).

{% endnote %}

The method `crm.contact.add` creates a new contact.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **fields**
[`object`](../../data-types.md) | An object in the following format:

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
[`object`](../../data-types.md) | An object containing a set of additional parameters.

The structure and possible values are described [below](#parameter-params) ||
|#

### Parameter fields {#parameter-fields}

#|
|| **Name**
`type` | **Description** ||
|| **HONORIFIC**
[`crm_status`](../data-types.md) | Salutation.

The list of available salutation types can be obtained using the method [`crm.status.list`](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "HONORIFIC" }`.

Default — the first available salutation type ||
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

The list of available contact types can be obtained using the method [`crm.status.list`](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "CONTACT_TYPE" }`.

Default — the first available contact type ||
|| **SOURCE_ID**
[`crm_status`](../data-types.md) | Source.

The list of available source types can be obtained using the method [`crm.status.list`](../status/crm-status-list.md) with the filter `{ ENTITY_ID: "SOURCE" }`.

Default — the first available source type ||
|| **SOURCE_DESCRIPTION**
[`string`](../../data-types.md) | Additional information about the source ||
|| **POST**
[`string`](../../data-types.md) | Position ||
|| **COMMENTS**
[`string`](../../data-types.md) | Comment. Supports BB codes ||
|| **OPENED**
[`boolean`](../../data-types.md) | Is it available to everyone? Possible values:
- `Y` — yes
- `N` — no

Default is `Y`. The default value can be changed in the CRM settings ||
|| **EXPORT**
[`boolean`](../../data-types.md) | Is the contact included in the export? Possible values:
- `Y` — yes
- `N` — no

Default is `Y` ||
|| **ASSIGNED_BY_ID**
[`user`](../../data-types.md) | Identifier of the user responsible for the item.

Default — the identifier of the user calling the method ||
|| **COMPANY_ID**
[`crm_company`](../data-types.md) | Identifier of the main company for the contact.

The list of companies can be obtained using the method [`crm.item.list`](../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **COMPANY_IDS**
[`crm_company[]`](../data-types.md) | Array of identifiers of companies associated with the contact.

The list of companies can be obtained using the method [`crm.item.list`](../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **UTM_SOURCE**
[`string`](../../data-types.md) | Advertising system (Google Ads, Microsoft Advertising, etc.) ||
|| **UTM_MEDIUM**
[`string`](../../data-types.md) | Traffic type. Possible values:
- `CPC` — ads
- `CPM` — banners ||
|| **UTM_CAMPAIGN**
[`string`](../../data-types.md) | Advertising campaign designation ||
|| **UTM_CONTENT**
[`string`](../../data-types.md) | Campaign content. For example, for contextual ads ||
|| **UTM_TERM**
[`string`](../../data-types.md) | Campaign search condition. For example, keywords for contextual advertising ||
|| **TRACE**
[`string`](../../data-types.md) | Information for [Sales Intelligence](../../../tutorials/crm/how-to-use-analitycs/use-analitics-for-add-contact.md) ||
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

Depending on the portal settings, contacts may have a set of custom fields of defined types. 

A custom field can be added to a contact using the method [crm.contact.userfield.add](./userfield/crm-contact-userfield-add.md) ||
||**PARENT_ID_...** | Relationship fields. 

If there are smart processes associated with contacts on the portal, there is a field for each such smart process that stores the relationship between that smart process and the contact. The field itself stores the identifier of the element of that smart process. 

For example, the field `PARENT_ID_153` — relationship with the smart process `entityTypeId=153`. It stores the identifier of the element of that smart process associated with the current contact ||
|#

**Fields for connections with external data sources**

If the contact is created by an external system, then:
- the field `ORIGINATOR_ID` stores the string identifier of that system
- the field `ORIGIN_ID` stores the string identifier of the contact in that external system
- the field `ORIGIN_VERSION` stores the version of the contact data in that external system

#|
|| **Name**
`type` | **Description** ||
|| **ORIGINATOR_ID**
[`string`](../../data-types.md) | Identifier of the external system that is the source of data about this contact ||
|| **ORIGIN_ID**
[`string`](../../data-types.md) | Version of the contact data in the external system. Used to protect data from accidental overwriting by the external system. 

If the data was imported and not changed in the external system, such data can be edited in CRM without fear that the next export will lead to data overwriting ||
|| **ORIGIN_VERSION**
[`string`](../../data-types.md) | Original version ||
|#

**Import**

The fields are available for filling when the parameter `IMPORT = 'Y'` is passed in the `params` parameter.

#|
|| **Name**
`type` | **Description** ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Creation date.

Available when `IMPORT = Y` is passed in `params`.

Cannot be earlier than the creation date of the last created contact
||
|| **DATE_MODIFY**
[`datetime`](../../data-types.md) | Modification date.

Available when `IMPORT = Y` is passed in `params` ||
|| **CREATED_BY_ID**
[`user`](../../data-types.md) | Created by.

Available when `IMPORT = Y` is passed in `params` ||
|| **MODIFY_BY_ID**
[`user`](../../data-types.md) | Modified by.
Available when `IMPORT = Y` is passed in `params` ||
|#

**Deprecated fields**

Address fields in the contact are deprecated and are only used for compatibility mode. To work with the address, use [requisites](../requisites/index.md).

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
[`boolean`](../../data-types.md) | Should the event of adding a contact be registered in the live feed? Possible values:
- `Y` — yes
- `N` — no

Default is `N` ||
|| **IMPORT**
[`boolean`](../../data-types.md) | Is import mode enabled? Possible values:
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
    -d '{"FIELDS":{"HONORIFIC":"HNR_RU_1","NAME":"John","SECOND_NAME":"Doe","LAST_NAME":"Smith","PHOTO":{"fileData":"**put_photo_data_here**"},"BIRTHDATE":"11.11.2001","TYPE_ID":"PARTNER","SOURCE_ID":"WEB","SOURCE_DESCRIPTION":"*Additional information about the source*","POST":"Administrator","COMMENTS":"**put_comment_here**","OPENED":"Y","EXPORT":"N","ASSIGNED_BY_ID":6,"COMPANY_ID":12,"COMPANY_IDS":[12,13,15],"UTM_SOURCE":"google","UTM_MEDIUM":"CPC","UTM_CAMPAIGN":"summer_sale","UTM_CONTENT":"header_banner","UTM_TERM":"discount","PHONE":[{"VALUE":"+1333333555","VALUE_TYPE":"WORK"},{"VALUE":"+15599888666","VALUE_TYPE":"HOME"}],"EMAIL":[{"VALUE":"john.smith@example.mailing","VALUE_TYPE":"MAILING"},{"VALUE":"john.smith@example.work","VALUE_TYPE":"WORK"}],"UF_CRM_1720697698689":"Example value of a custom field with type \"String\"","PARENT_ID_1224":12}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FIELDS":{"HONORIFIC":"HNR_RU_1","NAME":"John","SECOND_NAME":"Doe","LAST_NAME":"Smith","PHOTO":{"fileData":"**put_photo_data_here**"},"BIRTHDATE":"11.11.2001","TYPE_ID":"PARTNER","SOURCE_ID":"WEB","SOURCE_DESCRIPTION":"*Additional information about the source*","POST":"Administrator","COMMENTS":"**put_comment_here**","OPENED":"Y","EXPORT":"N","ASSIGNED_BY_ID":6,"COMPANY_ID":12,"COMPANY_IDS":[12,13,15],"UTM_SOURCE":"google","UTM_MEDIUM":"CPC","UTM_CAMPAIGN":"summer_sale","UTM_CONTENT":"header_banner","UTM_TERM":"discount","PHONE":[{"VALUE":"+1333333555","VALUE_TYPE":"WORK"},{"VALUE":"+15599888666","VALUE_TYPE":"HOME"}],"EMAIL":[{"VALUE":"john.smith@example.mailing","VALUE_TYPE":"MAILING"},{"VALUE":"john.smith@example.work","VALUE_TYPE":"WORK"}],"UF_CRM_1720697698689":"Example value of a custom field with type \"String\"","PARENT_ID_1224":12},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'crm.contact.add',
        params: {
          fields: {
            HONORIFIC: 'HNR_RU_1',
            NAME: 'Ivan',
            SECOND_NAME: 'Ivanovich',
            LAST_NAME: 'Ivanov',
            PHOTO: {
              fileData: document.getElementById('photo'),
            },
            BIRTHDATE: '11.11.2001',
            TYPE_ID: 'PARTNER',
            SOURCE_ID: 'WEB',
            SOURCE_DESCRIPTION: '*Additional source details*',
            POST: 'Administrator',
            COMMENTS: `
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
            `,
            OPENED: 'Y',
            EXPORT: 'N',
            ASSIGNED_BY_ID: 6,
            COMPANY_ID: 12,
            COMPANY_IDS: [12, 13, 15],
            UTM_SOURCE: 'yandex',
            UTM_MEDIUM: 'CPC',
            UTM_CAMPAIGN: 'summer_sale',
            UTM_CONTENT: 'header_banner',
            UTM_TERM: 'discount',
            PHONE: [
              {
                VALUE: '+7333333555',
                VALUE_TYPE: 'WORK',
              },
              {
                VALUE: '+35599888666',
                VALUE_TYPE: 'HOME',
              },
            ],
            EMAIL: [
              {
                VALUE: 'ivanov@example.mailing',
                VALUE_TYPE: 'MAILING',
              },
              {
                VALUE: 'ivanov@example.work',
                VALUE_TYPE: 'WORK',
              },
            ],
            UF_CRM_1720697698689: 'Example value of a custom field of type "String"',
            PARENT_ID_1224: 12,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created contact id:', result)
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
      async function addContact() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.contact.add',
            params: {
              fields: {
                HONORIFIC: 'HNR_RU_1',
                NAME: 'Ivan',
                SECOND_NAME: 'Ivanovich',
                LAST_NAME: 'Ivanov',
                PHOTO: {
                  fileData: document.getElementById('photo'),
                },
                BIRTHDATE: '11.11.2001',
                TYPE_ID: 'PARTNER',
                SOURCE_ID: 'WEB',
                SOURCE_DESCRIPTION: '*Additional source details*',
                POST: 'Administrator',
                COMMENTS: `
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
                `,
                OPENED: 'Y',
                EXPORT: 'N',
                ASSIGNED_BY_ID: 6,
                COMPANY_ID: 12,
                COMPANY_IDS: [12, 13, 15],
                UTM_SOURCE: 'yandex',
                UTM_MEDIUM: 'CPC',
                UTM_CAMPAIGN: 'summer_sale',
                UTM_CONTENT: 'header_banner',
                UTM_TERM: 'discount',
                PHONE: [
                  {
                    VALUE: '+7333333555',
                    VALUE_TYPE: 'WORK',
                  },
                  {
                    VALUE: '+35599888666',
                    VALUE_TYPE: 'HOME',
                  },
                ],
                EMAIL: [
                  {
                    VALUE: 'ivanov@example.mailing',
                    VALUE_TYPE: 'MAILING',
                  },
                  {
                    VALUE: 'ivanov@example.work',
                    VALUE_TYPE: 'WORK',
                  },
                ],
                UF_CRM_1720697698689: 'Example value of a custom field of type "String"',
                PARENT_ID_1224: 12,
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
          console.info('Created contact id:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addContact)
    </script>
    ```

- PHP

    ```php        
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.contact.add',
                [
                    'fields' => [
                        'HONORIFIC' => 'HNR_RU_1',
                        'NAME' => 'John',
                        'SECOND_NAME' => 'Doe',
                        'LAST_NAME' => 'Smith',
                        'PHOTO' => [
                            'fileData' => document.getElementById('photo'),
                        ],
                        'BIRTHDATE' => '11.11.2001',
                        'TYPE_ID' => 'PARTNER',
                        'SOURCE_ID' => 'WEB',
                        'SOURCE_DESCRIPTION' => '*Additional information about the source*',
                        'POST' => 'Administrator',
                        'COMMENTS' => '
                            Example comment within the contact

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
                        ',
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
                                'VALUE' => 'john.smith@example.mailing',
                                'VALUE_TYPE' => 'MAILING',
                            ],
                            [
                                'VALUE' => 'john.smith@example.work',
                                'VALUE_TYPE' => 'WORK',
                            ]
                        ],
                        'UF_CRM_1720697698689' => 'Example value of a custom field with type "String"',
                        'PARENT_ID_1224' => 12,
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding contact: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
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
                COMMENTS: `
                    Example comment within the contact

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
                `,
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
                        VALUE: "john.smith@example.mailing",
                        VALUE_TYPE: "MAILING",
                    },
                    {
                        VALUE: "john.smith@example.work",
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.contact.add',
        [
            'fields' => [
                'HONORIFIC' => 'HNR_RU_1',
                'NAME' => 'John',
                'SECOND_NAME' => 'Doe',
                'LAST_NAME' => 'Smith',
                'PHOTO' => [
                    'fileData' => document.getElementById('photo'),
                ],
                'BIRTHDATE' => '11.11.2001',
                'TYPE_ID' => 'PARTNER',
                'SOURCE_ID' => 'WEB',
                'SOURCE_DESCRIPTION' => '*Additional information about the source*',
                'POST' => 'Administrator',
                'COMMENTS' => '
                    Example comment within the contact

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
                ',
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
                        'VALUE' => 'john.smith@example.mailing',
                        'VALUE_TYPE' => 'MAILING',
                    ],
                    [
                        'VALUE' => 'john.smith@example.work',
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.contact.add", b24.Params{
    	"fields": b24.Params{
    		"HONORIFIC":   "HNR_RU_1",
    		"NAME":        "John",
    		"SECOND_NAME": "Doe",
    		"LAST_NAME":   "Smith",
    		"PHOTO": b24.Params{
    			"fileData": "**put_photo_data_here**",
    		},
    		"BIRTHDATE":          "11.11.2001",
    		"TYPE_ID":            "PARTNER",
    		"SOURCE_ID":          "WEB",
    		"SOURCE_DESCRIPTION": "*Additional information about the source*",
    		"POST":               "Administrator",
    		"COMMENTS":           "**put_comment_here**",
    		"OPENED":             "Y",
    		"EXPORT":             "N",
    		"ASSIGNED_BY_ID":     6,
    		"COMPANY_ID":         12,
    		"COMPANY_IDS":        []int{12, 13, 15},
    		"UTM_SOURCE":         "yandex",
    		"UTM_MEDIUM":         "CPC",
    		"UTM_CAMPAIGN":       "summer_sale",
    		"UTM_CONTENT":        "header_banner",
    		"UTM_TERM":           "discount",
    		"PHONE": []b24.Params{
    			{
    				"VALUE":      "+49333333555",
    				"VALUE_TYPE": "WORK",
    			},
    			{
    				"VALUE":      "+35599888666",
    				"VALUE_TYPE": "HOME",
    			},
    		},
    		"EMAIL": []b24.Params{
    			{
    				"VALUE":      "ivanov@example.mailing",
    				"VALUE_TYPE": "MAILING",
    			},
    			{
    				"VALUE":      "ivanov@example.work",
    				"VALUE_TYPE": "WORK",
    			},
    		},
    		"UF_CRM_1720697698689": "Example value of a custom field with type \"String\"",
    		"PARENT_ID_1224":       12,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.contact.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
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
[`integer`](../../data-types.md) | The root element of the response, contains the identifier of the created contact ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| Empty value | `Parameter 'fields' must be array` | The parameter `fields` is not an object ||
|| Empty value | `Parameter 'params' must be array` | The parameter `params` is not an object ||
|| Empty value | `Access denied` | The user does not have permission to "Add" or "Import" contacts ||
|| Empty value | Disk resource exhausted | ||
|| `ERROR_CORE` | The field `Work e-mail` contains an invalid address | ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-update.md)
- [{#T}](./crm-contact-get.md)
- [{#T}](./crm-contact-list.md)
- [{#T}](./crm-contact-delete.md)
- [{#T}](./crm-contact-fields.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contact.md)
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contact-with-requisite.md)
- [{#T}](../../../tutorials/crm/how-to-edit-crm-objects/how-to-change-email-or-phone.md)
