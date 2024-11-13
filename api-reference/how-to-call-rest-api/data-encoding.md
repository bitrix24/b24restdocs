# Data Encoding

## Basic Information

Depending on the tool that generates and sends REST requests to the server, and the data being transmitted, it may be necessary to encode URL parameters; otherwise, the result of the method call will be unpredictable. For example, suppose the task is to create a lead in the CRM with the name "John&Mary" through an incoming webhook. The full URL of the request according to the documentation for the method `crm.lead.add` looks like this:

```
https://b24-abcdef.bitrix24.com/rest/1/xxxxxxx/crm.lead.add?fields[TITLE]=John&Mary
```

After executing the request in any desired way (in a browser, in the command line with cURL, or via a script), you may find that the created lead's name does not match the request—it only shows "Petya." This happened because the lead's name contains the `&` symbol, which has a special meaning in URLs: it is used to separate query parameters. Therefore, the request formed two parameters—`fields[TITLE]` with the value `John` and a new parameter `Mary` with an empty value. To ensure that REST (and any web server in the world) understands that the ampersand is not a special character but part of the value, it must be converted to the sequence `%26`. The correct URL for the task would look like this:

```
https://b24-abcdef.bitrix24.com/rest/1/xxxxxxx/crm.lead.add?fields[TITLE]=John%26Mary
```

This transformation is called URL encoding. There are many characters that can appear in the value of a parameter but have a special role in URLs and "break" the request: `&`, `?`, `%`, `[`, etc. To ensure the request is formed correctly, all parameter values must be URL-encoded. Each programming language has a function for this transformation:

* Javascript - `encodeURIComponent`
* PHP - `urlencode`
* Python - `urllib.quote_plus`
* another language - search for "my_language url encoding" or "my_language url encode"

If the request is being formed manually without using a programming language, you should use any service found by searching for "url encoding online."

{% note tip %}

You can check the correctness of the request formation using request testing services—such as https://webhook.site. It allows you to view each sent request, its headers, and the transmitted parameters in a convenient interface. Such services do not check the correctness of parameters concerning a specific REST API method but can help identify more general issues, like the one mentioned above.

{% endnote %}

## Double URL Encoding

When using the `batch` method, an array of requests is passed in the `cmd` parameter in the form of "method?parameter1=value&parameter2=7," meaning it looks almost like these requests would appear [individually](./general-principles.html), just without the first part of the address. However, since such a URL becomes the value of a request parameter, it also needs to be URL-encoded. This means that first, as usual, the values of the request parameters must be URL-encoded, and then the request itself, because it is now the value of the `cmd` parameter of the `batch` request. If we needed to create a lead from the example above within the batch request execution, the URL would look like this:

```
https://b24-abcdef.bitrix24.com/rest/1/xxxxxxx/batch?cmd[0]=crm.lead.add%3Ffields%5BTITLE%5D%3DJohn%2526Mary
```

## Encoding Complex Structures

{% note warning %}

This section is intended for developers who implement calls to the REST API independently without an SDK or debug requests. For a beginner developer using only the js library BX24 and/or the PHP class CRest and not experiencing any issues, the information provided below may be confusing.

{% endnote %}

When calling the API, it is often necessary to pass a nested data structure—an object with a large depth, an array within an object, an array of objects. The simplest way to do this is to perform a POST request with the data transmitted in JSON format:

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
  // The CRest library does not have the capability to send JSON, 
  // so such requests need to be sent manually
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

- Javascript

  ```javascript
  // Vanilla JS
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

If the ability to send JSON is not available, the data can be sent via a GET or POST request. The GET request from the example above would look like this:

```
https://***/rest/***/crm.lead.add.json?fields[TITLE]=My%20company&fields[PHONE][0][VALUE]=112233&fields[PHONE][0][VALUE_TYPE]=WORK&fields[PHONE][1][VALUE]=555888112&fields[PHONE][1][VALUE_TYPE]=OTHER
```

In PHP, you can obtain a similar string from an array using the `http_build_query` function, while in JS, you can use third-party solutions (for example, the qs library).

When sending a POST request, it is necessary to specify the `Content-Type` of the sent data—`application/x-www-form-urlencoded` or `multipart/form-data; boundary=SomeBoundary`—and encode the data accordingly. Examples of request bodies:

{% list tabs %}

- application/x-www-form-urlencoded

  ```
  fields[TITLE]=My%20company&fields[PHONE][0][VALUE]=112233&fields[PHONE][0][VALUE_TYPE]=WORK&fields[PHONE][1][VALUE]=555888112&fields[PHONE][1][VALUE_TYPE]=OTHER
  ```

- multipart/form-data

  ```
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

Some methods require strict adherence to the order of parameters in the request (for example, [task.commentitem.add](../tasks/comment-item/task-comment-item-add.html), [task.checklistitem.complete](../tasks/checklist-item/task-checklist-item-complete.html)). This means that these parameters must not be passed as named (in PHP as an associative array, in JS as an object); otherwise, the execution result will be unpredictable and may result in an error. They must be presented as an indexed (ordered) array (starting from 0).

Example of **incorrect** comment addition to a task:

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

- Javascript
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

Example of **correct** comment addition to a task:

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

- Javascript

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