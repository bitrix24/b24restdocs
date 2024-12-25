# Get Data on User Field Settings userfieldconfig.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`userfieldconfig, module scope`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
userfieldconfig.get({id: number, moduleId: string})
```

The method will return data on the settings of the user field with the identifier **id**.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id^*^** | Identifier of the field settings.  | ||
|| **moduleId^*^** | String identifier of the module.  | ||
|#

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

## Examples

Example response:

```json
{
    "field": {
        "id": "165",
        "entityId": "RPA_1",
        "fieldName": "UF_RPA_1_1585069397",
        "userTypeId": "file",
        "xmlId": null,
        "sort": "100",
        "multiple": "Y",
        "mandatory": "N",
        "showFilter": "E",
        "showInList": "Y",
        "editInList": "Y",
        "isSearchable": "Y",
        "settings": {
            "SIZE": 20,
            "LIST_WIDTH": 0,
            "LIST_HEIGHT": 0,
            "MAX_SHOW_SIZE": 0,
            "MAX_ALLOWED_SIZE": 0,
            "EXTENSIONS": []
        },
        "languageId": {
            "en": "en",
            "de": "de"
        },
        "editFormLabel": {
            "en": "",
            "de": "Multiple file"
        },
        "listColumnLabel": {
            "en": null,
            "de": null
        },
        "listFilterLabel": {
            "en": null,
            "de": null
        },
        "errorMessage": {
            "en": null,
            "de": null
        },
        "helpMessage": {
            "en": null,
            "de": null
        }
    }
}
```

Where:
- **id** - identifier
- **entityId** - string identifier of the object
- **fieldName** - field code
- **userTypeId** - string identifier of the field type
- **xmlId** - external identifier
- **sort** - sort index
- **multiple** - multiplicity flag
- **mandatory** - mandatory flag
- **showFilter** - flag for showing the field in the filter. For some objects, such as RPA, it returns the value Enabled `E` instead of `Y`
- **showInList** - flag for showing the field in the list
- **editInList** - flag for allowing editing the field in the list
- **isSearchable** - flag for the presence of the field value in the full-text index
- **settings** - list of additional field settings, depending on its type
- **languageId** - list of language identifiers for which phrases are available
- **editFormLabel** - list with language-dependent field names, where the key is the language identifier and the value is the phrase
- **listColumnLabel**, **listFilterLabel**, **errorMessage**, **helpMessage** - similar lists of phrases for various purposes (not used)
- **enum** - array with value options for properties of type "list" (enumeration), including option identifier, value, default flag, sort index, and external identifier of the option.

{% include [Footnote on examples](../../../../../_includes/examples.md) %}


{% if build == 'dev' %}

{% note alert "Parameter `settings`" %}

During the description of `crm.contact.userfield.get`, almost all `settings` for all types of user fields were mistakenly described.
I am leaving them here to simplify their description later :)

{% endnote %}

### Parameter settings

{% list tabs %}

- double

  #|
  || **Name**
  `type` | **Description** ||
  || **PRECISION**
  [`integer`][1] | Precision (number of decimal places) ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **MIN_VALUE**
  [`double`][1] | Minimum value (0 - do not check) ||
  || **MAX_VALUE**
  [`double`][1] | Maximum value (0 - do not check) ||
  || **DEFAULT_VALUE**
  [`double`][1] | Default value ||
  |#

- boolean

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`integer`][1] | Default value
  `0` - No
  `1` - Yes ||
  || **DISPLAY**
  [`string`][1] | Appearance

  Possible values:
  `CHECKBOX` - Checkbox
  `RADIO` - Radio buttons
  `DROPDOWN` - Dropdown list ||
  || **LABEL**
  [`string[]`][1] | Labels for values, where
    * array element with index `0` - Label for value "No"
    * array element with index `1` - Label for value "Yes"

  ||
  || **LABEL_CHECKBOX**
  [`string`][1] | Checkbox label ||
  |#

- date

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`object`][1] | Default value. Object format:
  ```
  {
      TYPE: 'NONE'|'FIXED'|'NONE',
      VALUE: date
  }
  ```

  where `TYPE` - Type of default value:
  `NONE` - Absent
  `NOW` - Current date
  `FIXED` - Date from `VALUE`

  `VALUE` is of type `date` ||
  |#

- integer

  #|
  || **Name**
  `type` | **Description** ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **MIN_VALUE**
  [`integer`][1] | Minimum value (0 - do not check) ||
  || **MAX_VALUE**
  [`integer`][1] | Maximum value (0 - do not check) ||
  || **DEFAULT_VALUE**
  [`integer`][1] | Default value ||
  |#

- datetime

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`object`][1] | Default value. Object format:
  ```
  {
      TYPE: 'NONE'|'FIXED'|'NONE',
      VALUE: datetime
  }
  ```

  where `TYPE` - Type of default value:
  `NONE` - Absent
  `NOW` - Current date with time
  `FIXED` - Date with time from `VALUE`

  `VALUE` is of type `datetime` ||
  || **USE_SECOND**
  [`boolean`][1] | Use seconds

  Possible values:
  `Y` - Yes
  `N` - No ||
  || **USE_TIMEZONE**
  [`boolean`][1] | Use time zones

  Possible values:
  `Y` - Yes
  `N` - No ||
  |#

- string

  #|
  || **Name**
  `type` | **Description** ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **ROWS**
  [`integer`][1] | Number of lines in the input field ||
  || **REGEXP**
  [`string`][1] | Regular expression for validation ||
  || **MIN_LENGTH**
  [`integer`][1] | Minimum string length (0 - do not check) ||
  || **MAX_LENGTH**
  [`integer`][1] | Maximum string length (0 - do not check) ||
  || **DEFAULT_VALUE**
  [`string`][1] | Default value ||
  |#

- enumeration

  #|
  || **Name**
  `type` | **Description** ||
  || **DISPLAY**
  [`string`][1] | Appearance

  Possible values:
  `LIST` - List
  `CHECKBOX` - Checkboxes
  `UI` - Input list
  `DIALOG` - Entity selection dialog ||
  || **LIST_HEIGHT**
  [`integer`][1] | List height ||
  || **CAPTION_NO_VALUE**
  [`string`][1] | Label when no value ||
  || **SHOW_NO_VALUE**
  [`boolean`][1] | Show empty value for required field

  Possible values:
  `Y` - Yes
  `N` - No ||
  |#

- iblock_section|iblock_element

  #|
  || **Name**
  `type` | **Description** ||
  || **DISPLAY**
  [`string`][1] | Appearance

  Possible values:
  `LIST` - List
  `CHECKBOX` - Checkboxes
  `UI` - Input list
  `DIALOG` - Entity selection dialog ||
  || **LIST_HEIGHT**
  [`integer`][1] | List height ||
  || **IBLOCK_ID**
  [`integer`][1] | Identifier of the information block ||
  || **DEFAULT_VALUE**
  [`integer`][1] | Default value ||
  || **ACTIVE_FILTER**
  [`boolean`][1] | Show only active elements

  Possible values:
  `Y` - Yes
  `N` - No ||
  |#

- crm_status

  #|
  || **Name**
  `type` | **Description** ||
  || **ENTITY_TYPE**
  [`object`][1] | CRM reference. Structure is similar to the returned elements of the method [`crm.status.entity.types`](../../../status/crm-status-entity-types.md) ||
  |#

- crm

  #|
  || **Name**
  `type` | **Description** ||
  || **LEAD**
  [`boolean`][1] | Lead binding enabled ||
  || **CONTACT**
  [`boolean`][1] | Contact binding enabled ||
  || **COMPANY**
  [`boolean`][1] | Company binding enabled ||
  || **DEAL**
  [`boolean`][1] | Deal binding enabled ||
  || **ORDER**
  [`boolean`][1] | Order binding enabled ||
  || **QUOTE**
  [`boolean`][1] | Estimate binding enabled ||
  || **SMART_INVOICE**
  [`boolean`][1] | New invoice binding enabled ||
  || **DYNAMIC_...**
  [`boolean`][1] | Binding to a specific SPA enabled

  Each such field has the form: `DYNAMIC_{entityTypeId}`, where `entityTypeId` is the identifier of the SPA type to which the binding is enabled. ||
  |#

- money

  #|
  || **Name**
  `type` | **Description** ||
  || **DEFAULT_VALUE**
  [`string`][1] | Default value

  The value of this field has the format: "**{VALUE}\|{CURRENCY}**", where
  `VALUE` - Default amount of money
  `CURRENCY` - String identifier of the currency

  For example: "**300\|USD**" - 300 dollars

  ||
  |#

- disk_file|disk_version

  #|
  || **Name**
  `type` | **Description** ||
  || **IBLOCK_ID**
  [`integer`][1] | Document library information block ||
  || **SECTION_ID**
  [`integer`][1] | Document library folder ||
  || **UF_TO_SAVE_ALLOW_EDIT**
  [`boolean`][1] | Save editing settings ||
  |#

- video

  #|
  || **Name**
  `type` | **Description** ||
  || **BUFFER_LENGTH**
  [`integer`][1] | Buffer size in seconds ||
  || **CONTROLBAR**
  [`string`][1] | Control bar position

  Possible values:
  `bottom` - Bottom
  `over` - Over
  `none` - Do not show ||
  || **AUTOSTART**
  [`boolean`][1] | Automatically start playing ||
  || **VOLUME**
  [`integer`][1] | Volume level as a percentage of maximum ||
  || **SKIN**
  [`string`][1] | Skin ||
  || **FLASHVARS**
  [`string`][1] | Additional Flashvars ||
  || **WMODE_FLV**
  [`string`][1] | Window mode (WMode)

  Possible values:
  `WMV` - Window mode
  `WINDOW` - Normal
  `OPAQUE` - Opaque
  `TRANSPARENT` - Transparent ||
  || **BGCOLOR**
  [`string`][1] | Background color of the control panel ||
  || **COLOR**
  [`string`][1] | Color of control elements ||
  || **OVER_COLOR**
  [`string`][1] | Color of control elements on hover ||
  || **SCREEN_COLOR**
  [`string`][1] | Screen color ||
  || **SILVERVARS**
  [`string`][1] | Additional Silverlight variables ||
  || **WMODE_WMV**
  [`string`][1] | Window mode

  Possible values:
  `window` - Normal
  `windowless` - Transparent ||
  || **WIDTH**
  [`integer`][1] | Width ||
  || **HEIGHT**
  [`integer`][1] | Height ||
  |#

- hlblock

  #|
  || **Name**
  `type` | **Description** ||
  || **DISPLAY**
  [`string`][1] | Appearance

  Possible values:
  `LIST` - List
  `CHECKBOX` - Checkboxes
  `UI` - Input list
  `DIALOG` - Entity selection dialog ||
  || **LIST_HEIGHT**
  [`integer`][1] | List height ||
  || **HLBLOCK_ID**
  [`integer`][1] | Identifier of the highload block ||
  || **HLFIELD_ID**
  [`integer`][1] | Identifier of the highload block element ||
  || **DEFAULT_VALUE**
  [`integer`][1] | Default value ||
  |#

- resourcebooking

  #|
  || **Name**
  `type` | **Description** ||
  || **USE_USERS**
  [`boolean`][1] | Use users or not ||
  || **USE_RESOURCES**
  [`boolean`][1] | Use resources or not ||
  || **RESOURCES**
  [`object`][1] | List of resources ||
  || **SELECTED_RESOURCES**
  [`resourcebooking_resource[]`](#resourcebooking_resource) | Selected resources ||
  || **SELECTED_USERS**
  [`user[]`][1] | Selected employees ||
  || **FULL_DAY**
  [`boolean`][1] | Only date without time ||
  || **ALLOW_OVERBOOKING**
  [`boolean`][1] | Allow booking occupied resources ||
  || **USE_SERVICES**
  [`boolean`][1] | Use services or not ||
  || **SERVICE_LIST**
  [`resourcebooking_service[]`](#resourcebooking_service) | List of services ||
  || **RESOURCE_LIMIT**
  [`integer`][1] | Limit on resource booking. `-1` - No limits ||
  || **TIMEZONE**
  [`string`][1] | Time zone ||
  || **USE_USER_TIMEZONE**
  [`boolean`][1] | Consider user time zone  ||
  |#

  #### resourcebooking_resource

  #|
  || **Name**
  `type` | **Description** ||
  || **name**
  [`string`][1] | Name of the service ||
  || **duration**
  [`integer`][1] | Duration (in minutes) ||
  |#

  #### resourcebooking_service

  #|
  || **Name**
  `type` | **Description** ||
  || **name**
  [`string`][1] | Name of the service ||
  || **duration**
  [`integer`][1] | Duration (in minutes) ||
  |#

- address

  #|
  || **Name**
  `type` | **Description** ||
  || **SHOW_MAP**
  [`boolean`][1] | Show map ||
  |#

- url

  #|
  || **Name**
  `type` | **Description** ||
  || **POPUP**
  [`boolean`][1] | Open in a new window ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **MIN_LENGTH**
  [`integer`][1] | Minimum string length (0 - do not check) ||
  || **MAX_LENGTH**
  [`integer`][1] | Maximum string length (0 - do not check) ||
  || **DEFAULT_VALUE**
  [`string`][1] | Default value ||
  || **ROWS**
  [`integer`][1] | Number of lines in the input field ||
  |#

- file

  #|
  || **Name**
  `type` | **Description** ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **LIST_WIDTH**
  [`integer`][1] | Maximum width for display in the list ||
  || **LIST_HEIGHT**
  [`integer`][1] | Maximum height for display in the list ||
  || **MAX_SHOW_SIZE**
  [`integer`][1] | Maximum allowed size for display in the list (0 - no limit) ||
  || **MAX_ALLOWED_SIZE**
  [`integer`][1] | Maximum allowed file size for upload (0 - do not check) ||
  || **EXTENSIONS**
  [`string[]`][1] | Extensions ||
  || **TARGET_BLANK**
  [`boolean`][1] | Open file in a new tab ||
  |#

- string_formatted

  #|
  || **Name**
  `type` | **Description** ||
  || **SIZE**
  [`integer`][1] | Input field size for display ||
  || **ROWS**
  [`integer`][1] | Number of lines in the input field ||
  || **REGEXP**
  [`string`][1] | Regular expression for validation ||
  || **MIN_LENGTH**
  [`integer`][1] | Minimum string length (0 - do not check) ||
  || **MAX_LENGTH**
  [`integer`][1] | Maximum string length (0 - do not check) ||
  || **DEFAULT_VALUE**
  [`string`][1] | Default value ||
  || **PATTERN**
  [`string`][1] | Output pattern (#VALUE# - value) ||
  |#

- vote

  TODO!!!
  Settings Preparer: Bitrix\Vote\Uf\VoteUserType::preparesettings

  #|
  || **Name**
  `type` | **Description** ||
  || **CHANNEL_ID**
  [`TODO`][1] | TODO ||
  || **UNIQUE**
  [`TODO`][1] | TODO ||
  || **UNIQUE_IP_DELAY**
  [`TODO[]`][1] | TODO ||
  || **NOTIFY**
  [`TODO`][1] | TODO ||
  |#

{% endlist %}

{% endif %}

[1]: ../../../../data-types.md