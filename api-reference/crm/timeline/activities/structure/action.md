# Click Reaction

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

The action defines the reaction to a click on a specific element of the timeline record. There are several different types of actions, each with its own format.

## Link Navigation

If a relative link to standard account entities that support opening in a slider is used, the slider will be displayed. Otherwise, it will be a regular link navigation.

#|
|| **Field** | **Description** ||
|| **type^*^**
[`const`](../../../../data-types.md) | Must have the value `redirect` ||
|| **uri**
[`string`](../../../../data-types.md) | URI of the link. For example, `https://ya.com` or `/crm/deal/details/1/` ||
|#

{% include [Note on parameters](../../../../../_includes/required.md) %}

### Example

```json
{
    "type": "redirect",
    "uri": "/crm/deal/details/1/"
}
```

## Rest Event

Calling the action will generate a Rest event **onCrmTimelineItemAction**. When the event occurs, handlers from only the application that created this timeline record will be invoked. The context will always be passed to the handler:

- **id** - event identifier,
- **entityTypeId** - identifier of the entity type to which the deal is linked,
- **entityId** - identifier of the element of this entity,
- **activityId** - identifier of the deal,
- **userId** - identifier of the user who triggered the action.

#|
|| **Field** | **Description** ||
|| **type^*^**
[`const`](../../../../data-types.md) | Must have the value `restEvent`. ||
|| **id^*^**
[`string`](../../../../data-types.md) | Event identifier. You can specify any value that is convenient for you. ||
|| **actionParams**
[`array`](../../../../data-types.md) | An array of arbitrary format, the data from which will be passed to the event handler. ||
|| **animationType**
[`string`](../../../../data-types.md) | URI of the link. Animation that will be shown during the event processing. ||
|#

{% include [Note on parameters](../../../../../_includes/required.md) %}

### Example

```json
{
    "type": "restEvent",
    "id": "resetButtonClick",
    "actionParams": {
     "myId": 123,
     "someImportant": "qwerty"
    },
    "animationType": "disable"
}
```

## Opening Application Slider

This action is not supported in the mobile application.

Calling the action will open the slider of the application that created this timeline record. The context will be passed to the slider:

- **entityTypeId** - identifier of the entity type to which the deal is linked,
- **entityId** - identifier of the element of this entity,
- **activityId** - identifier of the deal.

#|
|| **Field** | **Description** ||
|| **type^*^**
[`const`](../../../../data-types.md) | Must have the value `openRestApp`. ||
|| **actionParams**
[`array`](../../../../data-types.md) | An array of arbitrary format, the data from which will be passed to the event handler. ||
|| **sliderParams**
[ActionSliderParamsDto](#ActionSliderParamsDto) | Opening parameters. ||
|#

{% include [Note on parameters](../../../../../_includes/required.md) %}

### Example

```json
{
    "type": "openRestApp",
    "actionParams": {
        "myId": 123,
        "someImportant": "qwerty"
    },
    "sliderParams": {
        "labelText": "this application",
        "labelColor": "#c3b3ff",
        "labelBgColor": "violet",
        "title": "This is the application slider title",
        "width": 700
    }
}
```

### ActionSliderParamsDto

Parameters for opening the application slider

#|
|| **Field** | **Description** | **Additional** ||
|| **width**
[`int`](../../../../data-types.md) | Width of the slider, px | Cannot be used simultaneously with leftBoundary ||
|| **leftBoundary**
[`int`](../../../../data-types.md) | Slider spans the full width of the browser window with a left margin, px | Cannot be used simultaneously with width ||
|| **labelBgColor**
[`string`](../../../../data-types.md) | Background color code of the slider close button | Can only take values aqua, green, orange, brown, pink, blue, grey, violet ||
|| **labelColor**
[`string`](../../../../data-types.md) | Color code of the slider close button | In the format #112233 ||
|| **labelText**
[`string`](../../../../data-types.md) | Text of the slider close button | ||
|| **title**
[`string`](../../../../data-types.md) | Text of the browser window title when opening the slider | ||
|#