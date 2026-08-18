# How to Create a Custom Field in a Smart Process

> Scope: [`crm`, `userfieldconfig`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — administrative access to the CRM section
>
> - [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) — a user with administrative access to the CRM section
> - [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) — a user with the "Allow changing settings" permission in CRM
> - [userfieldconfig.list](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-list.md) — a user with permission to read smart process items

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields extend CRM functionality to meet your business requirements:

- You can create fields to store information in various formats: string, money, number, address, file, and others.

- You can configure field properties: names for different languages, a multiple field flag, rounding settings for numeric fields, and others.

The custom field code and the object identifier are built from the sequential number of the smart process, so you first need to retrieve it from the smart process settings. As a result of the scenario, a multiple field of the "list" type with two value options appears in the smart process.

The scenario consists of two steps.

1. Retrieve the `id` of the smart process using the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method.
2. Create the custom field using the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method, building the object identifier and the field code from `id`.

## Before You Start

- The smart process is already created in Bitrix24, and you know its name.

- The webhook is created on behalf of a user with administrative access to the CRM section. Without it, the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method returns an error.

- Both scopes are selected in the webhook permissions: `crm` and `userfieldconfig`. The [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method requires the `userfieldconfig` scope and the scope of the module passed in `moduleId`.

## 1. Retrieve the Smart Process ID {#spa-id}

To retrieve the SPA ID, use the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method with a filter:

- `title` — specify the SPA name. Replace `Equipment procurement` with the name of your own smart process.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: 'crm.type.list',
        params: {
            filter: { // array of fields for filtering
                "title": "Equipment procurement" // smart process name
            }
        },
        requestId: 'type-list'
    });
    ```

- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $sb->getCRMScope()->type()->list(
        order: [],
        filter: ['title' => 'Equipment procurement'] // smart process name
    );
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    result = client.crm.type.list(
        filter={
            "title": "Equipment procurement",
        }
    ).response.result
    ```

{% endlist %}

As a result, you will receive an `id` — this is the sequential number of the SPA in Bitrix24. In the example `id`: `7`.

```json
{
    "result": {
        "types": [
            {
                "id": 7,
                "title": "Equipment procurement",
                "code": "",
                "createdBy": 1,
                "entityTypeId": 177,
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
                "createdTime": "2021-11-26T10:52:17+03:00",
                "updatedTime": "2024-11-12T15:32:39+03:00",
                "updatedBy": 1
            }
        ]
    }
}
```

{% note warning "" %}

From here on, you need exactly the `id`, not the `entityTypeId`. These are different numbers: for a smart process with `id`: `7`, the type identifier is `177`. If you substitute `entityTypeId`, the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method rejects the request with the message "You cannot create custom fields".

{% endnote %}

## 2. Create a Custom Field in an SPA

To create a custom field, use the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method with the following parameters:

- `moduleId` — the module identifier where the method will create the field, a required parameter. The SPA module is `crm`.

- `field[entityId]` — the object identifier following the formula `CRM_ + {id}`, where `id` is the sequential number of the SPA from the [crm.type.list](./how-to-add-user-field-to-spa.md#spa-id) step result, a required parameter. In the example, we will specify `CRM_7`.

- `field[fieldName]` — the field code according to the formula `UF_ + {object_id} + _ + {arbitrary string in UPPERCASE}`. The code length limit is 50 characters, a required parameter. In the example, we will specify `UF_CRM_7_NEW_REST_LIST`.

- `field[userTypeId]` — the [field type](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md) identifier, a required parameter. In the example, we will specify `enumeration` to create a list type field; the value options for the list field will be passed in a separate `enum` array.

- `field[multiple]` — a multiple field flag, an optional parameter. The multiplicity flag cannot be changed after the field is created.

- `field[editFormLabel]` — an array of names for displaying the field in Bitrix24 in different languages. An optional parameter; if no name is provided, Bitrix24 will display the field code.

{% list tabs %}

- JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'userfieldconfig.add',
        params: {
            moduleId: 'crm', // Module ID
            field: {
                entityId: 'CRM_7', // Object ID
                fieldName: 'UF_CRM_7_NEW_REST_LIST', // Field code
                userTypeId: 'enumeration', // Field type ID
                multiple: 'Y', // Multiple flag
                editFormLabel: {
                    'de': 'List of characteristics', // Field name in German
                    'en': 'List of characteristics' // Field name in English
                },
                enum: [ // List field values
                    {
                        value: 'Characteristic 1', // Option value
                        def: 'N', // Default value flag
                        sort: 100, // Sort index
                    },
                    {
                        value: 'Characteristic 2',
                        def: 'Y', // This option will be the default value
                        sort: 200,
                    }
                ]
            }
        },
        requestId: 'userfieldconfig-add'
    });
    ```

- PHP

    ```php
    // userfieldconfig.add has no wrapper in the SDK — calling the method directly
    $result = $sb->core->call(
        'userfieldconfig.add',
        [
            'moduleId' => 'crm', // Module ID
            'field' => [
                'entityId' => 'CRM_7', // Object ID
                'fieldName' => 'UF_CRM_7_NEW_REST_LIST', // Field code
                'userTypeId' => 'enumeration', // Field type ID
                'multiple' => 'Y', // Multiple flag
                'editFormLabel' => [
                    'de' => 'List of characteristics', // Field name in German
                    'en' => 'List of characteristics' // Field name in English
                ],
                'enum' => [ // List field values
                    [
                        'value' => 'Characteristic 1', // Option value
                        'def' => 'N', // Default value flag
                        'sort' => 100, // Sort index
                    ],
                    [
                        'value' => 'Characteristic 2',
                        'def' => 'Y', // This option will be the default value
                        'sort' => 200,
                    ]
                ]
            ]
        ]
    );
    ```

- Python

    ```python
    field = client.userfieldconfig.add(
        module_id="crm",  # Module ID
        field={
            "entityId": "CRM_7",  # Object ID
            "fieldName": "UF_CRM_7_NEW_REST_LIST",  # Field code
            "userTypeId": "enumeration",  # Field type ID
            "multiple": "Y",  # Multiple flag
            "editFormLabel": {
                "de": "List of characteristics",  # Field name in German
                "en": "List of characteristics",  # Field name in English
            },
            "enum": [  # List field values
                {
                    "value": "Characteristic 1",  # Option value
                    "def": "N",  # Default value flag
                    "sort": 100,  # Sort index
                },
                {
                    "value": "Characteristic 2",
                    "def": "Y",  # This option will be the default value
                    "sort": 200,
                },
            ],
        },
    ).response.result["field"]
    ```

{% endlist %}

As a result, you will receive the data for the created field. Retain the `id` — you will need it to modify the field using the [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) method or to delete it using the [userfieldconfig.delete](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-delete.md) method.

```json
{
    "result": {
        "field": {
            "id": "6953",
            "entityId": "CRM_7",
            "fieldName": "UF_CRM_7_NEW_REST_LIST",
            "userTypeId": "enumeration",
            "xmlId": null,
            "sort": "100",
            "multiple": "Y",
            "mandatory": "N",
            "showFilter": "N",
            "showInList": "Y",
            "editInList": "Y",
            "isSearchable": "N",
            "settings": {
                "DISPLAY": "LIST",
                "LIST_HEIGHT": 1,
                "CAPTION_NO_VALUE": "",
                "SHOW_NO_VALUE": "Y"
            },
            "languageId": {
                "en": "en",
                "de": "de"
            },
            "editFormLabel": {
                "en": "List of characteristics",
                "de": "List of characteristics"
            },
            "listColumnLabel": {
                "en": null,
                "de": null
            },
            "listFilterLabel": {
                "en": null,
                "de": null
            },
            "errorMessage": {
                "en": null,
                "de": null
            },
            "helpMessage": {
                "en": null,
                "de": null
            },
            "enum": [
                {
                    "id": "3363",
                    "userFieldId": "6953",
                    "value": "Characteristic 1",
                    "def": "N",
                    "sort": "100",
                    "xmlId": "56dff18efcfe25f3bae0117a6b372567"
                },
                {
                    "id": "3365",
                    "userFieldId": "6953",
                    "value": "Characteristic 2",
                    "def": "Y",
                    "sort": "200",
                    "xmlId": "42e3ebcf5506a65283bf3bf510d8f05a"
                }
            ]
        }
    }
}
```

## Verify the Result

Open the card of any smart process item in Bitrix24. The new field is displayed in the card under the name from `editFormLabel` — "List of characteristics". The "Characteristic 2" value is set by default because it has `def`: `Y`.

Through REST, the set of smart process fields is returned by the [userfieldconfig.list](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-list.md) method with the following parameters:

- `moduleId` — `crm`

- `filter` — specify the `entityId` field with the value `CRM_7` to retrieve only the fields of this smart process

{% list tabs %}

- JS

    ```javascript
    const checkResult = await $b24.actions.v2.call.make({
        method: 'userfieldconfig.list',
        params: {
            moduleId: 'crm',
            filter: { entityId: 'CRM_7' }
        },
        requestId: 'userfieldconfig-list'
    });

    console.dir(checkResult.getData().result.fields);
    ```

- PHP

    ```php
    // userfieldconfig.list has no wrapper in the SDK — calling the method directly
    $fields = $sb->core->call(
        'userfieldconfig.list',
        [
            'moduleId' => 'crm',
            'filter' => ['entityId' => 'CRM_7']
        ]
    )->getResponseData()->getResult();
    ```

- Python

    ```python
    fields = client.userfieldconfig.list(
        module_id="crm",
        filter={"entityId": "CRM_7"},
    ).response.result["fields"]
    ```

{% endlist %}

The scenario is complete if the `fields` array contains an object with `fieldName`: `UF_CRM_7_NEW_REST_LIST`, its `userTypeId` equals `enumeration`, and `multiple` equals `Y`.

## Errors and Diagnostics

If the method returns an error, check the request data. The [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method returns errors with an empty code, so rely on the text in `error_description`.

#|
|| **Error text** | **Reason and action** ||
|| `You cannot create custom fields` | Either `field[entityId]` contains an object identifier that does not exist, or `field[fieldName]` does not start with `UF_{entityId}_`. A common cause is `entityTypeId` instead of `id`: a smart process with `id`: `7` needs `CRM_7`, not `CRM_177`. For the `CRM_7` object, the code must start with `UF_CRM_7_` ||
|| `Field ... already exists` | A field with this `field[fieldName]` has already been created for this object. Choose another code or modify the existing field using the [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) method ||
|| `The 'FIELD_NAME' field is not found` | The required `field[fieldName]` was not passed ||
|| `The 'USER_TYPE_ID' field is not found` | The required `field[userTypeId]` was not passed. The list of allowed values is returned by the [userfieldconfig.getTypes](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-get-types.md) method ||
|| `Access denied` | The user does not have the "Allow changing settings" permission in CRM. Check which user the webhook was created on behalf of ||
|| `Fail to save enumeration field values` | The list options were not retained. Check the `enum` array: each option requires a non-empty `value`, and `def` accepts only `Y` or `N` ||
|#

An error with the text `ACCESS_DENIED` or `allowed_only_intranet_user` is returned by step 1: the webhook user does not have administrative access to the CRM section.

Step 1 does not create anything, so it can be repeated any number of times. If step 2 returned the error, the field was not created: fix the `field` and repeat only that step.

## Key Considerations

- The `multiple` flag cannot be changed after the field is created. To make a field multiple, delete it using the [userfieldconfig.delete](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-delete.md) method and create it again

- The method does not check whether `field[entityId]` belongs to a smart process. The `CRM_ + {id}` formula works only for smart processes; leads, deals, and other CRM objects have different object identifiers

- Running the example again with the same `fieldName` returns the "Field ... already exists" error, and no new field is created

- The list options are returned in the `enum` array with their own `id` values. To add or modify an option later, pass these `id` values to the [userfieldconfig.update](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md) method

## Code Example

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Function to get the smart process and create a custom field
    async function getCrmTypeAndAddUserField() {
        // Variable for the smart process name entered by the user
        var processTitle = prompt("Enter the smart process name to search for:", "Your_Process_Name");
        try {
            // Calling the crm.type.list method to get the smart process
            const result = await $b24.actions.v2.call.make({
                method: 'crm.type.list',
                params: { filter: { "title": processTitle } }, // Using the name entered by the user
                requestId: 'type-list'
            });
            console.log('Smart process successfully retrieved:', result.getData().result);
            var spaId = result.getData().result.types[0].id; // Using the id, not the entityTypeId
            await addUserField(spaId);
        } catch (error) {
            console.error('Error retrieving the smart process:', error);
        }
    }

    // Function to create a custom field
    async function addUserField(spaId) {
        try {
            // Calling the userfieldconfig.add method to create a custom field
            const result = await $b24.actions.v2.call.make({
                method: 'userfieldconfig.add',
                params: {
                    moduleId: 'crm',
                    field: {
                        entityId: 'CRM_' + spaId, // Using the id from the previous result
                        fieldName: 'UF_CRM_' + spaId + '_NEW_REST_LIST', // The field code starts with UF_ + object ID
                        userTypeId: 'enumeration',
                        multiple: 'Y',
                        editFormLabel: {
                            'de': 'List of characteristics',
                            'en': 'List of characteristics'
                        },
                        enum: [
                            {
                                value: 'Characteristic 1',
                                def: 'N',
                                sort: 100
                            },
                            {
                                value: 'Characteristic 2',
                                def: 'Y',
                                sort: 200
                            }
                        ]
                    }
                },
                requestId: 'userfieldconfig-add'
            });
            console.log('Custom field successfully created:', result.getData().result);
        } catch (error) {
            console.error('Error creating custom field:', error);
        }
    }

    // Calling the function to get smart process data and create a custom field
    getCrmTypeAndAddUserField();
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Bitrix24\SDK\Services\ServiceBuilder;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Function to get the smart process and create a custom field
    function getCrmTypeAndAddUserField(ServiceBuilder $sb, $processTitle) {
        try {
            // Calling the crm.type.list method to get the smart process
            $types = $sb->getCRMScope()->type()->list(
                order: [],
                filter: ['title' => $processTitle] // Using the name entered by the user
            )->getTypes();

            if (!empty($types)) {
                $spaId = $types[0]->id; // Using the id, not the entityTypeId
                addUserField($sb, $spaId);
            } else {
                echo 'Smart process not found.';
            }
        } catch (\Throwable $e) {
            echo 'Error retrieving the smart process: ' . $e->getMessage();
        }
    }

    // Function to create a custom field
    function addUserField(ServiceBuilder $sb, $spaId) {
        try {
            // userfieldconfig.add has no wrapper in the SDK — calling the method directly
            $sb->core->call('userfieldconfig.add', [
                'moduleId' => 'crm',
                'field' => [
                    'entityId' => 'CRM_' . $spaId, // Using the id from the previous result
                    'fieldName' => 'UF_CRM_' . $spaId . '_NEW_REST_LIST', // The field code starts with UF_ + object ID
                    'userTypeId' => 'enumeration',
                    'multiple' => 'Y',
                    'editFormLabel' => [
                        'de' => 'List of characteristics',
                        'en' => 'List of characteristics'
                    ],
                    'enum' => [
                        [
                            'value' => 'Characteristic 1',
                            'def' => 'N',
                            'sort' => 100
                        ],
                        [
                            'value' => 'Characteristic 2',
                            'def' => 'Y',
                            'sort' => 200
                        ]
                    ]
                ]
            ]);
            echo 'Custom field successfully created.';
        } catch (\Throwable $e) {
            echo 'Error creating custom field: ' . $e->getMessage();
        }
    }

    // Calling the function to get smart process data and create a custom field
    $processTitle = readline("Enter the smart process name to search for: ");
    getCrmTypeAndAddUserField($sb, $processTitle);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    def get_crm_type_and_add_user_field(client):
        process_title = input("Enter the smart process name to search for: ")

        try:
            resp = client.crm.type.list(
                filter={"title": process_title},
            ).response
        except BitrixAPIError as error:
            print(f"Error retrieving the smart process: {error}")
            return

        print("Smart process successfully retrieved:")
        print(resp.result)

        types = resp.result.get("types") or []
        if types:
            spa_id = int(types[0]["id"])  # using the id, not the entityTypeId
            add_user_field(client, spa_id)
        else:
            print("Smart process not found.")

    def add_user_field(client, spa_id):
        try:
            result = client.userfieldconfig.add(
                module_id="crm",
                field={
                    "entityId": f"CRM_{spa_id}",
                    # the field code starts with UF_ + object ID
                    "fieldName": f"UF_CRM_{spa_id}_NEW_REST_LIST",
                    "userTypeId": "enumeration",
                    "multiple": "Y",
                    "editFormLabel": {
                        "de": "List of characteristics",
                        "en": "List of characteristics",
                    },
                    "enum": [
                        {"value": "Characteristic 1", "def": "N", "sort": 100},
                        {"value": "Characteristic 2", "def": "Y", "sort": 200},
                    ],
                },
            ).response
        except BitrixAPIError as error:
            print(f"Error creating custom field: {error}")
        else:
            print("Custom field successfully created:")
            print(result.result)

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    get_crm_type_and_add_user_field(client)
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md)
- [{#T}](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-list.md)
- [{#T}](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-update.md)
- [{#T}](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-delete.md)
- [{#T}](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-get-types.md)
- [{#T}](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md)
- [{#T}](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md)
