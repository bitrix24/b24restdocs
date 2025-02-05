# Click Reaction

`ActionDto` - the action defines the reaction to a click on a specific timeline record element. There are several different types of actions, each with its own format.

## Link Navigation

If a relative link to standard account entities that support opening in a slider is used, the slider will be displayed. Otherwise, it will be a regular link navigation.

### Parameters

{% include [Footnote about parameters](../../../../../../_includes/required.md) %}

#| 
|| **Field** | **Description** ||
|| **type^*^**
[`const`](../../../../data-types.md) | Must have the value `redirect` ||
|| **uri**
[`string`](../../../../data-types.md) | Valid URI link, for example `https://example.com` or `/crm/deal/details/1/` ||
|#

### Example

```json
{
    "type": "redirect",
    "uri": "/crm/deal/details/1/"
}
```

## Rest Event

Calling the action will generate a Rest event **onCrmTimelineItemAction**. When the event occurs, handlers will be invoked only for the application that created this timeline record. The context will always be passed to the handler:

- **id** - event identifier,
- **entityTypeId** - identifier of the entity type to which the deal is linked,
- **entityId** - identifier of the element of this entity,
- **activityId** - identifier of the deal,
- **userId** - identifier of the user who triggered the action.

### Parameters

{% include [Footnote about parameters](../../../../../../_includes/required.md) %}

#| 
|| **Field** | **Description** ||
|| **type^*^**
[`const`](../../../../data-types.md) | Must have the value `restEvent`. ||
|| **id^*^**
[`string`](../../../../data-types.md) | Event identifier. You can specify any convenient value, for example `resetButtonClick` ||
|| **actionParams**
[`array`](../../../../data-types.md) | Array of arbitrary format, the data from which will be passed to the event handler ||
|| **animationType**
[`string`](../../../../data-types.md) | Animation that will be shown during event processing, for example `disable` ||
|#

In some scenarios, sending a Rest event implies that the handler of this event should ultimately change the appearance of the timeline record. For example, adding new blocks, changing the set of buttons, etc.

To give the user an understanding that some processing is currently happening, you can use the `animationType` parameter. If `animationType` is set to **loader**, the timeline record will be locked, and a loader will be displayed over it until this record is updated via [crm.activity.configurable.update](../crm-activity-configurable-update.md). If the action is triggered on a button at the bottom of the timeline and `animationType` is set to `disable`, the corresponding button will be locked until this record is updated via [crm.activity.configurable.update](../crm-activity-configurable-update.md).

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

If an action of this configuration is assigned to a button, clicking it will trigger the Rest event handler `onCrmTimelineItemAction`, registered by the application that created the timeline record. The handler will receive parameters `id=resetButtonClick`, `entityTypeId`, `entityId`, `activityId`, `userId`, as well as explicitly defined developer parameters `myId=123` and `someImportant=qwerty`. From the moment the button is clicked until the timeline record is updated via `crm.activity.configurable.update`, the button will be locked.

## Opening Application Slider

{% note warning %}

The action is not supported in the mobile application.

{% endnote %}

Calling the action will open the slider of the application that created this timeline record. The context will be passed to the slider:

- **entityTypeId** - identifier of the entity type to which the deal is linked,
- **entityId** - identifier of the element of this entity,
- **activityId** - identifier of the deal.

### Parameters

{% include [Footnote about parameters](../../../../../../_includes/required.md) %}

#| 
|| **Field** | **Description** ||
|| **type^*^**
[`const`](../../../../data-types.md) | Must have the value `openRestApp`. ||
|| **actionParams**
[`array`](../../../../data-types.md) | Array of arbitrary format, the data from which will be passed to the event handler ||
|| **sliderParams**
[ActionSliderParamsDto](#actionsliderparamsdto) | Options with which the slider is opened ||
|#

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

#### Parameters for Opening Application Slider

#| 
|| **Field** | **Description** | **Additional** ||
|| **width**
[`int`](../../../../data-types.md) | Width of the slider, `px` | Cannot be used simultaneously with `leftBoundary` ||
|| **leftBoundary**
[`int`](../../../../data-types.md) | Slider across the full width of the browser window with a left margin, `px` | Cannot be used simultaneously with `width` ||
|| **labelBgColor**
[`string`](../../../../data-types.md) | Background color code of the slider close button | Can only take values `aqua|green|orange|brown|pink|blue|grey|violet` ||
|| **labelColor**
[`string`](../../../../data-types.md) | Color code of the slider close button | In `hex` format ||
|| **labelText**
[`string`](../../../../data-types.md) | Text of the slider close button | ||
|| **title**
[`string`](../../../../data-types.md) | Text of the browser window title when opening the slider | ||
|#

## Continue Exploring

- [{#T}](./layout.md)
- [{#T}](./icon.md)
- [{#T}](./header.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)