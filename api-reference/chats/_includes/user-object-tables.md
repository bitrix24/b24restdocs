#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | User identifier ||
|| **active**
[`boolean`](../../data-types.md) | User account activity status ||
|| **name**
[`string`](../../data-types.md) | User's full name ||
|| **first_name**
[`string`](../../data-types.md) | User's first name ||
|| **last_name**
[`string`](../../data-types.md) | User's last name ||
|| **work_position**
[`string`](../../data-types.md) | User's job title ||
|| **color**
[`string`](../../data-types.md) | User's color in HEX format ||
|| **avatar**
[`string`](../../data-types.md) 
[`null`](../../data-types.md) | Link to the user's avatar ||
|| **avatar_hr**
[`string`](../../data-types.md) 
[`null`](../../data-types.md) | Link to the high-resolution avatar. Currently, this field always returns a value, regardless of the `AVATAR_HR` setting ||
|| **gender**
[`string`](../../data-types.md) | User's gender: `M`, `F`, or empty if not specified ||
|| **birthday**
[`string`](../../data-types.md) | Birthday in `DD-MM` format or an empty string ||
|| **extranet**
[`boolean`](../../data-types.md) | Extranet user status ||
|| **network**
[`boolean`](../../data-types.md) | Bitrix24 Network user status ||
|| **bot**
[`boolean`](../../data-types.md) | Bot user status ||
|| **connector**
[`boolean`](../../data-types.md) | Open Channels connector user status ||
|| **external_auth_id**
[`string`](../../data-types.md) | External authentication identifier ||
|| **status**
[`string`](../../data-types.md) | User status.

In the new messenger, this field always contains `online` regardless of the actual status set. The current status can be checked using the [im.user.status.get](../users/im-user-status-get.md) method, and changed using the [im.user.status.set](../users/im-user-status-set.md) method ||
|| **idle**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Time of transition to "Away" status in ISO 8601 (RFC3339) format or `false` ||
|| **last_activity_date**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Time of last activity in ISO 8601 (RFC3339) format or `false` ||
|| **mobile_last_date**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Time of last activity in the mobile application in ISO 8601 (RFC3339) format or `false` ||
|| **desktop_last_date**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Time of last activity in the desktop application in ISO 8601 (RFC3339) format or `false` ||
|| **absent**
[`string`](../../data-types.md) 
[`boolean`](../../data-types.md) | Date of absence end in ISO 8601 (RFC3339) format or `false` ||
|| **departments**
[`array`](../../data-types.md) | Array of department identifiers ||
|| **phones**
[`object`](../../data-types.md) 
[`boolean`](../../data-types.md) | User's phones or `false`.

The structure of the object is described in detail [below](#phones-object) ||
|| **website**
[`string`](../../data-types.md) | User's website or an empty string ||
|| **email**
[`string`](../../data-types.md) | User's email or an empty string ||
|| **bot_data**
[`object`](../../data-types.md) 
[`null`](../../data-types.md) | Bot data for bot users ||
|| **type**
[`string`](../../data-types.md) | User type ||
|#

### Phones Object {#phones-object}

#|
|| **Name**
`Type` | **Description** ||
|| **personal_mobile**
[`string`](../../data-types.md) | Mobile phone ||
|| **work_phone**
[`string`](../../data-types.md) | Work phone ||
|| **inner_phone**
[`string`](../../data-types.md) | Internal phone ||
|#