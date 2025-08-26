# Get the user type by id crm.type.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with administrative access to the SPA, or a user with read access to the SPA

The method retrieves information about the SPA with the identifier `id`.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`][1] | Identifier of the SPA. Can be obtained using the methods: [`crm.type.list`](./crm-type-list.md), [`crm.type.add`](./crm-type-add.md) ||
|#

## Code Examples

Get information about the SPA with `id = 16`.

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":16}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.type.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":16,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.type.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.type.get',
    		{
    			id: 16,
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
        $response = $b24Service
            ->core
            ->call(
                'crm.type.get',
                [
                    'id' => 16,
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
        echo 'Error getting CRM type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.type.get',
        {
            id: 16,
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
        'crm.type.get',
        [
            'id' => 16
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
        "type": {
            "id": 16,
            "title": "SPA",
            "code": "",
            "createdBy": 1,
            "entityTypeId": 2024,
            "customSectionId": null,
            "isCategoriesEnabled": "Y",
            "isStagesEnabled": "Y",
            "isBeginCloseDatesEnabled": "Y",
            "isClientEnabled": "Y",
            "isUseInUserfieldEnabled": "Y",
            "isLinkWithProductsEnabled": "Y",
            "isMycompanyEnabled": "Y",
            "isDocumentsEnabled": "Y",
            "isSourceEnabled": "Y",
            "isObserversEnabled": "Y",
            "isRecyclebinEnabled": "Y",
            "isAutomationEnabled": "Y",
            "isBizProcEnabled": "Y",
            "isSetOpenPermissions": "Y",
            "isPaymentsEnabled": "N",
            "isCountersEnabled": "N",
            "createdTime": "2024-07-08T14:46:54+02:00",
            "updatedTime": "2024-07-08T14:46:54+02:00",
            "updatedBy": 1,
            "relations": {
                "parent": [
                    {
                        "entityTypeId": 3,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "Y"
                    },
                    {
                        "entityTypeId": 4,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "Y"
                    },
                    {
                        "entityTypeId": 1,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "N"
                    },
                    {
                        "entityTypeId": 2,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "N"
                    },
                    {
                        "entityTypeId": 31,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "N"
                    }
                ],
                "child": [
                    {
                        "entityTypeId": 3,
                        "isChildrenListEnabled": "Y",
                        "isPredefined": "N"
                    },
                    {
                        "entityTypeId": 4,
                        "isChildrenListEnabled": "N",
                        "isPredefined": "N"
                    }
                ]
            },
            "linkedUserFields": {
                "CALENDAR_EVENT|UF_CRM_CAL_EVENT": "Y",
                "TASKS_TASK|UF_CRM_TASK": "Y",
                "TASKS_TASK_TEMPLATE|UF_CRM_TASK": "N"
            },
            "customSections": []
        }
    },
    "time": {
        "start": 1720442829.865672,
        "finish": 1720442830.334845,
        "duration": 0.46917295455932617,
        "processing": 0.13246917724609375,
        "date_start": "2024-07-08T14:47:09+02:00",
        "date_finish": "2024-07-08T14:47:10+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response containing the object [`type`](../../data-types.md#type) with information about the SPA ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "SPA not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes
#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `allowed_only_intranet_user` | Action allowed only for intranet users | Occurs if the user is not an intranet user ||
|| `400` | `ACCESS_DENIED` | Access denied | Occurs if the user does not have administrative rights in CRM or does not have read access to the SPA ||
|| `400` | `0` | SPA not found | SPA with the provided `id` not found ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-type-add.md)
- [{#T}](./crm-type-update.md)
- [{#T}](./crm-type-get-by-entity-type-id.md)
- [{#T}](./crm-type-list.md)
- [{#T}](./crm-type-delete.md)
- [{#T}](./crm-type-fields.md)


[1]: ../../../data-types.md