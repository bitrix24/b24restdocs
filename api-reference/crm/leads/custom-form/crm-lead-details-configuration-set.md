# Set Parameters for crm.lead.details.configuration.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
>  - any user can set their own settings,
>  - a user with the "Allow to change settings" access permission in CRM can set others' and shared settings.

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.details.configuration.set](../../universal/item-details-configuration/crm-item-details-configuration-set.md).

{% endnote %}

The method `crm.lead.details.configuration.set` sets the settings for lead cards.

{% note warning %}

The settings for repeat leads may differ from those for simple leads. To switch between lead card settings, use the `leadCustomerType` parameter.

{% endnote %}

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **data***
[`section[]`](#section) | A list of sections in the card. Each section contains a set of fields that will be displayed in the lead card. ||
|| **userId**
[`user`](../../../data-types.md) | The identifier of the user for whom to save the personal configuration.

If the parameter is not provided, the `userId` of the user calling the method will be used.

This is only required when setting personal settings. ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings. Possible values:
- `'P'` - personal settings
- `'C'` - shared settings

The default value is `'P'`. ||
|| **extras**
[`object`](../../../data-types.md) | Additional parameters for selecting the lead type. The structure is described [below](#extras) ||
|#

### Parameter section {#section}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Unique name of the section. ||
|| **title***
[`string`](../../../data-types.md) | Title of the section. ||
|| **type***
[`string`](../../../data-types.md) | Type of the section. Only the value `'section'` is supported. ||
|| **elements**
[`section_element[]`](#section_element) | A list of fields displayed in the section. ||
|#

### Parameter section_element {#section_element}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Identifier of the lead field. A list of available fields can be obtained using the [crm.lead.fields](../crm-lead-fields.md) method. ||
|| **optionFlags**
[`integer`](../../../data-types.md) | Whether to always show the field:
- `1` - yes
- `0` - no

The default value is `0`. ||
|| **options**
[`object`](../../../data-types.md) | Additional options for the field. The set of options depends on the field type. ||
|#

### Parameter extras {#extras}

#|
|| **Name**
`type` | **Description** ||
|| **leadCustomerType**
[`integer`](../../../data-types.md) | Type of lead. Possible values:
- `1` - simple lead
- `2` - repeat lead ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"scope":"P","userId":1,"extras":{"leadCustomerType":2},"data":[{"name":"main","title":"About the Lead","type":"section","elements":[{"name":"TITLE"},{"name":"STATUS_ID"},{"name":"SOURCE_ID"},{"name":"NAME"},{"name":"PHONE","optionFlags":1}]},{"name":"additional","title":"Additional Information","type":"section","elements":[{"name":"ASSIGNED_BY_ID"},{"name":"COMMENTS"}]}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.details.configuration.set
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"scope":"P","userId":1,"extras":{"leadCustomerType":2},"data":[{"name":"main","title":"About the Lead","type":"section","elements":[{"name":"TITLE"},{"name":"STATUS_ID"},{"name":"SOURCE_ID"},{"name":"NAME"},{"name":"PHONE","optionFlags":1}]},{"name":"additional","title":"Additional Information","type":"section","elements":[{"name":"ASSIGNED_BY_ID"},{"name":"COMMENTS"}]}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.details.configuration.set
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
      method: 'crm.lead.details.configuration.set',
      params: {
        scope: 'P',
        userId: 1,
        extras: {
          leadCustomerType: 2,
        },
        data: [
          {
            name: 'main',
            title: 'About lead',
            type: 'section',
            elements: [
              { name: 'TITLE' },
              { name: 'STATUS_ID' },
              { name: 'SOURCE_ID' },
              { name: 'NAME' },
              { name: 'PHONE', optionFlags: 1 },
            ],
          },
          {
            name: 'additional',
            title: 'Additional',
            type: 'section',
            elements: [
              { name: 'ASSIGNED_BY_ID' },
              { name: 'COMMENTS' },
            ],
          },
        ],
      },
      requestId: Text.getUuidRfc4122()
    })

    // The payload is available only on a successful response
    if (!response.isSuccess) {
      console.error(response.getErrorMessages().join('; '))
    } else {
      const result = response.getData()!.result
      console.info('Configuration saved:', result)
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
    async function setLeadCardConfiguration() {
      try {
        // Initialize the SDK inside a Bitrix24 frame
        const $b24 = await B24Js.initializeB24Frame()

        const response = await $b24.actions.v2.call.make({
          method: 'crm.lead.details.configuration.set',
          params: {
            scope: 'P',
            userId: 1,
            extras: {
              leadCustomerType: 2,
            },
            data: [
              {
                name: 'main',
                title: 'About lead',
                type: 'section',
                elements: [
                  { name: 'TITLE' },
                  { name: 'STATUS_ID' },
                  { name: 'SOURCE_ID' },
                  { name: 'NAME' },
                  { name: 'PHONE', optionFlags: 1 },
                ],
              },
              {
                name: 'additional',
                title: 'Additional',
                type: 'section',
                elements: [
                  { name: 'ASSIGNED_BY_ID' },
                  { name: 'COMMENTS' },
                ],
              },
            ],
          },
          requestId: B24Js.Text.getUuidRfc4122()
        })

        // The payload is available only on a successful response
        if (!response.isSuccess) {
          console.error(response.getErrorMessages().join('; '))
          return
        }

        const result = response.getData().result
        console.info('Configuration saved:', result)
      } catch (error) {
        // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
        console.error(error)
      }
    }

    document.addEventListener('DOMContentLoaded', setLeadCardConfiguration)
  </script>
  ```

- PHP

  ```php
  try {
      $response = $b24Service
          ->core
          ->call(
              'crm.lead.details.configuration.set',
              [
                  'scope' => 'P',
                  'userId' => 1,
                  'extras' => [
                      'leadCustomerType' => 2,
                  ],
                  'data' => [
                      [
                          'name' => 'main',
                          'title' => 'About the Lead',
                          'type' => 'section',
                          'elements' => [
                              ['name' => 'TITLE'],
                              ['name' => 'STATUS_ID'],
                              ['name' => 'SOURCE_ID'],
                              ['name' => 'NAME'],
                              ['name' => 'PHONE', 'optionFlags' => 1],
                          ],
                      ],
                      [
                          'name' => 'additional',
                          'title' => 'Additional Information',
                          'type' => 'section',
                          'elements' => [
                              ['name' => 'ASSIGNED_BY_ID'],
                              ['name' => 'COMMENTS'],
                          ],
                      ],
                  ],
              ]
          );

      $result = $response
          ->getResponseData()
          ->getResult();

      echo 'Result: ' . print_r($result, true);
  } catch (Throwable $e) {
      error_log($e->getMessage());
      echo 'Error: ' . $e->getMessage();
  }
  ```

- BX24.js

  ```javascript
  BX24.callMethod(
      'crm.lead.details.configuration.set',
      {
          scope: 'P',
          userId: 1,
          extras: {
              leadCustomerType: 2
          },
          data: [
              {
                  name: 'main',
                  title: 'About the Lead',
                  type: 'section',
                  elements: [
                      { name: 'TITLE' },
                      { name: 'STATUS_ID' },
                      { name: 'SOURCE_ID' },
                      { name: 'NAME' },
                      { name: 'PHONE', optionFlags: 1 }
                  ]
              },
              {
                  name: 'additional',
                  title: 'Additional Information',
                  type: 'section',
                  elements: [
                      { name: 'ASSIGNED_BY_ID' },
                      { name: 'COMMENTS' }
                  ]
              }
          ]
      },
      function(result)
      {
          if (result.error())
          {
              console.error(result.error());
          }
          else
          {
              console.log(result.data());
          }
      }
  );
  ```

- PHP CRest

  ```php
  require_once('crest.php');

  $result = CRest::call(
      'crm.lead.details.configuration.set',
      [
          'scope' => 'P',
          'userId' => 1,
          'extras' => [
              'leadCustomerType' => 2,
          ],
          'data' => [
              [
                  'name' => 'main',
                  'title' => 'About the Lead',
                  'type' => 'section',
                  'elements' => [
                      ['name' => 'TITLE'],
                      ['name' => 'STATUS_ID'],
                      ['name' => 'SOURCE_ID'],
                      ['name' => 'NAME'],
                      ['name' => 'PHONE', 'optionFlags' => 1],
                  ],
              ],
              [
                  'name' => 'additional',
                  'title' => 'Additional Information',
                  'type' => 'section',
                  'elements' => [
                      ['name' => 'ASSIGNED_BY_ID'],
                      ['name' => 'COMMENTS'],
                  ],
              ],
          ],
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
        bitrix_response = client.crm.lead.details.configuration.set(
            scope="P",
            user_id=1,
            extras={"leadCustomerType": 2},
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
        "start": 1720728468.828951,
        "finish": 1720728469.214046,
        "duration": 0.38509488105773926,
        "processing": 0.018099069595336914,
        "date_start": "2024-07-11T22:54:28+02:00",
        "date_finish": "2024-07-11T22:54:29+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The root element of the response. Returns `true` if the settings were successfully saved. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Element at index 0 in section at index 1 does not have name."
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | Access denied | Insufficient permissions. ||
|| `-` | Parameter 'data' must be array | The `data` parameter is not an array. ||
|| `-` | The data must be indexed array | The `data` parameter is not an indexed array. ||
|| `-` | There are no data to write | An empty array was passed in `data`. ||
|| `-` | Section at index `i` has type `data[i].type`. The expected type is 'section' | A value other than `'section'` was passed in `data[i].type`. ||
|| `-` | Section at index `i` does not have name | An empty value was passed in `data[i].name`. ||
|| `-` | Section at index `i` does not have title | An empty value was passed in `data[i].title`. ||
|| `-` | Element at index `j` in section at index `i` does not have name | An empty value was passed in `data[i].elements[j].name`. ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-lead-details-configuration-get.md)
- [{#T}](./crm-lead-details-configuration-reset.md)
- [{#T}](./crm-lead-details-configuration-force-common-scope-for-all.md)