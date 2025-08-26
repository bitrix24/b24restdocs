# Create a new custom type crm.type.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with administrative access to the CRM section

This method creates a new SPA.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type`         | **Description** ||
|| **fields***
[`object`][1] | Field values (detailed description provided [below](#parametr-fields)) for adding a new SPA ||
|#

### Parameter fields

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** | **Default** ||
|| **title***
[`string`][1]  | Title of the SPA | ||
|| **entityTypeId**
[`integer`][1] | Identifier of the created SPA. If this field is not provided, it will be generated automatically.

It should be noted that:
1. This parameter is unique. It is not possible to create two SPAs with the same `entityTypeId`
2. The value of `entityTypeId` must be within one of two ranges:
   - an even integer that is greater than or equal to 1030
   - in the range from 128 to 192
| ||
|| **relations**
[`object`][1]  | An object containing links to other CRM entities. The structure is described [below](#relations) | ||
|| **isUseInUserfieldEnabled** 
[`boolean`][1] | Is the use of the SPA in the user field enabled | `N` ||
|| **linkedUserFields** 
[`object`][1]  | A set of user fields in which this SPA should be displayed. The structure is described [below](#linkedUserFields) | `{}` ||
|| **isAutomationEnabled**
[`boolean`][1] | Are Automation rules and triggers enabled | `N` ||
|| **isBeginCloseDatesEnabled**
[`boolean`][1] | Are the **Start Date** and **End Date** fields enabled | `N` ||
|| **isBizProcEnabled**
[`boolean`][1] | Is the business process designer enabled | `N` ||
|| **isCategoriesEnabled**
[`boolean`][1] | Are custom sales funnels and tunnels enabled | `N` ||
|| **isClientEnabled**
[`boolean`][1] | Is the **Client** field enabled. When this option is enabled, the SPA has a preset binding to **Contacts** and **Companies** | `N` ||
|| **isDocumentsEnabled**
[`boolean`][1] | Is document printing enabled | `N` ||
|| **isLinkWithProductsEnabled**
[`boolean`][1] | Is the binding of catalog products enabled | `N` ||
|| **isMycompanyEnabled**
[`boolean`][1] | Is the **Your Company Details** field enabled | `N` ||
|| **isObserversEnabled** 
[`boolean`][1] | Is the **Observers** field enabled | `N` ||
|| **isRecyclebinEnabled**
[`boolean`][1] | Is the use of the recycle bin enabled | `N` ||
|| **isSetOpenPermissions** 
[`boolean`][1] | Should new funnels be available to everyone | `Y` ||
|| **isSourceEnabled** 
[`boolean`][1] | Are the **Source** and **Additional Information about Source** fields enabled | `N` ||
|| **isStagesEnabled**
[`boolean`][1] | Is the use of custom stages and kanban enabled | `N` ||
|| **isExternal**
[`boolean`][1] | Is the SPA external to the CRM (linked to a digital workplace)

This parameter is deprecated. For working with digital workplaces, use the methods [`crm.automatedsolution.*`](../../automated-solution/index.md) | `-` ||
|| **customSectionId**
[`integer`][1] | Identifier of the digital workplace

This parameter is deprecated. For working with digital workplaces, use the methods [`crm.automatedsolution.*`](../../automated-solution/index.md) | `-` ||
|| **customSections**
[`array`][1] | Array of digital workplaces

This parameter is deprecated. For working with digital workplaces, use the methods [`crm.automatedsolution.*`](../../automated-solution/index.md) | `-` ||
|#

### Relations relations {#relations}

#|
|| **Name**
`type` | **Description** ||
|| **parent**
[`relation[]`](#relation) | CRM elements that will be linked to this SPA ||
|| **child**
[`relation[]`](#relation) | CRM elements to which this SPA will be linked ||
|#

#### One relation relation {#relation}

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** | **Default** ||
|| **entityTypeId***
[`integer`][1] | Identifier of the [system](../index.md) or [custom type](./index.md) of the CRM entity | `-` ||
|| **isChildrenListEnabled**
[`boolean`][1] | Should the linked element be added to the card. 

Values `Y` and `N` do not work. You must pass `"true"` or `"false"` | `"false"` ||
|#

### Binding to user fields {#linkedUserFields}

`linkedUserFields` — a set of fields in which this SPA should be displayed.
This setting will only be considered if `isUseInUserfieldEnabled = 'Y'` is passed.

#|
|| **Name**
`type` | **Description** | **Default Value** ||
|| **CALENDAR_EVENT**\|**UF_CRM_CAL_EVENT**
[`boolean`][1] | Calendar event | `"false"` ||
|| **TASKS_TASK**\|**UF_CRM_TASK**
[`boolean`][1] | Tasks | `"false"` ||
|| **TASKS_TASK_TEMPLATE**\|**UF_CRM_TASK**
[`boolean`][1] | Task templates | `"false"` ||
|#

These parameters do not support passing values `Y` and `N`. Use `"true"` or `"false"`.

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

Let's create an SPA with the following conditions:
* The most complete set of enabled settings
* Link `Leads`, `Deals`, `Invoices` to the created SPA. Each link is added to the card of the elements of this SPA
* Link the created SPA to `Contacts` and `Companies`. The link to `Contacts` is added to the card
* The SPA should be displayed in the following fields: `Calendar Event`, `Tasks`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"SPA","entityTypeId":2024,"isAutomationEnabled":"Y","isBeginCloseDatesEnabled":"Y","isBizProcEnabled":"Y","isCategoriesEnabled":"Y","isClientEnabled":"Y","isDocumentsEnabled":"Y","isLinkWithProductsEnabled":"Y","isMycompanyEnabled":"Y","isObserversEnabled":"Y","isRecyclebinEnabled":"Y","isSetOpenPermissions":"Y","isSourceEnabled":"Y","isStagesEnabled":"Y","isUseInUserfieldEnabled":"Y","linkedUserFields":{"CALENDAR_EVENT|UF_CRM_CAL_EVENT":"true","TASKS_TASK|UF_CRM_TASK":"true"},"relations":{"parent":[{"entityTypeId":1,"isChildrenListEnabled":"true"},{"entityTypeId":2,"isChildrenListEnabled":"true"},{"entityTypeId":31,"isChildrenListEnabled":"true"}],"child":[{"entityTypeId":3,"isChildrenListEnabled":"true"},{"entityTypeId":4}]}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.type.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"SPA","entityTypeId":2024,"isAutomationEnabled":"Y","isBeginCloseDatesEnabled":"Y","isBizProcEnabled":"Y","isCategoriesEnabled":"Y","isClientEnabled":"Y","isDocumentsEnabled":"Y","isLinkWithProductsEnabled":"Y","isMycompanyEnabled":"Y","isObserversEnabled":"Y","isRecyclebinEnabled":"Y","isSetOpenPermissions":"Y","isSourceEnabled":"Y","isStagesEnabled":"Y","isUseInUserfieldEnabled":"Y","linkedUserFields":{"CALENDAR_EVENT|UF_CRM_CAL_EVENT":"true","TASKS_TASK|UF_CRM_TASK":"true"},"relations":{"parent":[{"entityTypeId":1,"isChildrenListEnabled":"true"},{"entityTypeId":2,"isChildrenListEnabled":"true"},{"entityTypeId":31,"isChildrenListEnabled":"true"}],"child":[{"entityTypeId":3,"isChildrenListEnabled":"true"},{"entityTypeId":4}]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.type.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.type.add',
    		{
    			fields: {
    				title: "SPA",
    				entityTypeId: 2024,
    				isAutomationEnabled: "Y",
    				isBeginCloseDatesEnabled: "Y",
    				isBizProcEnabled: "Y",
    				isCategoriesEnabled: "Y",
    				isClientEnabled: "Y",
    				isDocumentsEnabled: "Y",
    				isLinkWithProductsEnabled: "Y",
    				isMycompanyEnabled: "Y",
    				isObserversEnabled: "Y",
    				isRecyclebinEnabled: "Y",
    				isSetOpenPermissions: "Y",
    				isSourceEnabled: "Y",
    				isStagesEnabled: "Y",
    				isUseInUserfieldEnabled: "Y",
    				linkedUserFields: {
    					"CALENDAR_EVENT|UF_CRM_CAL_EVENT": "true",
    					"TASKS_TASK|UF_CRM_TASK": "true",
    				},
    				relations: {
    					parent: [
    						{
    							entityTypeId: 1,
    							isChildrenListEnabled: "true",
    						},
    						{
    							entityTypeId: 2,
    							isChildrenListEnabled: "true",
    						},
    						{
    							entityTypeId: 31,
    							isChildrenListEnabled: "true",
    							
    						},
    					],
    					child: [
    						{
    							entityTypeId: 3,
    							isChildrenListEnabled: "true",
    						},
    						{
    							entityTypeId: 4,
    						},
    					],
    				},
    			},
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
                'crm.type.add',
                [
                    'fields' => [
                        'title'                     => "SPA",
                        'entityTypeId'              => 2024,
                        'isAutomationEnabled'       => "Y",
                        'isBeginCloseDatesEnabled'  => "Y",
                        'isBizProcEnabled'          => "Y",
                        'isCategoriesEnabled'       => "Y",
                        'isClientEnabled'           => "Y",
                        'isDocumentsEnabled'        => "Y",
                        'isLinkWithProductsEnabled' => "Y",
                        'isMycompanyEnabled'        => "Y",
                        'isObserversEnabled'        => "Y",
                        'isRecyclebinEnabled'       => "Y",
                        'isSetOpenPermissions'      => "Y",
                        'isSourceEnabled'           => "Y",
                        'isStagesEnabled'           => "Y",
                        'isUseInUserfieldEnabled'   => "Y",
                        'linkedUserFields'          => [
                            "CALENDAR_EVENT|UF_CRM_CAL_EVENT" => "true",
                            "TASKS_TASK|UF_CRM_TASK"          => "true",
                        ],
                        'relations'                 => [
                            'parent' => [
                                [
                                    'entityTypeId'           => 1,
                                    'isChildrenListEnabled'  => "true",
                                ],
                                [
                                    'entityTypeId'           => 2,
                                    'isChildrenListEnabled'  => "true",
                                ],
                                [
                                    'entityTypeId'           => 31,
                                    'isChildrenListEnabled'  => "true",
                                ],
                            ],
                            'child'  => [
                                [
                                    'entityTypeId'           => 3,
                                    'isChildrenListEnabled'  => "true",
                                ],
                                [
                                    'entityTypeId'           => 4,
                                ],
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding CRM type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.type.add',
        {
            fields: {
                title: "SPA",
                entityTypeId: 2024,
                isAutomationEnabled: "Y",
                isBeginCloseDatesEnabled: "Y",
                isBizProcEnabled: "Y",
                isCategoriesEnabled: "Y",
                isClientEnabled: "Y",
                isDocumentsEnabled: "Y",
                isLinkWithProductsEnabled: "Y",
                isMycompanyEnabled: "Y",
                isObserversEnabled: "Y",
                isRecyclebinEnabled: "Y",
                isSetOpenPermissions: "Y",
                isSourceEnabled: "Y",
                isStagesEnabled: "Y",
                isUseInUserfieldEnabled: "Y",
                linkedUserFields: {
                    "CALENDAR_EVENT|UF_CRM_CAL_EVENT": "true",
                    "TASKS_TASK|UF_CRM_TASK": "true",
                },
                relations: {
                    parent: [
                        {
                            entityTypeId: 1,
                            isChildrenListEnabled: "true",
                        },
                        {
                            entityTypeId: 2,
                            isChildrenListEnabled: "true",
                        },
                        {
                            entityTypeId: 31,
                            isChildrenListEnabled: "true",
                            
                        },
                    ],
                    child: [
                        {
                            entityTypeId: 3,
                            isChildrenListEnabled: "true",
                        },
                        {
                            entityTypeId: 4,
                        },
                    ],
                },
            },
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
        'crm.type.add',
        [
            'fields' => [
                'title' => "SPA",
                'entityTypeId' => 2024,
                'isAutomationEnabled' => "Y",
                'isBeginCloseDatesEnabled' => "Y",
                'isBizProcEnabled' => "Y",
                'isCategoriesEnabled' => "Y",
                'isClientEnabled' => "Y",
                'isDocumentsEnabled' => "Y",
                'isLinkWithProductsEnabled' => "Y",
                'isMycompanyEnabled' => "Y",
                'isObserversEnabled' => "Y",
                'isRecyclebinEnabled' => "Y",
                'isSetOpenPermissions' => "Y",
                'isSourceEnabled' => "Y",
                'isStagesEnabled' => "Y",
                'isUseInUserfieldEnabled' => "Y",
                'linkedUserFields' => [
                    "CALENDAR_EVENT|UF_CRM_CAL_EVENT" => "true",
                    "TASKS_TASK|UF_CRM_TASK" => "true",
                ],
                'relations' => [
                    'parent' => [
                        [
                            'entityTypeId' => 1,
                            'isChildrenListEnabled' => "true",
                        ],
                        [
                            'entityTypeId' => 2,
                            'isChildrenListEnabled' => "true",
                        ],
                        [
                            'entityTypeId' => 31,
                            'isChildrenListEnabled' => "true",
                        ],
                    ],
                    'child' => [
                        [
                            'entityTypeId' => 3,
                            'isChildrenListEnabled' => "true",
                        ],
                        [
                            'entityTypeId' => 4,
                        ],
                    ],
                ],
            ],
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
            "createdBy": 1,
            "entityTypeId": 2024,
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
            "createdTime": "2024-07-05T19:47:33+02:00",
            "updatedTime": "2024-07-05T19:47:33+02:00",
            "updatedBy": 1,
            "title": "SPA",
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
            "customSections": [],
            "customSectionId": null
        }
    },
    "time": {
        "start": 1720201651.707909,
        "finish": 1720201654.748627,
        "duration": 3.040717840194702,
        "processing": 2.71589994430542,
        "date_start": "2024-07-05T19:47:31+02:00",
        "date_finish": "2024-07-05T19:47:34+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | Root element of the response. Contains a single key `type` ||
|| **type**
[`type`](../../data-types.md#type) | Information about the created SPA ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "INVALID_ARG_VALUE", 
    "error_description": "entityTypeId is out of allowed range"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `INVALID_ARG_VALUE` | entityTypeId is out of allowed range | Occurs when an invalid `entityTypeId` is passed ||
|| `400` | `ACCESS_DENIED` | Access denied | Occurs if the user does not have administrative rights in CRM ||
|| `403` | `allowed_only_intranet_user` | Action allowed only for intranet users | Occurs if the user is not an intranet user ||
|| `400` | `100` | Could not find value for parameter {fields} | Occurs if the required parameter `fields` is not passed ||
|| `400` | `0` | Select a workplace where the SPA will be located | Occurs when `isExternal = 'true'`, but `customSectionId` is empty ||
|| `400` | `CREATE_DYNAMIC_TYPE_RESTRICTED` | Maximum number of SPAs exceeded | Creating an SPA is restricted due to the plan ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-type-update.md)
- [{#T}](./crm-type-get.md)
- [{#T}](./crm-type-get-by-entity-type-id.md)
- [{#T}](./crm-type-list.md)
- [{#T}](./crm-type-delete.md)
- [{#T}](./crm-type-fields.md)

[1]: ../../../data-types.md