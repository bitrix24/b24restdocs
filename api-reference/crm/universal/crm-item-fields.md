# Retrieve Fields of CRM Item

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "read" access permission for CRM object elements.

This method retrieves a list of fields and their configurations for elements of type `entityTypeId`.

{% note warning %}

Elements belonging to different types of CRM objects will have different sets of fields.

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](../data-types.md#object_type) or [custom type](./user-defined-object-types/index.md) whose fields we want to retrieve.

Numeric values for system types (Lead — 1, Deal — 2, Contact — 3, Company — 4, Invoice — 31, etc.) are listed in the [CRM object types reference](../data-types.md#object_type). The identifier for the SPA can be obtained using the [crm.type.list](./user-defined-object-types/crm-type-list.md) method. ||
|| **useOriginalUfNames**
[`boolean`][1] | This parameter controls the format of custom field names in the response.   
Possible values:

- `Y` — original names of custom fields, e.g., `UF_CRM_2_1639669411830`
- `N` — custom field names in camelCase, e.g., `ufCrm2_1639669411830`

Default is `N` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Retrieve the list of fields for SPA elements with `entityTypeId = 1268`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1268,"useOriginalUfNames":"N"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1268,"useOriginalUfNames":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldDescription = {
      type: string
      isRequired: boolean
      isReadOnly: boolean
      isImmutable: boolean
      isMultiple: boolean
      isDynamic: boolean
      title: string
      upperName: string
      settings?: Record<string, unknown>
      statusType?: string
      isDeprecated?: boolean
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CrmItemFieldsResult = {
      fields: Record<string, FieldDescription>
    }

    try {
      const response = await $b24.actions.v2.call.make<CrmItemFieldsResult>({
        method: 'crm.item.fields',
        params: {
          entityTypeId: 1268,
          useOriginalUfNames: 'N',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field count:', Object.keys(result.fields).length)
        console.info('Fields:', result.fields)
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
      async function fetchCrmItemFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.fields',
            params: {
              entityTypeId: 1268,
              useOriginalUfNames: 'N',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field count:', Object.keys(result.fields).length)
          console.info('Fields:', result.fields)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchCrmItemFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.fields',
                [
                    'entityTypeId'      => 1268,
                    'useOriginalUfNames' => 'N',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            return;
        }
    
        echo 'Success: ' . print_r($result->data(), true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching CRM item fields: ' . $e->getMessage();
    }
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.item.fields(
            entity_type_id=1268,
            use_original_uf_names=False,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- BX24.js

    ```js
        BX24.callMethod(
            'crm.item.fields',
            {
                entityTypeId: 1268,
                useOriginalUfNames: 'N',
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
        'crm.item.fields',
        [
            'entityTypeId' => 1268,
            'useOriginalUfNames' => 'N',
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
    "result": {
        "fields": {
            "id": {
                "type": "integer",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "ID",
                "upperName": "ID"
            },
            "title": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Name",
                "upperName": "TITLE"
            },
            "xmlId": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "External code",
                "upperName": "XML_ID"
            },
            "createdTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Created at",
                "upperName": "CREATED_TIME"
            },
            "updatedTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Updated at",
                "upperName": "UPDATED_TIME"
            },
            "createdBy": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Created by",
                "upperName": "CREATED_BY"
            },
            "updatedBy": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Updated by",
                "upperName": "UPDATED_BY"
            },
            "assignedById": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Assigned to",
                "upperName": "ASSIGNED_BY_ID"
            },
            "opened": {
                "type": "boolean",
                "isRequired": true,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Publicly available",
                "upperName": "OPENED"
            },
            "webformId": {
                "type": "integer",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Created by CRM form",
                "upperName": "WEBFORM_ID"
            },
            "begindate": {
                "type": "date",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Start date",
                "upperName": "BEGINDATE"
            },
            "closedate": {
                "type": "date",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "End date",
                "upperName": "CLOSEDATE"
            },
            "companyId": {
                "type": "crm_company",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Company",
                "settings": {
                    "parentEntityTypeId": 4
                },
                "upperName": "COMPANY_ID"
            },
            "contactId": {
                "type": "crm_contact",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "isDeprecated": true,
                "title": "Contact",
                "upperName": "CONTACT_ID"
            },
            "contactIds": {
                "type": "crm_contact",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": true,
                "isDynamic": false,
                "title": "CONTACT_IDS",
                "upperName": "CONTACT_IDS"
            },
            "contacts": {
                "type": "crm_contact",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": true,
                "isDynamic": false,
                "title": "CONTACTS",
                "upperName": "CONTACTS"
            },
            "observers": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": true,
                "isDynamic": false,
                "title": "Observers",
                "upperName": "OBSERVERS"
            },
            "categoryId": {
                "type": "crm_category",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Pipeline",
                "upperName": "CATEGORY_ID"
            },
            "movedTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Moved at",
                "upperName": "MOVED_TIME"
            },
            "movedBy": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Moved by",
                "upperName": "MOVED_BY"
            },
            "stageId": {
                "type": "crm_status",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "statusType": "DYNAMIC_1268_STAGE_52",
                "title": "Stage",
                "upperName": "STAGE_ID"
            },
            "previousStageId": {
                "type": "crm_status",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "statusType": "DYNAMIC_1268_STAGE_52",
                "title": "Previous stage",
                "upperName": "PREVIOUS_STAGE_ID"
            },
            "sourceId": {
                "type": "crm_status",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "statusType": "SOURCE",
                "title": "Source",
                "upperName": "SOURCE_ID"
            },
            "sourceDescription": {
                "type": "text",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Additional source info",
                "upperName": "SOURCE_DESCRIPTION"
            },
            "currencyId": {
                "type": "crm_currency",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Currency",
                "upperName": "CURRENCY_ID"
            },
            "isManualOpportunity": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Amount calculation mode",
                "upperName": "IS_MANUAL_OPPORTUNITY"
            },
            "opportunity": {
                "type": "double",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Amount",
                "upperName": "OPPORTUNITY"
            },
            "taxValue": {
                "type": "double",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Tax amount",
                "upperName": "TAX_VALUE"
            },
            "mycompanyId": {
                "type": "crm_company",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Your company details",
                "settings": {
                    "isMyCompany": true,
                    "parentEntityTypeId": 4,
                    "isEmbeddedEditorEnabled": true
                },
                "upperName": "MYCOMPANY_ID"
            },
            "lastActivityBy": {
                "type": "user",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Last activity in timeline by",
                "upperName": "LAST_ACTIVITY_BY"
            },
            "lastActivityTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": true,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Last activity",
                "upperName": "LAST_ACTIVITY_TIME"
            },
            "parentId1": {
                "type": "crm_entity",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Lead",
                "settings": {
                    "parentEntityTypeId": 1
                },
                "upperName": "PARENT_ID_1"
            },
            "parentId2": {
                "type": "crm_entity",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Deal",
                "settings": {
                    "parentEntityTypeId": 2
                },
                "upperName": "PARENT_ID_2"
            },
            "parentId1248": {
                "type": "crm_entity",
                "isRequired": false,
                "isReadOnly": false,
                "isImmutable": false,
                "isMultiple": false,
                "isDynamic": false,
                "title": "Smart Process #16",
                "settings": {
                    "parentEntityTypeId": 1248
                },
                "upperName": "PARENT_ID_1248"
            }
        }
    },
    "time": {
        "start": 1721038185.626335,
        "finish": 1721038186.072804,
        "duration": 0.4464690685272217,
        "processing": 0.17344903945922852,
        "date_start": "2024-07-15T12:09:45+02:00",
        "date_finish": "2024-07-15T12:09:46+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | The root element of the response. Contains a single key `fields` ||
|| **fields**
[`object`][1] | An object in the format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...
    field_n: value_n,
}
```

where: 
- `field_n` — field of the element
- `value_n` — information about the field in the format [`crm_rest_field_description`](../data-types.md#crm_rest_field_description)

||
|| **time**
[`time`][1]   | Information about the request execution time ||
|#

{% note info " " %}

By default, custom field names are returned in camelCase, e.g., `ufCrm2_1639669411830`.
When passing the parameter `useOriginalUfNames` with the value `Y`, custom fields will be returned with their original names, for example `UF_CRM_2_1639669411830`.

{% endnote %}

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "NOT_FOUND", 
    "error_description": "Smart process not found"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}


### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403`      | `allowed_only_intranet_user` | Action is allowed only to intranet users | User is not an intranet user                 ||
|| `400`      | `NOT_FOUND` | SPA not found                          | Occurs when an invalid `entityTypeId` is passed              ||
|| `400`      | `ACCESS_DENIED` | You do not have permission to view this item        | User does not have read access permission for elements of type `entityTypeId` ||
|#

{% include [System errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](crm-item-add.md)
- [{#T}](crm-item-update.md)
- [{#T}](crm-item-get.md)
- [{#T}](crm-item-list.md)
- [{#T}](crm-item-delete.md)
- [{#T}](./object-fields.md)

[1]: ../../data-types.md