# Data Types and Parameter Formats in REST API

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

## TO-DO _not exported to prod_

- Article about time zones and calculating offsets from the server. Check the specifics in tasks and CRM and link to it in the note block.
- Article on working with files with all examples that differ due to different APIs and link to it in the table.
- Article about crm.status.list with details on what values are there and how to extract them correctly. The link to the article should be placed in the types table.
- Article about the currency list.
- Article about the contacts list.
- Article about the companies list.
- Article about the leads list.
- Article about the compreds list.
- Article on working with locations.
- Article about the timeline tasks list.
- Need to describe the structure of the time type object.
- Missing descriptions for the types function, callable.

{% endnote %}

{% endif %}

## Basic Data Types {#standart-types}

#| 
|| **Type** | **Descriptions and Values** ||
|| `integer` | Whole number. For example, 10116 ||
|| `boolean` | Boolean value. Most often takes values `Y` or `N`. In some outdated methods, it may take values 0 or 1 ||
|| `char` | Fixed-length string type, usually `CHAR(1)`. Often used as a replacement for `boolean` and stores `Y` or `N` ||
|| `double` | Floating-point number. For example, 100.15 ||
|| `date` | Date in the format 'YYYY-MM-DD'. For example, '2023-12-28', which means December 28, 2023 ||
|| `datetime` | Date and time in the format 'YYYY-MM-DDThh:mm:ss±hh:mm'. For example, '2023-12-28T14:05:48', which means 14 hours, 5 minutes, and 48 seconds on December 28, 2023. ||
|| `string` | Single-line string value. For example, 'Supply Agreement' ||
|| `text` | Multi-line string value, applicable in some special fields of objects. ||
|| `file` | Attached file. Can take a numeric value with a unique file identifier in the system or a value in the form of an array describing a series of file parameters. Read about working with files in the corresponding section. ||
|| `array` | A set of simple type elements. For example, an array of integers [1, 5, 67] or strings ['deal', 'lead', 'quote'] ||
|| `object` | Structure of arbitrary nesting level containing elements of different data types. For example,

```json
{
    data: {
        foo: "bar",
        bar: "foo",
        items: [
            1,
            100,
            200
        ]
    }
}
```

 ||
|| `any`  |  Various data types can serve as parameter values. ||
|#

{% note tip "Features of Dates and Times" %}

When working with fields of types _date_ and _datetime_, note that each user in Bitrix24 can have their own time zone specified in the settings. The Bitrix24 user interface displays dates and times, adapting them to the specific user; however, at the API level, all dates and times are stored according to the server parameters. Read more details in the corresponding material.

{% endnote %}

## Links to Objects and Directories {#standart-objects}

Fields of Bitrix24 objects can contain values that refer to other objects or to values from directories. Technically, such values are most often stored and indicated as integer identifiers of specific objects or directory elements. However, for convenience and to emphasize such relationships, we will use special types in the documentation, such as _crm_company_ or _crm_status_. Below are examples of such types with links to methods for obtaining possible values.

#| 
|| **Type** | **Descriptions and Values** ||
|| `user` | Integer identifier of a Bitrix24 user. For example, 1. User identifiers can be obtained using the [user.get](./user/user-get.md) method. ||
|#

Data directories of various Bitrix24 tools:

- [CRM](./crm/data-types.md)
- [Online Store](./sale/data-types.md)
- [Product Catalog](./catalog/data-types.md)

## Time Object {#time}

The `time` object is present in the responses to all REST requests and contains information about the execution time of the request.

#| 
|| **Name**
`type` | **Description** ||
|| **start**
[`double`] | Timestamp of the moment the request was initialized. ||
|| **finish**
[`double`] | Timestamp of the moment the request execution was completed. ||
|| **duration**
[`double`] | How long in milliseconds the request took, i.e., the difference between `finish` and `start`. ||
|| **date_start**
[`string`] | String representation of the date and time of the moment the request was initialized. ||
|| **date_finish**
[`double`] | String representation of the date and time of the moment the request execution was completed. ||
|| **operating_reset_at**
[`timestamp`] | Timestamp of the moment when the limit on REST API resources will be reset.

Read more details in the article [operation limit](../limits.md). ||
|| **operating**
[`double`] | In how many milliseconds the limit on REST API resources will be reset.

Read more details in the article [operation limit](../limits.md). ||
|#