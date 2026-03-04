# Data Encoding

When sending REST requests, special characters in parameters can disrupt the URL structure. This can lead to errors or incorrect data.

For example, if you need to create a lead with the name `John&Martin` via an incoming webhook, according to the documentation for the method [crm.lead.add](../../api-reference/crm/leads/crm-lead-add.md), the request URL should look like this:

```curl
https://b24-abcdef.bitrix24.com/rest/1/xxxxxxx/crm.lead.add?fields[TITLE]=John&Martin
```

After executing the request, you may find that the lead's name only contains `John`. This happened because the `&` character separates query parameters. If it appears within a value, the server interprets it as the start of a new parameter rather than part of the data.

As a result, the request has two parameters:

- `fields[TITLE]` with the value `John`
- a new parameter `Martin` without a value

To ensure the method works correctly, you need to replace the special character with its encoded equivalent. For `&`, this is `%26`. This transformation is known as URL encoding.

The correct URL for the task at hand is:

```curl
https://b24-abcdef.bitrix24.com/rest/1/xxxxxxx/crm.lead.add?fields[TITLE]=John%26Martin
```

## Which Characters Need Encoding

In a URL, special characters such as `&`, `?`, `%`, `[`, `]`, `#`, and others play a significant role. If they appear in a parameter value, they must be encoded. Otherwise, the server will interpret them as control characters, and the result of the request will become unpredictable.

## How to Encode in Different Languages

Each programming language provides a built-in function:

- JavaScript — `encodeURIComponent`
- PHP — `urlencode`
- Python — `urllib.parse.quote_plus`
- Java — `URLEncoder.encode`

If you are forming a request manually, you can use any online service by searching for "URL encoding online."

{% note tip "" %}

You can check the correctness of the request using the service [https://webhook.site](https://webhook.site). It shows the complete request, including headers and parameters.

{% endnote %}

## Double Encoding for Batch Requests

The `batch` method allows you to execute multiple requests in a single call. Requests are passed in the `cmd` parameter as strings: `method?parameter1=value&parameter2=7`. However, each of these strings becomes a parameter value itself. Therefore, it also needs to be encoded.

1. First, encode the values inside the nested request.
2. Then, encode the entire string of the nested request.

If you needed to create a lead from the example above as part of a batch request, the URL would look like this:

```curl
https://b24-abcdef.bitrix24.com/rest/1/xxxxxxx/batch?cmd[0]=crm.lead.add%3Ffields%5BTITLE%5D%3DJohn%2526Martin
```

Note: `%26` has turned into `%2526` because the `%` character itself was encoded as `%25`.

{% note info "" %}

To avoid manual encoding and other complexities, use ready-made SDKs. They handle all the work with URLs, encoding, and error handling.

Official Bitrix24 SDKs:

- [B24PhpSdk](../../sdk/b24phpsdk/index.md) and [CRest PHP SDK](../../sdk/crest-php-sdk/index.md) for PHP
- [B24JsSDK](../../sdk/b24jssdk/index.md) for JavaScript

{% endnote %}

## How to Pass Complex Structures

Many REST API methods accept nested data, such as a list of contact phone numbers or an array of fields. In such cases, it is recommended to send POST requests with the body in JSON format. This way, the structure is preserved, and you don't have to worry about encoding individual parameters.

Example of a lead with multiple phone numbers:

{% list tabs %}

- cURL

  ```bash
  curl -X POST \
  -H "Content-Type: application/json" \
  -d '{
          "fields": {
              "TITLE": "My company",
              "PHONE": [
                  {
                      "VALUE": "112233",
                      "VALUE_TYPE": "WORK"
                  },
                  {
                      "VALUE": "555888112",
                      "VALUE_TYPE": "OTHER"
                  }
              ]
          }
      }' \ 
  https://***/rest/***/crm.lead.add
  ```

- PHP

  ```php
  $data = [
      "fields" => [
          "TITLE" => "My company",
          "PHONE" => [
              [
                  "VALUE" => "112233",
                  "VALUE_TYPE" => "WORK"
              ],
              [
                  "VALUE" => "555888112",
                  "VALUE_TYPE" => "OTHER"
              ]
          ]
      ]
  ];
  
  $ch = curl_init();
  curl_setopt($ch, CURLOPT_USERAGENT, 'php script');
  curl_setopt($ch, CURLOPT_HEADER, false);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($ch, CURLOPT_TIMEOUT, 5);
  curl_setopt($ch, CURLOPT_URL, 'https://***/rest/***/crm.lead.add');
  curl_setopt($ch, CURLOPT_HTTPHEADER, ['Content-Type: application/json']);
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
  $raw = curl_exec($ch);
  curl_close($ch);
  ```

- JS

  ```js
  data = {
      fields: {
          TITLE: 'My company',
          PHONE: [
              {
                  VALUE: '112233',
                  VALUE_TYPE: 'WORK'
              },
              {
                  VALUE: '555888112',
                  VALUE_TYPE: 'OTHER'
              }
          ]
      }
  };
  
  fetch('https://***/rest/***/crm.lead.add', {
      method: 'POST',
      headers: {
          'Content-Type': 'application/json'
      },
      body: JSON.stringify(data)
  });
  
  ```

- Python

  ```python
  import requests
  
  data = {
      'fields': {
          'TITLE': 'My company',
          'PHONE': [
                {
                      'VALUE': '112233',
                      'VALUE_TYPE': 'WORK'
                },
                {
                      'VALUE': '555888112',
                      'VALUE_TYPE': 'OTHER'
                }
          ]
      }
  }
  
  r = requests.post('https://***/rest/***/crm.lead.add', json=data)
  
  ```

{% endlist %}

If sending JSON is not possible, use a GET request or a standard POST request. Data should be passed in the query parameters, following the encoding rules.

### GET Request

The GET request for the example above:

```curl
https://***/rest/***/crm.lead.add.json?fields[TITLE]=My%20company&fields[PHONE][0][VALUE]=112233&fields[PHONE][0][VALUE_TYPE]=WORK&fields[PHONE][1][VALUE]=555888112&fields[PHONE][1][VALUE_TYPE]=OTHER
```

How to programmatically assemble such a string:

- PHP — the `http_build_query` function converts an array into a properly encoded string.
- JavaScript — you can use the `qs` library or write your own function.

### POST Request

For a POST request, you need to specify the correct `Content-Type` header and format the request body.

- `application/x-www-form-urlencoded` — data is converted into a single string
- `multipart/form-data` — data is split into parts, each with its own headers and separated by a boundary string `boundary=SomeBoundary`

{% list tabs %}

- application/x-www-form-urlencoded

  ```curl
  fields[TITLE]=My%20company&fields[PHONE][0][VALUE]=112233&fields[PHONE][0][VALUE_TYPE]=WORK&fields[PHONE][1][VALUE]=555888112&fields[PHONE][1][VALUE_TYPE]=OTHER
  ```

- multipart/form-data

  ```curl
  --SomeBoundary
  Content-Disposition: form-data; name="fields[TITLE]"
  
  My company
  --SomeBoundary
  Content-Disposition: form-data; name="fields[PHONE][0][VALUE]"
  
  112233
  --SomeBoundary
  Content-Disposition: form-data; name="fields[PHONE][0][VALUE_TYPE]"
  
  WORK
  --SomeBoundary
  Content-Disposition: form-data; name="fields[PHONE][1][VALUE]"
  
  555888112
  --SomeBoundary
  Content-Disposition: form-data; name="fields[PHONE][1][VALUE_TYPE]"
  
  OTHER
  --SomeBoundary
  ```

{% endlist %}

## Order of Parameters

Some methods, such as [task.commentitem.add](../../api-reference/tasks/comment-item/task-comment-item-add.md) and [task.checklistitem.complete](../../api-reference/tasks/checklist-item/task-checklist-item-complete.md), require strict adherence to the order of parameter transmission. In such cases, parameters cannot be passed by name — as an object in JavaScript or an associative array in PHP. Otherwise, the execution result will be unpredictable, or the method will return an error.

To pass such parameters, use an array with numeric indices. The indices should start from 0.

Example of incorrect comment addition to a task:

{% list tabs %}

- cURL

  ```bash
  curl 'https://***/rest/***/task.commentitem.add?TASKID=123&FIELDS[POST_MESSAGE]=test'
  ```

- PHP 

  ```php
  CRest::call(
      'task.commentitem.add',
      [
          'TASKID' => 123,
          'FIELDS' => ['POST_MESSAGE' => 'text']
      ]
  );
  ```

- JS
  
  ```javascript
  BX24.callMethod(
      'task.commentitem.add',
      {
          TASKID: 123,
          FIELDS: { 'POST_MESSAGE': 'text' }
      }
  );
  ```

{% endlist %}

Example of correct comment addition to a task:

{% list tabs %}

- cURL

  ```bash
  curl 'https://***/rest/***/task.commentitem.add?0=123&1[POST_MESSAGE]=test'
  ```

- PHP
  
  ```php
  CRest::call(
      'task.commentitem.add',
      [
          123,
          ['POST_MESSAGE' => 'text']
      ]
  );
  ```

- JS

  ```javascript
  BX24.callMethod(
      'task.commentitem.add',
      [
          123,
          { 'POST_MESSAGE': 'text' }
      ]
  );
  ```

{% endlist %}