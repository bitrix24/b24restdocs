# Add Custom Activity Type crm.activity.type.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

The method `crm.activity.type.add` registers a custom activity type by specifying a name and an icon.

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
[`string`](../../../../data-types.md) | String value of the activity type, for example `QuickBooks and other similar platforms`. When creating an activity, this field is `PROVIDER_TYPE_ID` ||
|| **NAME**
[`string`](../../../../data-types.md) | Name of the activity type, for example `Activity for QuickBooks` for a deal. Default is an empty string ||
|| **ICON_FILE**
[`attached_diskfile`](../../../../data-types.md) | Icon file for the activity type, described according to [rules](../../../../files/how-to-upload-files.md) ||
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
    -d '{"fields":{"TYPE_ID":"QuickBooks and other similar platforms","NAME":"Activity for QuickBooks","ICON_FILE":"@type-icon","IS_CONFIGURABLE_TYPE":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.type.add
    ```

    After this, it is sufficient to specify your type when creating an activity, and the icon and name will be loaded automatically.
    
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"OWNER_TYPE_ID":1,"OWNER_ID":selectedEntityId,"PROVIDER_ID":"REST_APP","PROVIDER_TYPE_ID":"QuickBooks and other similar platforms","SUBJECT":"New Activity","COMPLETED":"N","RESPONSIBLE_ID":1,"DESCRIPTION":"Description of the new activity"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.activity.type.add',
        params: {
          fields: {
            TYPE_ID: '1C',
            NAME: '1C activity',
            ICON_FILE: document.getElementById('type-icon'), // file input node
            IS_CONFIGURABLE_TYPE: 'N',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activity type added:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addActivityType() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.type.add',
            params: {
              fields: {
                TYPE_ID: '1C',
                NAME: '1C activity',
                ICON_FILE: document.getElementById('type-icon'), // file input node
                IS_CONFIGURABLE_TYPE: 'N',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Activity type added:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addActivityType)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.type.add',
                [
                    'fields' => [
                        'TYPE_ID'            => 'QuickBooks and other similar platforms',
                        'NAME'               => 'Activity for QuickBooks',
                        'ICON_FILE'          => $_FILES['type-icon'], // file input node
                        'IS_CONFIGURABLE_TYPE' => 'N',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding activity type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.activity.type.add",
        {
            fields:
            {
                "TYPE_ID": 'QuickBooks and other similar platforms',
                "NAME": "Activity for QuickBooks",
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

    After this, it is sufficient to specify your type when creating an activity, and the icon and name will be loaded automatically. 

    ```js
    BX24.callMethod(
        'crm.activity.add',
        {
            fields:
            {
                "OWNER_TYPE_ID": 1,
                "OWNER_ID": selectedEntityId,
                "PROVIDER_ID": 'REST_APP',
                "PROVIDER_TYPE_ID": 'QuickBooks and other similar platforms',
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.type.add',
        [
            'fields' => [
                'TYPE_ID' => 'QuickBooks and other similar platforms',
                'NAME' => 'Activity for QuickBooks',
                'ICON_FILE' => $_FILES['type-icon'], // Assuming file input is handled
                'IS_CONFIGURABLE_TYPE' => 'N'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

    After this, it is sufficient to specify your type when creating an activity, and the icon and name will be loaded automatically. 

     ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.add',
        [
            'fields' => [
                'OWNER_TYPE_ID' => 1,
                'OWNER_ID' => $selectedEntityId, // Assuming this variable is defined
                'PROVIDER_ID' => 'REST_APP',
                'PROVIDER_TYPE_ID' => 'QuickBooks and other similar platforms',
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

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.activity.type.add(
            fields={
                "TYPE_ID": "CUSTOM_CALL",
                "NAME": "Custom call",
                "ICON_FILE": {
                    "fileData": [
                        "activity-type.svg",
                        "PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPjwvc3ZnPg==",
                    ],
                },
                "IS_CONFIGURABLE_TYPE": "N",
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `Access denied! Application context required` | The method works only in the context of applications ||
|| `INVALID_ARG_VALUE` | The required field `TYPE_ID` is not filled ||
|| `INVALID_ARG_VALUE` | A custom activity type with the specified `TYPE_ID` already exists ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-type-list.md)
- [{#T}](./crm-activity-type-delete.md)