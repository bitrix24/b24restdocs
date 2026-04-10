# Data Types and Parameter Formats in REST API

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This page describes the data types used in method parameters, object structures, and REST API responses. For each type, the value format and usage specifics are provided. This is a general dictionary of types: the exact field composition, acceptable values, and additional constraints should be checked on the specific method or object page.

## Basic Data Types {#standart-types}

#|
|| **Type** | **Description** ||
|| `integer` | An integer. For example, `10116` ||
|| `boolean` | A boolean value. Most often takes values `Y` or `N`. In some methods, it may accept values `0` or `1` ||
|| `char` | A fixed-length string type, usually `CHAR(1)`. Often used as a substitute for `boolean` and stores `Y` or `N` ||
|| `double` | A floating-point number. For example, `100.15` ||
|| `date` | A date in the format `YYYY-MM-DD`. For example, `2025-12-28`, December 28, 2025 ||
|| `datetime` | A date and time in the format `YYYY-MM-DDThh:mm:ss±hh:mm`. For example, `2023-12-28T14:05:48+03:00` ||
|| `timestamp` | A date and time in Unix Timestamp format, usually an integer representing the number of seconds since January 1, 1970. For example, `1719826800` ||
|| `string` | A single-line string value. For example, `Supply Agreement` ||
|| `text` | A multi-line string value, applicable in some special fields of objects ||
|| `file` | An attached file. It can take a numeric value with a unique file identifier in the system or a value in the form of an array describing the file parameters. 

For more information on working with files, read the articles:
- [How to Upload Files](./files/how-to-upload-files.md)
- [How to Update and Delete Files](./files/how-to-update-files.md)
||
|| `enum` | A list type. The field accepts one value from a fixed set of allowed options. For example, status, object type, or deletion status ||
|| `array` | A set of simple type elements. For example, an array of integers `[1, 5, 67]` or strings `["deal", "lead", "quote"]` ||
|| `object` | A structure of arbitrary nesting level containing key-value pairs. The key is a parameter or field. Example:

```json
{
  "data": {
    "foo": "bar",
    "bar": "foo",
    "items": [
      1,
      100,
      200
    ]
  }
}
```

||
|| `function` | A function. Used in JavaScript examples and Bitrix24 js interfaces to describe a handler ||
|| `callable` | A callable handler, usually a callback function. In JavaScript, this can be a regular function or its shorthand notation using `=>` ||
|| `any` | Various data types can serve as the parameter value ||
|#

{% note tip "Date and Time Features" %}

When working with fields of types _date_ and _datetime_, please note that each user in Bitrix24 can have their own time zone specified in the settings. The Bitrix24 user interface displays dates and times, adapting them to the specific user; however, at the API level, all dates and times are stored considering the server parameters.

{% endnote %}

## Data Types for Object References and Directories {#standart-objects}

Bitrix24 object fields can contain values that reference other objects or values from directories. Technically, such values are most often stored and indicated as integer identifiers of specific objects or directory elements. However, for convenience and to emphasize such relationships, we will use special types in the documentation, such as `_crm_company_` or `_crm_status_`. Below are examples of such types with links to methods for obtaining possible values.

#|
|| **Type** | **Description** ||
|| `user` | An integer identifier of a Bitrix24 user, for example, `1`. 

User identifiers can be obtained using the [user.get](./user/user-get.md) method ||
|#

Data directories for various Bitrix24 tools:

- [CRM](./crm/data-types.md)
- [Online Store](./sale/data-types.md)
- [Product Catalog](./catalog/data-types.md)

## Time Object {#time}

The `time` object is present in the responses to all REST requests and contains information about the request execution time.

The composition of the `time` object fields may vary depending on the method and environment. In its full version, the response may contain all fields from the example below.

Example structure:

```json
{
    "time": {
        "start": 1757424671.509725,
        "finish": 1757424672.203906,
        "duration": 0.694180965423584,
        "processing": 0.6726489067077637,
        "date_start": "2025-09-09T16:31:11+03:00",
        "date_finish": "2025-09-09T16:31:12+03:00",
        "operating_reset_at": 1757425271,
        "operating": 0.6726338863372803
    }
}
```

#| 
|| **Name**
`type` | **Description** ||
|| **start**
`double` | A timestamp in Unix Timestamp format for the moment the request is initialized. Returned as a number with a fractional part because time is transmitted in seconds with millisecond precision ||
|| **finish**
`double` | A timestamp in Unix Timestamp format for the moment the request execution is completed. Returned as a number with a fractional part because time is transmitted in seconds with millisecond precision ||
|| **duration**
`double` | The duration of the request execution in seconds, i.e., the difference between `finish` and `start` ||
|| **processing**
`double` | The time taken to process the request in seconds ||
|| **date_start**
`string` | A string representation of the date and time of the request initialization moment ||
|| **date_finish**
`string` | A string representation of the date and time of the request completion moment ||
|| **operating_reset_at**
`timestamp` | A timestamp in Unix Timestamp format for the moment when a portion of the method's resource consumption limit will be released. Usually returned as an integer

For more details, see the article [operation limit](../limits.md) ||
|| **operating**
`double` | The accumulated time of requests to a specific method in seconds. Used to monitor the resource consumption of the REST API.

For more details, see the article [operation limit](../limits.md) ||
|#