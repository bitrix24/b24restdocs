# Add Custom Field task.item.userfield.add

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- missing 1 example (there should be three examples - curl, js, php)
- no success response provided
- no error response provided

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `task.item.userfield.add` creates a new property.

When creating a custom field, the field name `FIELD_NAME` must use the prefix `UF_`. If the prefix is not specified, the system will automatically add it to the beginning of the name.

## Parameters

#|
|| **Parameter** / **Type**| **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|| **PARAMS**
[`unknown`](../../data-types.md) | Array with property parameters in the form `array("parameter": 'value' [, ...])`, containing the following parameters: 
- `USER_TYPE_ID` - data type of the custom field. Allowed values: 
  - `string` — string
  - `double` — number
  - `date` — date
  - `boolean` — yes/no  
- `FIELD_NAME` - field code; 
- `XML_ID` - external code; 
- `EDIT_FORM_LABEL` - label in the formatting form (specified in English ('en') and German ('de') languages); 
- `LABEL` - field title. ||
|#

## Examples

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.userfield.add" \
        -H "Content-Type: application/json" \
        -d '{
            "PARAMS": {
                "USER_TYPE_ID": "string",
                "FIELD_NAME": "NEW_TASKS_FIELD",
                "XML_ID": "MY_TASK_FIELD",
                "EDIT_FORM_LABEL": {
                    "en": "New task field",
                    "de": "Neues Aufgabenfeld"
                },
                "LABEL": "New task field"
            }
        }'
    ```

- cURL (OAuth)

    ```bash
    curl -X POST "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.item.userfield.add" \
        -H "Content-Type: application/json" \
        -d '{
            "PARAMS": {
                "USER_TYPE_ID": "string",
                "FIELD_NAME": "NEW_TASKS_FIELD",
                "XML_ID": "MY_TASK_FIELD",
                "EDIT_FORM_LABEL": {
                    "en": "New task field",
                    "de": "Neues Aufgabenfeld"
                },
                "LABEL": "New task field"
            },
            "auth": "**put_access_token_here**"
        }'
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.userfield.add',
        {
            PARAMS:
            {
                'USER_TYPE_ID' : 'string',
                'FIELD_NAME' : 'NEW_TASKS_FIELD',
                'XML_ID' : 'MY_TASK_FIELD',
                'EDIT_FORM_LABEL' : {'en':'New task field', 'de':'Neues Aufgabenfeld'},
                'LABEL' : 'New task field'
            }
        },
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.item.userfield.add',
        [
            'PARAMS' => [
                'USER_TYPE_ID' => 'string',
                'FIELD_NAME' => 'NEW_TASKS_FIELD',
                'XML_ID' => 'MY_TASK_FIELD',
                'EDIT_FORM_LABEL' => [
                    'en' => 'New task field',
                    'de' => 'Neues Aufgabenfeld',
                ],
                'LABEL' => 'New task field',
            ],
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}