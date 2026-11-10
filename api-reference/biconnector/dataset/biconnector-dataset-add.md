# Create Dataset biconnector.dataset.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

{% note warning "DEPRECATED" %}

Development of the method has been discontinued. Use [biconnector.table.add](../table/biconnector-table-add.md).

{% endnote %}

The `biconnector.dataset.add` method creates a new dataset linked to a data source.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and only with the sources that the application created itself. When called via a webhook, the method returns the `ACCESS_DENIED` error

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object containing data to create a new dataset. The object format:

```
{
    "field_1": "value_1",
    "field_2": "value_2",
    ...,
    "field_n": "value_n"
}
```

- `field_n` — field name
- `value_n` — field value

[Detailed description below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Dataset name. The name must start with a letter and may contain only lowercase Latin letters `a-z`, digits, and the `_` sign. The maximum name length is 230 characters ||
|| **externalName***
[`string`](../../data-types.md) | Name of the dataset in the external source, in the application. The maximum length is 512 characters ||
|| **externalCode***
[`string`](../../data-types.md) | Unique code of the dataset in the external source, used when selecting data. The maximum length is 512 characters ||
|| **sourceId***
[`integer`](../../data-types.md) | Source identifier, can be obtained with the [biconnector.source.list](../source/biconnector-source-list.md) or [biconnector.source.add](../source/biconnector-source-add.md) method. The source must belong to the connector of the current application, otherwise the method returns `SOURCE_NOT_FOUND` ||
|| **verboseName**
[`string`](../../data-types.md) | User-friendly dataset name, for example, `Orders`. The maximum length is 255 characters ||
|| **description**
[`string`](../../data-types.md) | Description of the dataset ||
|| **tableGroupIds**
[`array`](../../data-types.md) | Array of identifiers of the table groups to add the dataset table to. Each identifier is a positive integer, and the group must exist.

If the parameter is not passed or an empty array is passed, the table is added to the user tables group.

The list of table groups cannot be retrieved via the REST API. The [biconnector.dataset.get](./biconnector-dataset-get.md) and [biconnector.dataset.list](./biconnector-dataset-list.md) methods do not return the groups ||
|| **fields***
[`array`](../../data-types.md) | Array of dataset fields, [(detailed description)](#field) ||
|#

### Element of the fields array {#field}

Each element of the `fields` array is an object with three required fields and an optional field description. Field visibility is not set on creation: all fields are created visible and can be hidden later with the [biconnector.dataset.fields.update](./biconnector-dataset-fields-update.md) method.

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Field name. The name must start with a letter and may contain only uppercase Latin letters `A-Z`, digits, and the `_` sign. The maximum name length is 32 characters ||
|| **externalCode***
[`string`](../../data-types.md) | External code of the field — the name the application knows the field by. Bitrix24 passes exactly this code in the data request ||
|| **type***
[`string`](../../data-types.md) | Data type of the field. Allowed values:
`int` — integer
`string` — string
`double` — float, dot separator
`date` — date, format `Y-m-d`
`datetime` — date with time, format `Y-m-d H:i:s`
`money` — monetary value, retained as a number, the currency is not retained
`timezone` — time zone identifier

The value is case-sensitive: an uppercase `INT` causes the `VALIDATION_FIELD_INVALID_TYPE` error ||
|| **description**
[`string`](../../data-types.md) | Field description. If the parameter is not passed, the description stays empty ||
|#

Field names and external codes must not repeat within one request: on a repeated `name` the method returns the `DUPLICATE_FIELDS` error, and on a repeated `externalCode` — `VALIDATION_DUPLICATE_FIELD_CODE`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "fields": {
            "sourceId": 3,
            "name": "rest_dataset",
            "externalName": "Sales orders",
            "externalCode": "sales_orders",
            "description": "Dataset description",
            "fields": [
                { "type": "int", "name": "ID", "externalCode": "ID" },
                { "type": "string", "name": "NAME", "externalCode": "NAME" },
                { "type": "string", "name": "SURNAME", "externalCode": "SURNAME" },
                { "type": "double", "name": "SCORE", "externalCode": "SCORE" },
                { "type": "date", "name": "DATA", "externalCode": "DATA" },
                { "type": "datetime", "name": "TIME", "externalCode": "TIME" }
            ]
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/biconnector.dataset.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Methods of this section put errors inside result and answer with HTTP 200
    type BiconnectorError = {
      error: {
        error: string
        error_description: string
      }
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DatasetAddResult = {
      id: number
    }

    try {
      const response = await $b24.actions.v2.call.make<DatasetAddResult | BiconnectorError>({
        method: 'biconnector.dataset.add',
        params: {
          fields: {
            sourceId: 3,
            name: 'rest_dataset',
            externalName: 'Sales orders',
            externalCode: 'sales_orders',
            description: 'Dataset description',
            fields: [
              { type: 'int', name: 'ID', externalCode: 'ID' },
              { type: 'string', name: 'NAME', externalCode: 'NAME' },
              { type: 'string', name: 'SURNAME', externalCode: 'SURNAME' },
              { type: 'double', name: 'SCORE', externalCode: 'SCORE' },
              { type: 'date', name: 'DATA', externalCode: 'DATA' },
              { type: 'datetime', name: 'TIME', externalCode: 'TIME' },
            ],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result

        // The SDK sees HTTP 200 as success, so check the error inside result yourself
        if ('error' in result) {
          console.error(result.error.error, result.error.error_description)
        } else {
          console.info('Created dataset id:', result.id)
        }
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
      async function addDataset() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.dataset.add',
            params: {
              fields: {
                sourceId: 3,
                name: 'rest_dataset',
                externalName: 'Sales orders',
                externalCode: 'sales_orders',
                description: 'Dataset description',
                fields: [
                  { type: 'int', name: 'ID', externalCode: 'ID' },
                  { type: 'string', name: 'NAME', externalCode: 'NAME' },
                  { type: 'string', name: 'SURNAME', externalCode: 'SURNAME' },
                  { type: 'double', name: 'SCORE', externalCode: 'SCORE' },
                  { type: 'date', name: 'DATA', externalCode: 'DATA' },
                  { type: 'datetime', name: 'TIME', externalCode: 'TIME' },
                ],
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

          // The SDK sees HTTP 200 as success, so check the error inside result yourself
          if (result && result.error) {
            console.error(result.error.error, result.error.error_description)
            return
          }

          console.info('Created dataset id:', result.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addDataset)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.biconnector.dataset.add(
            fields={
                "sourceId": 3,
                "name": "rest_dataset",
                "externalName": "Sales orders",
                "externalCode": "sales_orders",
                "description": "Dataset description",
                "fields": [
                    {
                        "type": "int",
                        "name": "ID",
                        "externalCode": "ID",
                    },
                    {
                        "type": "string",
                        "name": "NAME",
                        "externalCode": "NAME",
                    },
                    {
                        "type": "string",
                        "name": "SURNAME",
                        "externalCode": "SURNAME",
                    },
                    {
                        "type": "double",
                        "name": "SCORE",
                        "externalCode": "SCORE",
                    },
                    {
                        "type": "date",
                        "name": "DATA",
                        "externalCode": "DATA",
                    },
                    {
                        "type": "datetime",
                        "name": "TIME",
                        "externalCode": "TIME",
                    },
                ],
            },
        ).response
        result = bitrix_response.result

        # Methods of this section put errors inside result and answer with HTTP 200
        if isinstance(result, dict) and "error" in result:
            print(
                "BIconnector error",
                f"error: {result['error']['error']}",
                f"error_description: {result['error']['error_description']}",
                sep="\n",
            )
        else:
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
        $response = $b24Service
            ->core
            ->call(
                'biconnector.dataset.add',
                [
                    'fields' => [
                        'sourceId'      => 3,
                        'name'          => 'rest_dataset',
                        'externalName'  => 'Sales orders',
                        'externalCode'  => 'sales_orders',
                        'description'   => 'Dataset description',
                        'fields'        => [
                            ['type' => 'int', 'name' => 'ID', 'externalCode' => 'ID'],
                            ['type' => 'string', 'name' => 'NAME', 'externalCode' => 'NAME'],
                            ['type' => 'string', 'name' => 'SURNAME', 'externalCode' => 'SURNAME'],
                            ['type' => 'double', 'name' => 'SCORE', 'externalCode' => 'SCORE'],
                            ['type' => 'date', 'name' => 'DATA', 'externalCode' => 'DATA'],
                            ['type' => 'datetime', 'name' => 'TIME', 'externalCode' => 'TIME'],
                        ],
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
            $data = $result->data();

            // Methods of this section put errors inside result and answer with HTTP 200
            if (isset($data['error'])) {
                echo 'BIconnector error: ' . $data['error']['error'] . ': ' . $data['error']['error_description'];
            } else {
                echo 'Success: ' . print_r($data, true);
            }
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding dataset: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.dataset.add',
        {
            fields: {
                "sourceId": 3,
                "name": "rest_dataset",
                "externalName": "Sales orders",
                "externalCode": "sales_orders",
                "description": "Dataset description",
                "fields": [
                    { "type": "int", "name": "ID", "externalCode": "ID" },
                    { "type": "string", "name": "NAME", "externalCode": "NAME" },
                    { "type": "string", "name": "SURNAME", "externalCode": "SURNAME" },
                    { "type": "double", "name": "SCORE", "externalCode": "SCORE" },
                    { "type": "date", "name": "DATA", "externalCode": "DATA" },
                    { "type": "datetime", "name": "TIME", "externalCode": "TIME" }
                ]
            }
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            const data = result.data();

            // Methods of this section put errors inside result and answer with HTTP 200
            if (data && data.error) {
                console.error(data.error.error, data.error.error_description);
                return;
            }

            console.info(data);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.dataset.add',
        [
            'fields' => [
                'sourceId' => 3,
                'name' => 'rest_dataset',
                'externalName' => 'Sales orders',
                'externalCode' => 'sales_orders',
                'description' => 'Dataset description',
                'fields' => [
                    [ 'type' => 'int', 'name' => 'ID', 'externalCode' => 'ID' ],
                    [ 'type' => 'string', 'name' => 'NAME', 'externalCode' => 'NAME' ],
                    [ 'type' => 'string', 'name' => 'SURNAME', 'externalCode' => 'SURNAME' ],
                    [ 'type' => 'double', 'name' => 'SCORE', 'externalCode' => 'SCORE' ],
                    [ 'type' => 'date', 'name' => 'DATA', 'externalCode' => 'DATA' ],
                    [ 'type' => 'datetime', 'name' => 'TIME', 'externalCode' => 'TIME' ]
                ]
            ]
        ]
    );

    // Methods of this section put errors inside result and answer with HTTP 200
    if (isset($result['result']['error'])) {
        echo 'BIconnector error: ' . $result['result']['error']['error']
            . ': ' . $result['result']['error']['error_description'];
    } else {
        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "biconnector.dataset.add", b24.Params{
    	"fields": b24.Params{
    		"sourceId":     3,
    		"name":         "rest_dataset",
    		"externalName": "Sales orders",
    		"externalCode": "sales_orders",
    		"description":  "Dataset description",
    		"fields": []b24.Params{
    			{
    				"type":         "int",
    				"name":         "ID",
    				"externalCode": "ID",
    			},
    			{
    				"type":         "string",
    				"name":         "NAME",
    				"externalCode": "NAME",
    			},
    			{
    				"type":         "string",
    				"name":         "SURNAME",
    				"externalCode": "SURNAME",
    			},
    			{
    				"type":         "double",
    				"name":         "SCORE",
    				"externalCode": "SCORE",
    			},
    			{
    				"type":         "date",
    				"name":         "DATA",
    				"externalCode": "DATA",
    			},
    			{
    				"type":         "datetime",
    				"name":         "TIME",
    				"externalCode": "TIME",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("biconnector.dataset.add: %w", err)
    }

    // Methods of this section put errors inside result and answer with HTTP 200.
    var apiErr struct {
    	Error *struct {
    		Error       string `json:"error"`
    		Description string `json:"error_description"`
    	} `json:"error"`
    }
    if err := json.Unmarshal(res.Result, &apiErr); err == nil && apiErr.Error != nil {
    	return fmt.Errorf("biconnector.dataset.add: %s: %s", apiErr.Error.Error, apiErr.Error.Description)
    }

    var item struct {
    	ID b24.ID `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
      "id": 10
    },
    "time": {
        "start": 1725013197.635808,
        "finish": 1725013198.580873,
        "duration": 0.9450650215148926,
        "processing": 0.6822988986968994,
        "date_start": "2024-08-30T12:19:57+02:00",
        "date_finish": "2024-08-30T12:19:58+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### The result object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the created dataset. Use it in the [biconnector.dataset.get](./biconnector-dataset-get.md), [biconnector.dataset.update](./biconnector-dataset-update.md), and [biconnector.dataset.fields.update](./biconnector-dataset-fields-update.md) methods ||
|#

## Error Handling

HTTP status: **200**

```json
{
    "result": {
        "error": {
            "error": "VALIDATION_FIELDS_NOT_PROVIDED",
            "error_description": "Fields not provided."
        }
    }
}
```

{% note warning "" %}

The method returns an error [inside the `result` field](../index.md#errors) and with HTTP status 200. Check `result.error`: the SDK wrappers parse only the top level of the response and treat such an error as a success

{% endnote %}

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | Access denied. | One of the two permissions is missing, BI Builder external data sources are not available on this Bitrix24, or the method was called via a webhook or outside the application context ||
|| `VALIDATION_FIELDS_NOT_PROVIDED` | Fields not provided. | Fields were not passed in the request ||
|| `VALIDATION_UNKNOWN_PARAMETERS` | Unknown parameters: #LIST_OF_PARAMS# | Unknown parameters detected: list ||
|| `VALIDATION_REQUIRED_FIELD_MISSING` | Field "#TITLE#" is required. | Required field #TITLE# was not provided ||
|| `VALIDATION_READ_ONLY_FIELD` | Field "#TITLE#" is read only. | Field #TITLE# is read-only and cannot be modified ||
|| `VALIDATION_INVALID_FIELD_TYPE` | Field "#TITLE#" must be of type #TYPE#. | Field #TITLE# must be of type #TYPE# ||
|| `VALIDATION_INVALID_FIELD_TYPE` | Field "tableGroupIds" must be an array of positive integers. | The `tableGroupIds` array contains a value that is not a positive integer ||
|| `VALIDATION_INVALID_FIELD_TYPE` | Field "description" must be of type string. | A field description in the `fields` array is not a string ||
|| `VALIDATION_TABLE_GROUP_NOT_FOUND` | Table group was not found. | There is no table group with one of the identifiers from `tableGroupIds` ||
|| `BX_INVALID_VALUE` | The length of "VERBOSE_NAME" should not exceed 255 characters. | The `verboseName` value is longer than 255 characters ||
|| `SOURCE_NOT_FOUND` | Source was not found. | The source does not exist or belongs to another application ||
|| `DATASET_ALREADY_EXIST` | Table with this name already exists. | The name is taken by a table that already exists in BI Builder itself ||
|| `VALIDATION_DATASET_NAME_RESERVED` | Dataset name is reserved by a system table. | The dataset name is taken by a BI Builder system table ||
|| `NAME_EXISTS` | A table named "#NAME#" already exists. | The #NAME# name is already taken by another dataset in Bitrix24. The name is checked across the entire Bitrix24, not within the source ||
|| `FIELDS_EMPTY` | $fields is empty | An empty `fields` array was passed ||
|| `DUPLICATE_FIELDS` | Duplicate column names: #FIELD_NAMES#. | The `name` parameter of the fields contains repeats: the list ||
|| `VALIDATION_DATASET_NAME_INVALID` | Dataset name has to start with a lowercase Latin character. Possible entry includes lowercase Latin characters (a-z), numbers (0-9) and underscores. | Invalid format of the dataset name. The name must start with a letter and may contain only lowercase Latin letters `a-z`, digits, and the `_` sign ||
|| `VALIDATION_DATASET_NAME_TOO_LONG` | Dataset name must not exceed 230 characters. | Dataset name must not exceed 230 characters ||
|| `VALIDATION_DUPLICATE_FIELD_CODE` | Duplicate values found in the "code" parameter: #LIST_CODES# | Duplicates found in the `externalCode` parameter of dataset fields ||
|| `VALIDATION_FIELD_MISSING_REQUIRED_PARAMETERS` | Field must include the required parameters: "name", "externalCode" and "type". | Field must include the parameters `name`, `externalCode`, and `type` ||
|| `VALIDATION_FIELD_NAME_INVALID_FORMAT` | Field "name" has to start with an uppercase Latin character. Possible entry includes uppercase Latin characters (A-Z), numbers (0-9) and underscores. | Invalid format of the field name. The name must start with a letter and may contain only uppercase Latin letters `A-Z`, digits, and the `_` sign ||
|| `VALIDATION_FIELD_NAME_TOO_LONG` | Field "name" must not exceed 32 characters. | The field name must not exceed 32 characters ||
|| `VALIDATION_FIELD_INVALID_TYPE` | Invalid field type. | Invalid field type ||
|| Empty value | Error creating table. or Error adding dataset | The dataset could not be created in BI Builder. This error has no string code of its own: the `error` field carries a numeric zero. No table is left behind — the record that was already created is deleted ||
|#

{% note info "" %}

The method creates two objects at once: a table — the same object as [biconnector.table.add](../table/biconnector-table-add.md) creates — and a BI Builder dataset on top of it. If the second step fails, the first one is rolled back: no partially created table is left. The [biconnector.table.add](../table/biconnector-table-add.md) method has no such step, so it never returns this error

{% endnote %}


{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-dataset-update.md)
- [{#T}](./biconnector-dataset-get.md)
- [{#T}](./biconnector-dataset-list.md)
- [{#T}](./biconnector-dataset-delete.md)
- [{#T}](./biconnector-dataset-fields-update.md)
- [{#T}](./biconnector-dataset-fields.md)
