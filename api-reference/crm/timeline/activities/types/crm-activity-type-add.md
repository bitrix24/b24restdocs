# Add Custom CRM Activity Type crm.activity.type.add

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method `crm.activity.type.add` registers a custom activity type by specifying its name and icon.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../../data-types.md#object_type) | Field values for adding a new custom activity type in the form of a structure:

```json
fields:
{
    "TYPE_ID": 'value',
    "NAME": 'value',
    "ICON_FILE": 'value',
    "IS_CONFIGURABLE_TYPE": 'value',
}
```

A detailed description is provided [below](#parametr-fields)
|#

### Parameter fields {#parametr-fields}

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID***
[`string`](../../../../data-types.md) | String value of the activity type, for example `1C`. When creating an activity, this field is `PROVIDER_TYPE_ID` ||
|| **NAME**
[`string`](../../../../data-types.md) | Name of the activity type, for example `Activity 1C` for a deal. Default is an empty string ||
|| **ICON_FILE**
[`attached_diskfile`](../../../../data-types.md) | Icon file of the activity type, described according to [rules](../../../../bx24-js-sdk/how-to-call-rest-methods/files.md) ||
|| **IS_CONFIGURABLE_TYPE**
[`string`](../../../../data-types.md) | Default value is `N`. Value `Y` indicates that the type will be used for [configurable activities](../configurable/crm-activity-configurable-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":"1C","NAME":"Activity 1C","ICON_FILE":"@type-icon","IS_CONFIGURABLE_TYPE":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.type.add
    ```

    After this, you just need to specify your type when creating an activity, and the icon and name will be loaded automatically.
    
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"OWNER_TYPE_ID":1,"OWNER_ID":selectedEntityId,"PROVIDER_ID":"REST_APP","PROVIDER_TYPE_ID":"1C","SUBJECT":"New Activity","COMPLETED":"N","RESPONSIBLE_ID":1,"DESCRIPTION":"Description of the new activity"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.activity.type.add",
        {
            fields:
            {
                "TYPE_ID": '1C',
                "NAME": "Activity 1C",
                'ICON_FILE': document.getElementById('type-icon'), // file input node
                "IS_CONFIGURABLE_TYPE": "N"
            }
        }, result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

    After this, you just need to specify your type when creating an activity, and the icon and name will be loaded automatically. 

    ```js
    BX24.callMethod(
        'crm.activity.add',
        {
            fields:
            {
                "OWNER_TYPE_ID": 1,
                "OWNER_ID": selectedEntityId,
                "PROVIDER_ID": 'REST_APP',
                "PROVIDER_TYPE_ID": '1C',
                "SUBJECT": "New Activity",
                "COMPLETED": "N",
                "RESPONSIBLE_ID": 1,
                "DESCRIPTION": "Description of the new activity"
            }
        }, result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.type.add',
        [
            'fields' => [
                'TYPE_ID' => '1C',
                'NAME' => 'Activity 1C',
                'ICON_FILE' => $_FILES['type-icon'], // Assuming file input is handled
                'IS_CONFIGURABLE_TYPE' => 'N'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

    After this, you just need to specify your type when creating an activity, and the icon and name will be loaded automatically. 

     ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.add',
        [
            'fields' => [
                'OWNER_TYPE_ID' => 1,
                'OWNER_ID' => $selectedEntityId, // Assuming this variable is defined
                'PROVIDER_ID' => 'REST_APP',
                'PROVIDER_TYPE_ID' => '1C',
                'SUBJECT' => 'New Activity',
                'COMPLETED' => 'N',
                'RESPONSIBLE_ID' => 1,
                'DESCRIPTION' => 'Description of the new activity'
            ]
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
    "result": true,
    "time": {
        "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2025-01-21T13:47:08+02:00",
        "date_finish": "2025-01-21T13:47:08+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | Root element of the response. Contains:
- `true` — in case of success
- `false` — in case of failure (an error occurred)
||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient rights to perform the operation ||
|| `Access denied! Application context required` | The method works only in the context of applications ||
|| `INVALID_ARG_VALUE` | The required field `TYPE_ID` is not filled ||
|| `INVALID_ARG_VALUE` | A custom activity type with the specified `TYPE_ID` already exists ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-type-list.md)
- [{#T}](./crm-activity-type-delete.md)