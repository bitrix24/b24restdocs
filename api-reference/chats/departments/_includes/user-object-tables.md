#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | User identifier ||
|| **active**
[`boolean`](../../../data-types.md) | User activity status ||
|| **name**
[`string`](../../../data-types.md) | User's full name ||
|| **first_name**
[`string`](../../../data-types.md) | User's first name ||
|| **last_name**
[`string`](../../../data-types.md) | User's last name ||
|| **work_position**
[`string`](../../../data-types.md) | User's job title ||
|| **color**
[`string`](../../../data-types.md) | User's color in hex format ||
|| **avatar**
[`string`](../../../data-types.md) | Link to the avatar ||
|| **avatar_hr**
[`string`](../../../data-types.md) | Link to the high-resolution avatar ||
|| **gender**
[`string`](../../../data-types.md) | User's gender ||
|| **birthday**
[`string`](../../../data-types.md) | Birthday in `DD-MM` format or an empty string ||
|| **extranet**
[`boolean`](../../../data-types.md) | External user status ||
|| **network**
[`boolean`](../../../data-types.md) | Bitrix24 Network user status ||
|| **bot**
[`boolean`](../../../data-types.md) | Bot status ||
|| **connector**
[`boolean`](../../../data-types.md) | Open Channels user status ||
|| **external_auth_id**
[`string`](../../../data-types.md) | External authorization code ||
|| **status**
[`string`](../../../data-types.md) | User status ||
|| **idle**
[`datetime`](../../../data-types.md) | User's idle date or `false` ||
|| **last_activity_date**
[`datetime`](../../../data-types.md) | User's last activity date ||
|| **mobile_last_date**
[`datetime`](../../../data-types.md) | User's last activity date in the mobile app or `false` ||
|| **desktop_last_date**
[`datetime`](../../../data-types.md) | User's last activity date in the desktop app or `false` ||
|| **absent**
[`datetime`](../../../data-types.md) | User's absence end date or `false` ||
|| **departments**
[`array`](../../../data-types.md) | Array of department identifiers ||
|| **phones**
[`object`](../../../data-types.md) | User's phones or `false` [(detailed description)](#phones) ||
|| **bot_data**
[`object`](../../../data-types.md) | Bot data or `null` ||
|| **type**
[`string`](../../../data-types.md) | User type ||
|| **website**
[`string`](../../../data-types.md) | User's website ||
|| **email**
[`string`](../../../data-types.md) | User's email ||
|#

#### Object phones {#phones}

#|
|| **Name**
`type` | **Description** ||
|| **personal_mobile**
[`string`](../../../data-types.md) | Mobile phone ||
|| **inner_phone**
[`string`](../../../data-types.md) | Internal phone ||
|#