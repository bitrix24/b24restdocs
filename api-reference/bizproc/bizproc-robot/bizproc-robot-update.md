# Update Fields of the Automation Rule bizproc.robot.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `bizproc.robot.update` method updates the fields of the Automation Rule registered by the application.

It only works in the context of the [application](../../../settings/app-installation/index.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **CODE*** 
[`string`](../../data-types.md) | Code of the Automation Rule that this application passed in `CODE` during registration. You can retrieve the codes using the [bizproc.robot.list](./bizproc-robot-list.md) method ||
|| **FIELDS*** 
[`object`](../../data-types.md) | Object with [fields](#parametr-fields) of the Automation Rule ||
|#

### FIELDS Parameter {#parametr-fields}

How the method updates the fields:

- `FIELDS` must contain at least one field, otherwise the method returns the `No fields to update` error
- `PLACEMENT_HANDLER` does not count as such a field: pass it together with another field, for example, `USE_PLACEMENT: 'Y'`
- fields that are not included in `FIELDS` remain unchanged
- `PROPERTIES` and `RETURN_PROPERTIES` are replaced entirely: to add a parameter, pass the full set

#| 
|| **Name**
`type` | **Description** ||
|| **HANDLER**
[`string`](../../data-types.md) | Handler URL to which Bitrix24 sends the Automation Rule data via the queue server.

It must start with `http://` or `https://`, and the host name must contain a dot, for example, `https://example.com/robot.php` ||
|| **AUTH_USER_ID** 
[`integer`](../../data-types.md) | Identifier of the user whose token Bitrix24 passes to the handler by default. An administrator can select a different user in the Automation Rule settings ||
|| **USE_SUBSCRIPTION** 
[`boolean`](../../data-types.md) | Should the Automation Rule wait for a response from the application? Possible values:
- `Y` — yes
- `N` — no
||
|| **NAME**
[`string` \| `object`](../../data-types.md) | Name of the Automation Rule.

Can be a string or an associative array of localized strings like:

```js
'NAME': {
    'de': 'Automatisierungsregel Name',
    'en': 'Automation rule name',
    ...
},
```

||
|| **DESCRIPTION** 
[`string` \| `object`](../../data-types.md) | Description of the Automation Rule.

Can be a string or an associative array of localized strings like:

```js
'DESCRIPTION': {
    'de': 'Beschreibung der Automatisierungsregel',
    'en': 'Automation rule description',
    ...
},
```
 ||
|| **PROPERTIES** 
[`object`](../../data-types.md) | Object with parameters of the Automation Rule. Contains objects, each describing a [parameter of the Automation Rule](#property).

The system name of the parameter must start with a letter and can contain characters `a-z`, `A-Z`, `0-9`, and underscore `_` ||
|| **RETURN_PROPERTIES** 
[`object`](../../data-types.md) | Object with additional results of the Automation Rule. Contains objects, each describing a [parameter of the Automation Rule](#property).

The application returns the values of these parameters using the [bizproc.event.send](./bizproc-event-send.md) method, and they become available to subsequent steps. Whether to wait for a response is set by the `USE_SUBSCRIPTION` parameter.

The system name of the parameter must start with a letter and can contain characters `a-z`, `A-Z`, `0-9`, and underscore `_`
||
|| **DOCUMENT_TYPE** 
[`array`](../../data-types.md) | Document type that will determine the data types for the `PROPERTIES` and `RETURN_PROPERTIES` parameters. Consists of three string-type elements:
- module identifier
- object identifier
- document type

Possible value options:

- CRM Module
    `['crm', 'CCrmDocumentLead', 'LEAD']` — leads
    `['crm', 'CCrmDocumentDeal', 'DEAL']` — deals
    `['crm', 'Bitrix\Crm\Integration\BizProc\Document\Quote', 'QUOTE']` — estimates
    `['crm', 'Bitrix\Crm\Integration\BizProc\Document\SmartInvoice', 'SMART_INVOICE']` — invoices
    `['crm', 'Bitrix\Crm\Integration\BizProc\Document\Dynamic', 'DYNAMIC_XXX']` — SPAs, where XXX is the identifier of the SPA

||
|| **FILTER** 
[`object`](../../data-types.md) | Object with rules for restricting the Automation Rule by document type and edition.

Can contain keys:
- `INCLUDE` — array of rules where the Automation Rule will be displayed
- `EXCLUDE` — array of rules where the Automation Rule will be hidden

Each rule in the array can be a string or an array of document types in full or partial form.

To restrict the Automation Rule by Bitrix24 edition, specify:
- `b24` — for cloud
- `box` — for on-premise

Examples:
1. Exclude the Automation Rule for on-premise Bitrix24
    ```js
    'FILTER': {
        EXCLUDE: [ 'box' ]
    }
    ```
2. Display the Automation Rule only for deals and leads in CRM
    ```js
    'FILTER': {
        INCLUDE: [
            ['crm', 'CCrmDocumentDeal'],
            ['crm', 'CCrmDocumentLead']
        ]
    }
    ```
||
|| **USE_PLACEMENT** 
[`boolean`](../../data-types.md) | Allows opening additional settings for the Automation Rule in the application slider. Possible values:
- `Y` — yes
- `N` — no ||
|| **PLACEMENT_HANDLER**
[`string`](../../data-types.md) | URL of the placement handler on the application side. It is validated the same way as `HANDLER`.

To enable the placement, pass the URL together with `USE_PLACEMENT: 'Y'`. The `USE_PLACEMENT: 'N'` value removes the handler registration, so you need to pass the URL again when you re-enable the placement ||
|#

### PROPERTY Object {#property}

#| 
|| **Name**
`type` | **Description** ||
|| **Name***
[`string` \| `object`](../../data-types.md) | Name of the parameter. Without it, the method returns the `Empty property NAME` error ||
|| **Description** 
[`string` \| `object`](../../data-types.md) | Description of the parameter ||
|| **Type** 
[`string`](../../data-types.md) | Type of the parameter. Basic values:
  - `bool` — yes or no
  - `date` — date
  - `datetime` — date and time
  - `double` — number
  - `file` — file
  - `int` — integer
  - `select` — list
  - `string` — string
  - `text` — text
  - `user` — user ||
|| **Options** 
[`object`](../../data-types.md) | Options for a list-type parameter, `Type: 'select'`. The key is the option value, and the value is its title:

```js
{
    'value1': 'title1',
    'value2': 'title2',
    'value3': 'title3',
    'value4': 'title4'
}
```
||
|| **Required** 
[`boolean`](../../data-types.md) | Requirement of the parameter. Possible values:
- `Y` — yes
- `N` — no ||
|| **Multiple** 
[`boolean`](../../data-types.md) | Multiplicity of the parameter. Possible values:
- `Y` — yes
- `N` — no ||
|| **Default** 
[`any`](../../data-types.md) | Default value of the parameter. For `Type = 'select'`, specify the key from `Options` ||
|#

#### Examples of Objects

Below are examples of `PROPERTY` objects for different parameter types.

- `select`

  ```js
  'docType': {
      'Name': {
          'de': 'Dokumenttyp',
          'en': 'Document type'
      },
      'Required': 'Y',
      'Multiple': 'N',
      'Default': 'pdf',
      'Type': 'select',
      'Options': {
          'pdf': 'PDF',
          'docx': 'DOCX'
      }
  }
  ```

- `bool`

  ```js
  'saveDoc': {
      'Name': {
          'de': 'Dokument speichern',
          'en': 'Save document'
      },
      'Description': {
          'de': 'Einen fortlaufenden Nummer zuweisen',
          'en': 'Assign a sequential number'
      },
      'Type': 'bool',
      'Required': 'Y',
      'Multiple': 'N',
      'Default': 'Y'
  }
  ```

- `file`

  ```js
  'attachment': {
      'Name': {
          'de': 'Datei',
          'en': 'File'
      },
      'Description': {
          'de': 'Datei zum Senden',
          'en': 'File to send'
      },
      'Type': 'file',
      'Required': 'N',
      'Multiple': 'Y'
  }
  ```

- `string`

  ```js
  'Parameters': {
      'Name': {
          'de': 'Vorlagenparameter',
          'en': 'Template\'s parameters'
      },
      'Description': {
          'de': 'ParamID={=ParamValue}',
          'en': 'ParamID={=ParamValue}'
      },
      'Type': 'string',
      'Required': 'N',
      'Multiple': 'Y'
  }
  ```

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"test_robot","FIELDS":{"NAME":"Send message to author","USE_SUBSCRIPTION":"N","FILTER":{"INCLUDE":[["crm","CCrmDocumentDeal"]]}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.robot.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.actions.v2.call.make({
            method: 'bizproc.robot.update',
            params: {
                'CODE': 'test_robot',
                'FIELDS': {
                    'NAME': 'Send message to author',
                    'USE_SUBSCRIPTION': 'N',
                    'FILTER': {
                        INCLUDE: [
                            ['crm', 'CCrmDocumentDeal']
                        ]
                    }
                }
            }
        });

        if (!response.isSuccess)
            console.error(response.getErrorMessages().join('; '));
        else
            console.log('Success:', response.getData().result);
    }
    catch( error )
    {
        alert("Error: " + error);
    }
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.bizproc.robot.update(
            code="test_robot",
            fields={
                "NAME": "Send a message to the author",
                "USE_SUBSCRIPTION": "N",
                "FILTER": {
                    "INCLUDE": [
                        [
                            "crm",
                            "CCrmDocumentDeal",
                        ],
                    ],
                },
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

- PHP

    ```php
    try {
        $result = $serviceBuilder
            ->getBizProcScope()
            ->robot()
            ->update(
                'robot_code',
                'https://example.com/handler',
                1,
                ['en' => 'Localized Name'],
                true,
                ['property1' => ['Name' => 'Parameter', 'Type' => 'string']],
                false,
                ['outputString' => ['Name' => 'Result', 'Type' => 'string']]
            );

        // Process the result
        if ($result->isSuccess()) {
            print_r($result->getCoreResponse()->getResponseData()->getResult());
        } else {
            print("Update failed.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'bizproc.robot.update',
        {
            'CODE': 'test_robot',
            'FIELDS': {
                'NAME': 'Send message to author',
                'USE_SUBSCRIPTION': 'N',
                'FILTER': {
                    INCLUDE: [
                        ['crm', 'CCrmDocumentDeal']
                    ]
                }
            },
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.robot.update',
        [
            'CODE' => 'test_robot',
            'FIELDS' => [
                'NAME' => 'Send message to author',
                'USE_SUBSCRIPTION' => 'N',
                'FILTER' => [
                    'INCLUDE' => [
                        ['crm', 'CCrmDocumentDeal']
                    ]
                ]
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "bizproc.robot.update", b24.Params{
    	"CODE": "test_robot",
    	"FIELDS": b24.Params{
    		"NAME":             "Send message to author",
    		"USE_SUBSCRIPTION": "N",
    		"FILTER": b24.Params{
    			"INCLUDE": []any{
    				[]string{"crm", "CCrmDocumentDeal"},
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("bizproc.robot.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1738149954.2918739,
        "finish": 1738149954.4590819,
        "duration": 0.16720795631408691,
        "processing": 0.017282962799072266,
        "date_start": "2025-01-29T14:25:54+01:00",
        "date_finish": "2025-01-29T14:25:54+01:00",
        "operating_reset_at": 1738150554,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result** 
[`boolean`](../../data-types.md) | Returns `true` if the Automation Rule was successfully updated ||
|| **time** 
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ACTIVITY_VALIDATION_FAILURE",
    "error_description": "Wrong properties array!"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! Application context required | Application context is required ||
|| `403` | `ACCESS_DENIED` | Access denied! | The method was called by a non-administrator ||
|| `400` | `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty activity code! | Automation Rule code is not specified ||
|| `400` | `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity code! | Invalid Automation Rule code ||
|| `400` | `ERROR_ACTIVITY_NOT_FOUND` | Activity or Robot not found! | Automation Rule not found ||
|| `400` | `ERROR_UNSUPPORTED_PROTOCOL` | Unsupported handler protocol | Invalid handler protocol http, https ||
|| `400` | `ERROR_WRONG_HANDLER_URL` | Wrong handler URL | Invalid handler URL ||
|| `400` | `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong properties array! | The `PROPERTIES` or `RETURN_PROPERTIES` parameters are specified incorrectly ||
|| `400` | `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong property key (\<key\>)! | Invalid property identifier ||
|| `400` | `ERROR_ACTIVITY_VALIDATION_FAILURE` | Empty property NAME (\<key\>)! | Property name not specified ||
|| `400` | `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity FILTER! | Invalid filter ||
|| `400` | `ERROR_ACTIVITY_VALIDATION_FAILURE` | Wrong activity DOCUMENT_TYPE! | Invalid `DOCUMENT_TYPE` ||
|| `400` | `ERROR_ACTIVITY_VALIDATION_FAILURE` | No fields to update | No fields to update ||
|| `400` | `ERROR_CORE` | Unable to set placement handler: Handler already binded | The URL is already used by the settings handler of another Automation Rule or workflow action of this application: each `CODE` must have its own URL ||
|| `400` | `ERROR_CORE` | Unable to set placement handler: \<error text\> | Failed to retain the placement handler ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bizproc-robot-add.md)
- [{#T}](./bizproc-robot-list.md)
- [{#T}](./bizproc-robot-delete.md)
- [{#T}](./bizproc-event-send.md)
