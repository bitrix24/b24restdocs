# Merge Duplicates crm.entity.mergeBatch

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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.entity.mergeBatch',
    		{
    			params: {
    				entityTypeId: 3,
    				entityIds: [100, 101, 102]
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error(error);
    }
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