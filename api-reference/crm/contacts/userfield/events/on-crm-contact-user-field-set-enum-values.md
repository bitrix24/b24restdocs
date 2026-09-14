# Event on Changing the Set of Values for a Custom List-type Field onCrmContactUserFieldSetEnumValues

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onCrmContactUserFieldSetEnumValues` is triggered when the set of values for a custom list-type field on contacts is changed.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
  "event": "ONCRMCONTACTUSERFIELDSETENUMVALUES",
  "event_handler_id": "17",
  "data": {
    "FIELDS": {
      "ID": "554",
      "ENTITY_ID": "CRM_CONTACT",
      "FIELD_NAME": "UF_CRM_1724771514"
    }
  },
  "ts": "1724771547",
  "auth": {
    "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
    "expires_in": "3600",
    "scope": "crm",
    "domain": "some-domain.bitrix24.com",
    "server_endpoint": "https://oauth.bitrix.info/rest/",
    "status": "F",
    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
    "member_id": "a223c6b3710f85df22e9377d6c4f7553",
    "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
    "application_token": "51856fefc120afa4b628cc82d3935cce"
  }
}
```

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../../data-types.md) | Symbolic code of the event.

In this case — `ONCRMCONTACTUSERFIELDSETENUMVALUES` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | An object containing information about a custom field whose list of possible values has changed.

Contains a single key `FIELDS` ||
|| **data.FIELDS**
[`object`](../../../../data-types.md) | An object containing the properties of a list-type custom field.

The structure is described [below](#fields) ||
|| **ts**
[`timestamp`](../../../../data-types.md) | Date and time of the event sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`](../../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter FIELDS {#fields}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the custom field whose list of possible values has changed ||
|| **ENTITY_ID**
[`userFieldEntityId`](../../../data-types.md#object_type) | Type of CRM object to which the custom field is attached.

In this case — `CRM_CONTACT` ||
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Code of the custom field whose list of possible values has changed ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./on-crm-contact-user-field-add.md)
- [{#T}](./on-crm-contact-user-field-update.md)
- [{#T}](./on-crm-contact-user-field-delete.md)