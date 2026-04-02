# Set Common Lead Card for All Users crm.lead.details.configuration.forceCommonScopeForAll

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Allow to modify settings" access permission in CRM

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [crm.item.details.configuration.forceCommonScopeForAll](../../universal/item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md).

{% endnote %}

The method `crm.lead.details.configuration.forceCommonScopeForAll` forcibly sets a common lead card for all users, removing their personal lead card settings.

{% note warning %}

Settings for repeat leads may differ from those for simple leads. To switch between lead card settings, use the `leadCustomerType` parameter.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **extras**
[`object`](../../../data-types.md) | Additional parameters for selecting the lead type. The structure is described [below](#extras) ||
|#

### Extras Parameter {#extras}

#|
|| **Name**
`type` | **Description** ||
|| **leadCustomerType**
[`integer`](../../../data-types.md) | Lead type. Possible values:
- `1` - simple lead
- `2` - repeat lead ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"extras":{"leadCustomerType":2}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.details.configuration.forceCommonScopeForAll
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"extras":{"leadCustomerType":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.details.configuration.forceCommonScopeForAll
  ```

- JS

  ```js
  try
  {
      const response = await $b24.callMethod(
          'crm.lead.details.configuration.forceCommonScopeForAll',
          {
              extras: {
                  leadCustomerType: 2,
              },
          }
      );

      const result = response.getData().result;
      console.info(result);
  }
  catch (error)
  {
      console.error(error);
  }
  ```

- PHP

  ```php
  try {
      $response = $b24Service
          ->core
          ->call(
              'crm.lead.details.configuration.forceCommonScopeForAll',
              [
                  'extras' => [
                      'leadCustomerType' => 2,
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
      'crm.lead.details.configuration.forceCommonScopeForAll',
      {
          extras: {
              leadCustomerType: 2
          }
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
      'crm.lead.details.configuration.forceCommonScopeForAll',
      [
          'extras' => [
              'leadCustomerType' => 2,
          ],
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
        "start": 1720687903.685834,
        "finish": 1720687904.076471,
        "duration": 0.3906371593475342,
        "processing": 0.02508091926574707,
        "date_start": "2024-07-11T10:51:43+02:00",
        "date_finish": "2024-07-11T10:51:44+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Returns `true` if the operation was successful ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | Access denied | Insufficient permissions to forcibly set the common lead card ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-lead-details-configuration-get.md)
- [{#T}](./crm-lead-details-configuration-reset.md)
- [{#T}](./crm-lead-details-configuration-set.md)