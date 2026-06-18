# Merge Duplicates crm.entity.mergeBatch

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: administrator

The method `crm.entity.mergeBatch` merges multiple entities into one.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **params***
[`object`](../../data-types.md) | Object containing the entities to merge [(detailed description)](#params) ||
|#

### Parameter params{#params}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../data-types.md) | Identifier of the [CRM object type](../data-types.md#object_type). Possible values:
- `1` — [lead](../leads/index.md)
- `2` — [deal](../deals/index.md)
- `3` — [contact](../contacts/index.md)
- `4` — [company](../companies/index.md)
- `7` — [estimate](../quote/index.md)
- `31` — [invoice](../universal/invoice.md)
- `128` — [SPA](../universal/index.md). The identifier of a specific SPA can be found using the methods [crm.enum.ownertype](../auxiliary/enum/crm-enum-owner-type.md) and [crm.type.list](../universal/user-defined-object-types/crm-type-list.md) ||
|| **entityIds***
[`integer[]`](../../data-types.md) | Array of identifiers of the entities to be merged. At least two entities are required ||
|#

### Method Operation Features

You can only merge entities of the same type: lead with lead, contact with contact, SPA element with ID 128 with another SPA element with ID 128.

Full automatic merging is available in several cases: 
- the entities are identical,
- the entities are not identical, but the differences in field values do not require manual processing. For example, if one entity has a field filled and the other has the same field empty — the value from the filled field will be retained.

The main entity in the merge will be the one whose `ID` is specified first in the `entityIds` array. Information from other entities will be transferred to the main entity. All entities except the main one will be deleted after a successful merge.

#### Manual Merging in Case of Conflict

If the method returns a status of `CONFLICT`, you can continue the merge manually in the Bitrix24 interface via the link:

- Contacts: `/crm/contact/merge/?id=1,2,3`
- Companies: `/crm/company/merge/?id=1,2,3`
- Leads: `/crm/lead/merge/?id=1,2,3`
- Deals: `/crm/deal/merge/?id=1,2,3`
  
The `id` parameter contains the identifiers of the entities to be merged, separated by commas.  

- Invoices: `/crm/type/31/merge/?id[]=1&id[]=2`
- Estimates: `/crm/type/7/merge/?id[]=1&id[]=2`
- SPAs: `/crm/type/128/merge/?id[]=1&id[]=2`

The `id[]` parameter contains the identifiers of the entities to be merged, passed as a repeating parameter.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"params":{"entityTypeId":3,"entityIds":[100,101,102]}}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.entity.mergeBatch
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**","params":{"entityTypeId":3,"entityIds":[100,101,102]}}' \
         https://**put_your_bitrix24_address**/rest/crm.entity.mergeBatch
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MergeBatchResult = {
      STATUS: 'SUCCESS' | 'CONFLICT' | 'ERROR'
      ENTITY_IDS: number[]
    }

    try {
      const response = await $b24.actions.v2.call.make<MergeBatchResult>({
        method: 'crm.entity.mergeBatch',
        params: {
          params: {
            entityTypeId: 3,
            entityIds: [100, 101, 102],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Merge status:', result.STATUS, '| Deleted entity IDs:', result.ENTITY_IDS)
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
      async function mergeEntities() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.entity.mergeBatch',
            params: {
              params: {
                entityTypeId: 3,
                entityIds: [100, 101, 102],
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
          console.info('Merge status:', result.STATUS, '| Deleted entity IDs:', result.ENTITY_IDS)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', mergeEntities)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.entity.mergeBatch',
                [
                    'params' => [
                        'entityTypeId' => 3,
                        'entityIds'    => [100, 101, 102]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error merging entities: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.entity.mergeBatch',
        {
            params: {
                entityTypeId: 3,
                entityIds: [100, 101, 102]
            }
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.entity.mergeBatch',
        [
            'params' => [
                'entityTypeId' => 3,
                'entityIds' => [100, 101, 102]
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
    "result": {
        "STATUS": "SUCCESS",
        "ENTITY_IDS": [101, 102]
    },
    "time": {
        "start": 1750754639.300838,
        "finish": 1750754641.350269,
        "duration": 2.049431085586548,
        "processing": 2.0271031856536865,
        "date_start": "2025-06-24T11:43:59+02:00",
        "date_finish": "2025-06-24T11:44:01+02:00",
        "operating_reset_at": 1750755239,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **STATUS**
[`string`](../../data-types.md) | Status of the operation. Possible values:
- `SUCCESS` — the merge was successful
- `CONFLICT` — a data conflict occurred, automatic merging is not possible
- `ERROR` — an [error](#errors) occurred ||
|| **ENTITY_IDS**
[`integer[]`](../../data-types.md) | Array of identifiers of the entities that were deleted during the merge ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400** 

```json
{
    "error": 0,
    "error_description": "The parameter entityIds must contain at least two elements."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes {#errors}

#|
|| **Code** | **Description** | **Value** ||
|| `403` | `Access denied` | The user does not have permission to modify or delete CRM entities ||
|| `400` | `The parameter entityTypeId is required.` | The required parameter `entityTypeId` is missing ||
|| `400` | `The parameter entityIds does not contain valid elements.` | No elements were provided or found for merging ||
|| `400` | `The parameter entityIds must contain at least two elements.` | At least two elements are required for merging ||
|| `400` | `Owner was not found` | The object was not found ||
|| `400` | `Entity type {entityTypeName} is not supported` | An unsupported object type was specified ||
|| `400` | `CRM_FEATURE_RESTRICTION_ERROR` | Plan restriction ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md)