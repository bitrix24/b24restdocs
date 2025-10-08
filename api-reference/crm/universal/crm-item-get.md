# Get Item by Id crm.item.get

> Scope: [`crm`](../../scopes/permissions.md)
> 
> Who can execute the method: any user with "read" access permission for CRM object elements

The method returns information about an item by its identifier and the identifier of the CRM object type.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](./index.md) or [user-defined type](./user-defined-object-types/index.md) whose item we want to retrieve ||
|| **id***
[`integer`][1] | Identifier of the item whose information we want to obtain.

It can be retrieved using the [`crm.item.list`](./crm-item-list.md) method or when creating an item with [`crm.item.add`](./crm-item-add.md) ||
|| **useOriginalUfNames**
[`boolean`][1] | This parameter is used to control the format of custom field names in the response.   
Possible values:

- `Y` — original custom field names, e.g., `UF_CRM_2_1639669411830`
- `N` — custom field names in camelCase, e.g., `ufCrm2_1639669411830`

Default is `N` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Get information about a lead with `id = 250`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"id":250,"useOriginalUfNames":"N"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"id":250,"useOriginalUfNames":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.item.get',
    		{
    			entityTypeId: 1,
    			id: 250,
    			useOriginalUfNames: 'N',
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php        
    try {
        $entityTypeId = 1; // Example entity type ID
        $id = 123; // Example item ID
        $itemResult = $serviceBuilder
            ->getCRMScope()
            ->item()
            ->get($entityTypeId, $id);
        $item = $itemResult->item();
        print("ID: " . $item->id . PHP_EOL);
        print("XML ID: " . $item->xmlId . PHP_EOL);
        print("Title: " . $item->title . PHP_EOL);
        print("Created By: " . $item->createdBy . PHP_EOL);
        print("Updated By: " . $item->updatedBy . PHP_EOL);
        print("Moved By: " . $item->movedBy . PHP_EOL);
        print("Created Time: " . $item->createdTime->format(DATE_ATOM) . PHP_EOL);
        print("Updated Time: " . $item->updatedTime->format(DATE_ATOM) . PHP_EOL);
        print("Moved Time: " . $item->movedTime->format(DATE_ATOM) . PHP_EOL);
        print("Category ID: " . $item->categoryId . PHP_EOL);
        print("Opened: " . ($item->opened ? 'true' : 'false') . PHP_EOL);
        print("Previous Stage ID: " . $item->previousStageId . PHP_EOL);
        print("Begin Date: " . $item->begindate->format(DATE_ATOM) . PHP_EOL);
        print("Close Date: " . $item->closedate->format(DATE_ATOM) . PHP_EOL);
        print("Company ID: " . $item->companyId . PHP_EOL);
        print("Contact ID: " . $item->contactId . PHP_EOL);
        print("Opportunity: " . $item->opportunity . PHP_EOL);
        print("Is Manual Opportunity: " . ($item->isManualOpportunity ? 'true' : 'false') . PHP_EOL);
        print("Tax Value: " . $item->taxValue . PHP_EOL);
        print("Currency ID: " . $item->currencyId . PHP_EOL);
        print("Opportunity Account: " . $item->opportunityAccount . PHP_EOL);
        print("Tax Value Account: " . $item->taxValueAccount . PHP_EOL);
        print("Account Currency ID: " . $item->accountCurrencyId . PHP_EOL);
        print("My Company ID: " . $item->mycompanyId . PHP_EOL);
        print("Source ID: " . $item->sourceId . PHP_EOL);
        print("Source Description: " . $item->sourceDescription . PHP_EOL);
        print("Webform ID: " . $item->webformId . PHP_EOL);
        print("Assigned By ID: " . $item->assignedById . PHP_EOL);
        print("Last Activity By: " . $item->lastActivityBy . PHP_EOL);
        print("Last Activity Time: " . $item->lastActivityTime->format(DATE_ATOM) . PHP_EOL);
        print("UTM Source: " . $item->utmSource . PHP_EOL);
        print("UTM Medium: " . $item->utmMedium . PHP_EOL);
        print("UTM Campaign: " . $item->utmCampaign . PHP_EOL);
        print("UTM Content: " . $item->utmContent . PHP_EOL);
        print("UTM Term: " . $item->utmTerm . PHP_EOL);
        print("Observers: " . json_encode($item->observers) . PHP_EOL);
        print("Contact IDs: " . json_encode($item->contactIds) . PHP_EOL);
        print("Entity Type ID: " . $item->entityTypeId . PHP_EOL);
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage() . PHP_EOL);
    }
    ```

- BX24.js

    ```js
        BX24.callMethod(
            'crm.item.get',
            {
                entityTypeId: 1,
                id: 250,
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
        'crm.item.get',
        [
            'entityTypeId' => 1,
            'id' => 250,
            'useOriginalUfNames' => 'N',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "item": {
            "id": 250,
            "createdTime": "2024-07-22T18:00:08+02:00",
            "dateCreateShort": null,
            "updatedTime": "2024-07-22T18:00:08+02:00",
            "dateModifyShort": null,
            "createdBy": 1,
            "updatedBy": 1,
            "assignedById": 1,
            "opened": "Y",
            "companyId": 0,
            "contactId": 0,
            "stageId": "IN_PROCESS",
            "isConvert": null,
            "statusDescription": null,
            "stageSemanticId": "P",
            "productId": null,
            "opportunity": 999.9,
            "currencyId": "USD",
            "sourceId": "TRADE_SHOW",
            "sourceDescription": "Admin Exhibition",
            "title": "Lead #250",
            "name": "Admin",
            "lastName": "Adminov",
            "secondName": "Adminovich",
            "shortName": null,
            "companyTitle": "Administrative Company",
            "post": "Admin",
            "address": null,
            "comments": "[B]Comment about admin[/B]",
            "webformId": 0,
            "originatorId": null,
            "originId": null,
            "dateClosed": null,
            "birthdate": "2000-01-01T02:00:00+02:00",
            "honorific": "UC_N1LWUS",
            "hasPhone": "Y",
            "hasEmail": "Y",
            "hasImol": "N",
            "login": null,
            "isReturnCustomer": "N",
            "searchContent": "250 Lead #250 Adminov Admin Adminovich Administrative Company 999.90 US Dollar 6111111111 111111111 11111111 1111111 111111 11111 1111 111 nqzva rknzcyr pbz In Process Exhibition Exhibition about Admin City Admin [O]Comment about admin[/O] 321",
            "isManualOpportunity": "Y",
            "movedBy": 1,
            "movedTime": "2024-07-22T17:00:08+02:00",
            "lastActivityBy": 1,
            "lastActivityTime": "2024-07-22T17:00:08+02:00",
            "phoneMobile": "",
            "phoneWork": "+16111111111",
            "phoneMailing": "",
            "emailHome": "",
            "emailWork": "admin@example.com",
            "emailMailing": "",
            "skype": null,
            "icq": null,
            "imol": "",
            "email": "admin@example.com",
            "phone": "+16111111111",
            "ufCrm_1720019876534": "321",
            "parentId1222": null,
            "parentId1226": null,
            "parentId1228": null,
            "parentId1236": null,
            "parentId1238": null,
            "parentId1240": null,
            "parentId1244": null,
            "parentId1246": null,
            "parentId1254": null,
            "parentId1256": null,
            "utmSource": null,
            "utmMedium": null,
            "utmCampaign": null,
            "utmContent": null,
            "utmTerm": null,
            "observers": [],
            "contactIds": [],
            "entityTypeId": 1
        }
    },
    "time": {
        "start": 1721660468.931424,
        "finish": 1721660469.416092,
        "duration": 0.4846680164337158,
        "processing": 0.16368508338928223,
        "date_start": "2024-07-22T17:01:08+02:00",
        "date_finish": "2024-07-22T17:01:09+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response. Contains a single key `item` ||
|| **item**
[`item`](./object-fields.md) | Information about the item, [field description](./object-fields.md) ||
|| **time**
[`time`][1] | Object containing information about the request execution time ||
|#

{% note info " " %}

By default, custom field names are returned in camelCase, e.g., `ufCrm2_1639669411830`.
When passing the parameter `useOriginalUfNames` with the value `Y`, custom fields will be returned with their original names, e.g., `UF_CRM_2_1639669411830`.

{% endnote %}

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Item not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code**                          | **Description**                                     | **Value**                                                    ||
|| `403`      | `allowed_only_intranet_user`     | Action allowed only for intranet users            | User is not an intranet user                                 ||
|| `400`      | `NOT_FOUND`                      | SPA not found                                      | Occurs when an invalid `entityTypeId` is passed             ||
|| `400`      | `NOT_FOUND`                      | Item not found                                    | Item with the given `id` of type `entityTypeId` does not exist ||
|| `400`      | `ACCESS_DENIED`                  | You do not have permission to view this item      | User does not have read access permission for items of type `entityTypeId` ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-add.md)
- [{#T}](./crm-item-update.md)
- [{#T}](./crm-item-list.md)
- [{#T}](./crm-item-delete.md)
- [{#T}](./crm-item-fields.md)
- [{#T}](./object-fields.md)

[1]: ../../data-types.md