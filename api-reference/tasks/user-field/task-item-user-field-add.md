# Add Custom Field to Task task.item.userfield.add

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
> Who can execute the method: any user

The method `task.item.userfield.add` creates a new property.

The system limitation on the field name is 20 characters. The prefix `UF_TASK_` is always added to the custom field name, meaning the actual length of the name is 12 characters.

## Parameters

#|
|| **Parameter** / **Type**| **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|| **PARAMS**
[`unknown`](../../data-types.md) | An array with property parameters in the form `array("parameter": 'value' [, ...])`, containing the following parameters: 
- `USER_TYPE_ID` - the data type of the custom field. Allowed values: 
    - `string` (**String**);
    - `double` (**Number**); 
    - `date` (**Date**);
    - `boolean` (**Yes/No**); 
- `FIELD_NAME` - field code; 
- `XML_ID` - external code; 
- `EDIT_FORM_LABEL` - label in the formatting form (specified in English ('en') and Russian ('ru') languages); 
- `LABEL` - field title. ||
|#

## Examples

{% list tabs %}

- cURL

    ```http
    $appParams = array(
        'auth' => 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
        'PARAMS' => array(
            'USER_TYPE_ID' => 'string',
            'FIELD_NAME' => 'NEW_TASKS_FIELD',
            'XML_ID' => 'MY_TASK_FIELD',
            'EDIT_FORM_LABEL' => array(
                'en' => 'New task field'
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
                'EDIT_FORM_LABEL' : {'en':'New task field'},
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

{% include [Footnote on examples](../../../_includes/examples.md) %}