# Event for Creating a New CRM Activity of Type "Comment" onCrmTimelineCommentAdd

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

This event triggers when a new CRM activity of type "Comment" is added to the CRM timeline.

## What the Handler Receives

Data is sent as a POST request {.b24-info}

```php
array(
    'event' => 'onCrmTimelineCommentAdd',
    'data' => array(
        'ID' => 999,
    ),
    'ts' => '1466439714',
    'auth' => array(
        'access_token' => 's6p6eclrvim6da22ft9ch94ekreb52lv',
        'expires_in' => '3600',
        'scope' => 'crm',
        'domain' => 'some-domain.bitrix24.com',
        'server_endpoint' => 'https://oauth.bitrix.info/rest/',
        'status' => 'F',
        'client_endpoint' => 'https://some-domain.bitrix24.com/rest/',
        'member_id' => 'a223c6b3710f85df22e9377d6c4f7553',
        'refresh_token' => '4s386p3q0tr8dy89xvmt96234v3dljg8',
        'application_token' => '51856fefc120afa4b628cc82d3935cce',
    ),
)
```

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | Symbolic code of the event. In our case, it is `onCrmTimelineCommentAdd`||
|| **data***
`array` | Array containing the data of the added element ||
|| **ts***
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../../events/index.md) ||
|| **auth***
[`array`](../../../data-types.md) | Authorization parameters and information about the account where the event occurred ||
|#

### Parameter data[]

{% include notitle [Note on Parameters](../../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID***
[`integer`](../../../data-types.md) | `ID` with the value of the added comment's identifier ||
|#

### Parameter auth[]

{% include notitle [Table with Keys in the auth Array](../../../../../_includes/auth-params-in-events.md) %}

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./on-Crm-Timeline-Comment-Update.md)
- [{#T}](./on-Crm-Timeline-Comment-Delete.md)