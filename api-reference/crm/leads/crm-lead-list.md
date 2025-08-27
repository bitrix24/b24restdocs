# Get the list of leads crm.lead.list

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with read access permission for leads

The method `crm.lead.list` returns a list of leads based on the filter. It is an implementation of the list method for leads.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | The array contains a list of fields to select (see lead fields [crm-lead-fields](./crm-lead-fields.md)).

When selecting, use masks:
- "*" - to select all fields (excluding custom and multiple fields)
- "UF_*" - to select all custom fields (excluding multiple fields)

There are no masks for selecting multiple fields. To select multiple fields, specify the required ones in the selection list ("PHONE", "EMAIL", etc.).
There is no option to add a logical OR condition to the filter if you need to select by several different fields. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected leads in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to lead fields [crm-lead-fields](./crm-lead-fields.md).

The key can have an additional prefix that clarifies the behavior of the filter. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@` — NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The "%" symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The "%" symbol needs to be passed in the value. Examples:
  - "mol%" — searching for values starting with "mol"
  - "%mol" — searching for values ending with "mol"
  - "%mol%" — searching for values where "mol" can be in any position

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The "%" symbol in the filter value does not need to be passed. The search goes from both sides.

- `=%` — NOT LIKE, substring search. The "%" symbol needs to be passed in the value. Examples:
  - "mol%" — searching for values not starting with "mol"
  - "%mol" — searching for values not ending with "mol"
  - "%mol%" — searching for values where the substring "mol" is not in any position

- `!%=` — NOT LIKE (see description above)

- `=` — equals, exact match (used by default)
- `!=` - not equal
- `!` — not equal
  ||
  || **order**
Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
  || **start**
  [`integer`](../data-types.md) | This parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

Also, see the description of [list methods](../../how-to-call-rest-api/list-methods-pecularities.md).

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

  ```bash
  curl -X POST \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"select":["*","UF_*"],"start":50,"filter":{"=OPPORTUNITY":15000},"order":{"STATUS_ID":"ASC"}}' \
  https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.list
  ```

- cURL (OAuth)

  ```bash
  curl -X POST \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"select":["*","UF_*"],"start":50,"filter":{"=OPPORTUNITY":15000},"order":{"STATUS_ID":"ASC"},"auth":"**put_access_token_here**"}' \
  https://**put_your_bitrix24_address**/rest/crm.lead.list
  ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    try {
      const response = await $b24.callListMethod(
        'crm.lead.list',
        {
          select: ['*', 'UF_*'],
          filter: {
            '=OPPORTUNITY': 15000,
          },
          order: {
            STATUS_ID: 'ASC',
          },
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod is preferred when working with large datasets. The method implements iterative selection using a generator, allowing data to be processed in parts and efficiently using memory.
    
    try {
      const generator = $b24.fetchListMethod('crm.lead.list', {
        select: ['*', 'UF_*'],
        filter: {
          '=OPPORTUNITY': 15000,
        },
        order: {
          STATUS_ID: 'ASC',
        },
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, with large volumes of data, it may be less efficient compared to fetchListMethod.
    
    try {
      const response = await $b24.callMethod('crm.lead.list', {
        select: ['*', 'UF_*'],
        filter: {
          '=OPPORTUNITY': 15000,
        },
        order: {
          STATUS_ID: 'ASC',
        },
      }, 50)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

  ```php      
  try {
      $order = [];
      $filter = []; // Define your filter criteria here
      $select = [
          'ID', 'TITLE', 'HONORIFIC', 'NAME', 'SECOND_NAME', 'LAST_NAME', 
          'BIRTHDATE', 'COMPANY_TITLE', 'SOURCE_ID', 'SOURCE_DESCRIPTION', 
          'STATUS_ID', 'STATUS_DESCRIPTION', 'STATUS_SEMANTIC_ID', 'POST', 
          'ADDRESS', 'ADDRESS_2', 'ADDRESS_CITY', 'ADDRESS_POSTAL_CODE', 
          'ADDRESS_REGION', 'ADDRESS_PROVINCE', 'ADDRESS_COUNTRY', 
          'ADDRESS_COUNTRY_CODE', 'ADDRESS_LOC_ADDR_ID', 'CURRENCY_ID', 
          'OPPORTUNITY', 'IS_MANUAL_OPPORTUNITY', 'OPENED', 'COMMENTS', 
          'HAS_PHONE', 'HAS_EMAIL', 'HAS_IMOL', 'ASSIGNED_BY_ID', 
          'CREATED_BY_ID', 'MODIFY_BY_ID', 'MOVED_BY_ID', 'DATE_CREATE', 
          'DATE_MODIFY', 'MOVED_TIME', 'COMPANY_ID', 'CONTACT_ID', 
          'CONTACT_IDS', 'IS_RETURN_CUSTOMER', 'DATE_CLOSED', 
          'ORIGINATOR_ID', 'ORIGIN_ID', 'UTM_SOURCE', 'UTM_MEDIUM', 
          'UTM_CAMPAIGN', 'UTM_CONTENT', 'UTM_TERM', 'PHONE', 'EMAIL', 
          'WEB', 'IM', 'LINK'
      ];
      $startItem = 0;
      $leadsResult = $serviceBuilder->getCRMScope()->lead()->list($order, $filter, $select, $startItem);
      
      foreach ($leadsResult->getLeads() as $lead) {
          print("ID: {$lead->ID}, TITLE: {$lead->TITLE}, NAME: {$lead->NAME}, BIRTHDATE: " . 
                ($lead->BIRTHDATE ? $lead->BIRTHDATE->format(DATE_ATOM) : 'N/A') . "\n");
      }
  } catch (Throwable $e) {
      print("Error: " . $e->getMessage());
  }
  ```

- BX24.js

    ```javascript 
    BX24.callMethod(
      'crm.lead.list',
      {
        select: ['*', 'UF_*'],
        start: 50,
        filter: {
            '=OPPORTUNITY': 15000,
        },
        order: {
            STATUS_ID: 'ASC',
        }, 
      },
      (result) => {
        if(result.error())
        {
          console.error(result.error());
  
          return;
        }
        
        console.info(result.data());
      }
    );
    ```

- PHP CRest

  ```php
  require_once('crest.php');

  $result = CRest::call(
      'crm.lead.list',
      [
          'select' => ['*', 'UF_*'],
          'start' => 50,
          'filter' => [
              '=OPPORTUNITY' => 15000,
          ],
          'order' => [
              'STATUS_ID' => 'ASC',
          ],
      ]
  );

  echo '<PRE>';
  print_r($result);
  echo '</PRE>';
  ```

{% endlist %}

## Some Practical Examples

{% list tabs %}

- Search for unconverted leads with an amount greater than zero

  ```js
  BX24.callMethod(
      "crm.lead.list",
      {
          order: { "STATUS_ID": "ASC" },
          filter: { ">OPPORTUNITY": 0, "!STATUS_ID": "CONVERTED" },
          select: [ "ID", "TITLE", "STATUS_ID", "OPPORTUNITY", "CURRENCY_ID" ],
      },
      (result) => {
          if(result.error())
          {
              console.error(result.error());
          }
          else
          {
              console.dir(result.data());
              if (result.more())
              {
                  result.next();
              }
          }
      }
  );
  ```

- Search for a lead by phone

  ```js
  BX24.callMethod(
      "crm.lead.list",
      {
          filter: { "PHONE": "555888" },
          select: [ "ID", "TITLE" ]
      },
      (result) => {
        if(result.error())
        {
          console.error(result.error());
        }
        else
        {
          console.dir(result.data());
          if (result.more())
          {
            result.next();
          }
        }
      }
  );
  ```

- Selecting leads for the month

  ```php
  $result = CRest::call(
      'crm.lead.list',
      [
          'filter' => [
              '>DATE_CREATE' => '2023-10-01T00:00:00',
              '<DATE_CREATE' => '2023-10-31T23:59:59',
          ],
          'select' => [
              'ID',
              'DATE_CREATE',
          ],
      ]
  );
  ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
  "result": [
    {
      "ID": "5",
      "TITLE": "Lead 1",
      "HONORIFIC": null,
      "NAME": "Erasmus",
      "SECOND_NAME": null,
      "LAST_NAME": "Golden of Ireland",
      "COMPANY_TITLE": null,
      "COMPANY_ID": "0",
      "CONTACT_ID": "2069",
      "IS_RETURN_CUSTOMER": "N",
      "BIRTHDATE": "",
      "SOURCE_ID": "CALL",
      "SOURCE_DESCRIPTION": null,
      "STATUS_ID": "CONVERTED",
      "STATUS_DESCRIPTION": null,
      "POST": null,
      "COMMENTS": null,
      "CURRENCY_ID": "USD",
      "OPPORTUNITY": "15000.00",
      "IS_MANUAL_OPPORTUNITY": "Y",
      "HAS_PHONE": "Y",
      "HAS_EMAIL": "Y",
      "HAS_IMOL": "N",
      "ASSIGNED_BY_ID": "1",
      "CREATED_BY_ID": "1",
      "MODIFY_BY_ID": "1",
      "DATE_CREATE": "2021-05-31T15:10:16+02:00",
      "DATE_MODIFY": "2021-11-26T18:56:13+02:00",
      "DATE_CLOSED": "2021-07-16T16:43:44+02:00",
      "STATUS_SEMANTIC_ID": "S",
      "OPENED": "Y",
      "ORIGINATOR_ID": null,
      "ORIGIN_ID": null,
      "MOVED_BY_ID": "1",
      "MOVED_TIME": "2021-07-16T16:43:44+02:00",
      "ADDRESS": "7677 Hollow Ridge Alley",
      "ADDRESS_2": null,
      "ADDRESS_CITY": null,
      "ADDRESS_POSTAL_CODE": null,
      "ADDRESS_REGION": null,
      "ADDRESS_PROVINCE": null,
      "ADDRESS_COUNTRY": "Indonesia",
      "ADDRESS_COUNTRY_CODE": null,
      "ADDRESS_LOC_ADDR_ID": "1",
      "UTM_SOURCE": null,
      "UTM_MEDIUM": null,
      "UTM_CAMPAIGN": null,
      "UTM_CONTENT": null,
      "UTM_TERM": null,
      "LAST_ACTIVITY_BY": "1",
      "LAST_ACTIVITY_TIME": "2021-05-31T15:10:16+02:00",
      "UF_CRM_1704817278": null,
      "UF_CRM_1706782596092": null,
      "UF_CRM_1708952993785": false
    },
    {
      "ID": "6",
      "TITLE": "Lead 2",
      "HONORIFIC": null,
      "NAME": "Ignacius",
      "SECOND_NAME": null,
      "LAST_NAME": "Slayny",
      "COMPANY_TITLE": null,
      "COMPANY_ID": "0",
      "CONTACT_ID": "2070",
      "IS_RETURN_CUSTOMER": "N",
      "BIRTHDATE": "",
      "SOURCE_ID": "CALL",
      "SOURCE_DESCRIPTION": null,
      "STATUS_ID": "CONVERTED",
      "STATUS_DESCRIPTION": null,
      "POST": null,
      "COMMENTS": null,
      "CURRENCY_ID": "USD",
      "OPPORTUNITY": "15000.00",
      "IS_MANUAL_OPPORTUNITY": "Y",
      "HAS_PHONE": "Y",
      "HAS_EMAIL": "Y",
      "HAS_IMOL": "N",
      "ASSIGNED_BY_ID": "1",
      "CREATED_BY_ID": "1",
      "MODIFY_BY_ID": "1",
      "DATE_CREATE": "2021-05-31T15:10:16+02:00",
      "DATE_MODIFY": "2021-11-26T18:56:13+02:00",
      "DATE_CLOSED": "2021-07-16T16:43:47+02:00",
      "STATUS_SEMANTIC_ID": "S",
      "OPENED": "Y",
      "ORIGINATOR_ID": null,
      "ORIGIN_ID": null,
      "MOVED_BY_ID": "1",
      "MOVED_TIME": "2021-07-16T16:43:47+02:00",
      "ADDRESS": "35 Mosinee Street",
      "ADDRESS_2": null,
      "ADDRESS_CITY": null,
      "ADDRESS_POSTAL_CODE": null,
      "ADDRESS_REGION": null,
      "ADDRESS_PROVINCE": null,
      "ADDRESS_COUNTRY": "Japan",
      "ADDRESS_COUNTRY_CODE": null,
      "ADDRESS_LOC_ADDR_ID": "2",
      "UTM_SOURCE": null,
      "UTM_MEDIUM": null,
      "UTM_CAMPAIGN": null,
      "UTM_CONTENT": null,
      "UTM_TERM": null,
      "LAST_ACTIVITY_BY": "1",
      "LAST_ACTIVITY_TIME": "2021-05-31T15:10:16+02:00",
      "UF_CRM_1704817278": null,
      "UF_CRM_1706782596092": null,
      "UF_CRM_1708952993785": true
    },
    
      48 more leads with similar structure
    
  ],
  "next": 50,
  "total": 654,
  "time": {
    "start": 1718292234.554781,
    "finish": 1718292234.657739,
    "duration": 0.10295796394348145,
    "processing": 0.05574321746826172,
    "date_start": "2024-06-13T18:23:54+02:00",
    "date_finish": "2024-06-13T18:23:54+02:00",
    "operating": 0
  }
}
```

### Returned Data

#|
|| **Name**
`type`  | **Description** ||
|| **result**
[`array`](../../data-types.md) | The root element of the response. Contains an array of objects that hold information about the fields of deals. 

It should be noted that the structure of the fields may change due to the `select` parameter.

 For information about the structure of a lead, see the method [`crm.lead.get`](./crm-lead-get.md) ||
|| **total**
[`integer`](../../data-types.md) | The total number of found items ||
|| **next**
[`integer`](../../data-types.md) | Contains the value to be passed in the next request in the `start` parameter to get the next batch of data.

The `next` parameter appears in the response if the number of items matching your request exceeds `50` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

> HTTP Status: 40x, 50x Error

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Error Text** | **Description** ||
|| `Access denied` | The user does not have permission to read leads ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-repeat-lead.md)
- [{#T}](../../../tutorials/crm/how-to-get-lists/search-by-phone-and-email.md)