# Widget as a Link with REST_APP_URI Slider

> Scope: [`placement`](../../scopes/permissions.md)

This integration does not have a separate, predefined button in the interface that allows the user to open it independently. The application can simply send a specially formatted link to any Bitrix24 tool that supports adding content with links:

- Messages and comments in the news feed;
- Messages in internal group and individual chats (will not work in open line dialogs);
- Task descriptions and comments;
- Calendar meeting descriptions;
- Etc.

To use this integration, the link must be in the format `/marketplace/view/#APP_CODE#/`, where `#APP_CODE#`:

- is the symbolic code of your mass-market application from the Developer's area, which is set in the application card;
- is the client_id of the local application (for example, `local.66ba434d853c87.18550109`), which can be copied from the application settings in the Developer resources section.

The integration can accept any number of arbitrary parameters in the GET key `params`, for example: `/marketplace/view/#APP_CODE#/?params[test]=y`. In this case, `PLACEMENT_OPTIONS` will be as follows:

```php
[PLACEMENT_OPTIONS] => {"test":"y"}
```

The widget's integration code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the Widget is Integrated

#|
|| **Integration Code** | **Location** ||
|| `REST_APP_URI` | The specific integration location is not indicated at the stage of adding the widget handler, as the widget will open when clicking on any links of the special format described above ||
|#

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 195ec4ee87932d8f9bbbd6a2f0a83553
    [AUTH_ID] => f27bbb6600705a0700005a4b00000001f0f107398c3f17f5fc48d5ce194d5c65de7cfb
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => e2fae26600705a0700005a4b00000001f0f1075f986dbd8dff24c36c2ad9bb0816a665
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => REST_APP_URI
    [PLACEMENT_OPTIONS] => {"test":"y"}
)
```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys that were specified in the `params` parameter of the specific link, as described above.

This means that by setting your links and processing the received GET parameter `params` in the widget handler, you can implement any necessary business logic.

{% note tip "Typical Use-Cases and Scenarios" %}

- [Viewing External Documents via Link](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268&LESSON_ID=26030&LESSON_PATH=25400.25996.26000.26030)

{% endnote %}

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)