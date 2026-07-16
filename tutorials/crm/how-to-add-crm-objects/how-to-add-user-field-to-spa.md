# How to Create a Custom Field in a Smart Process

> Scope: [`crm, userfieldconfig`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify the smart process

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Custom fields extend CRM functionality to meet your business requirements:

- You can create fields to store information in various formats: string, money, number, address, file, and others.

- You can configure field properties: names for different languages, a multiple field flag, rounding settings for numeric fields, and others.

To create a custom field in an SPA, we will sequentially call two methods:

1. [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) — retrieve the SPA ID.

2. [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) — create a custom field in the SPA.

## 1. Retrieve the Smart Process ID {#spa-id}

To retrieve the SPA ID, use the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method with a filter:

- `title` — specify the SPA name.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```JavaScript
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

## 2. Create a Custom Field in an SPA

To create a custom field, use the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method with the following parameters:

- `moduleId` — the module identifier where the method will create the field, a required parameter. The SPA module is `crm`.

- `field[entityId]` — the object identifier following the formula `CRM_ + {ID}`, where `ID` is the sequential number of the SPA in Bitrix24 from the [crm.type.list](./how-to-add-user-field-to-spa.md#spa-id) result, a required parameter. In the example, we will specify `CRM_7`.

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
        module_id="crm",
        field={
            "entityId": "CRM_7",
            "fieldName": "UF_CRM_7_NEW_REST_LIST",
            "userTypeId": "enumeration",
            "multiple": "Y",
            "editFormLabel": {
                "de": "List of characteristics",
            },
            "enum": [
                {
                    "value": "Characteristic 1",
                    "def": "N",
                    "sort": 100,
                },
                {
                    "value": "Characteristic 2",
                    "def": "Y",
                    "sort": 200,
                },
            ],
        },
    ).response.result["field"]
    ```

{% endlist %}

As a result, you will receive the data for the created field.

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
    },
}
```

## Code Example

{% list tabs %}

- JS
  
    ```JavaScript
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
            var spaId = result.getData().result.types[0].id; // Using the id from the result
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
                        fieldName: 'UF_CRM_' + spaId + '_NEW_REST_LIST', // Using the id
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
                $spaId = $types[0]->id; // Using the id from the result
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
                    'fieldName' => 'UF_CRM_' . $spaId . '_NEW_REST_LIST', // Using the id
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
            spa_id = int(types[0]["id"])
            add_user_field(client, spa_id)
        else:
            print("Smart process not found.")

    def add_user_field(client, spa_id):
        try:
            result = client.userfieldconfig.add(
                module_id="crm",
                field={
                    "entityId": f"CRM_{spa_id}",
                    "fieldName": f"UF_CRM_{spa_id}_NEW_REST_LIST",
                    "userTypeId": "enumeration",
                    "multiple": "Y",
                    "editFormLabel": {
                        "de": "List of characteristics",
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
