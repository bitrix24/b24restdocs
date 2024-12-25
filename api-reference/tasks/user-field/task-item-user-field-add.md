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

Some data may be missing here — we will complete it soon

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
[`unknown`](../../data-types.md) | An array with property parameters of the form `array("parameter": 'value' [, ...])`, containing the following parameters: 
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

- cURL

    ```bash
    $appParams = array(
        'auth' => 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
        'PARAMS' => array(
            'USER_TYPE_ID' => 'string',
            'FIELD_NAME' => 'NEW_TASKS_FIELD',
            'XML_ID' => 'MY_TASK_FIELD',
            'EDIT_FORM_LABEL' => array(
                'en' => 'New task field',
                'de' => 'Neues Aufgabenfeld'
            ),
            'LABEL' => 'New task field'
        ),
    );
    $request = 'http://your-domain.com/rest/task.item.userfield.add.xml?' . http_build_query($appParams);
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

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}