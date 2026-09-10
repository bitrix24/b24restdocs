# Meetups, Broadcasts, and Recordings for Developers

Bitrix24 webinars show how developers solve tasks with the REST API: make a first call with a webhook, build local applications, embed widgets, receive offline events, and automate CRM. Below are recordings of the past episodes of the Bitrix24 REST API webinar series, listed from the latest episode to the earliest. Where an episode has a code example, it is built with [B24PhpSDK](https://github.com/bitrix24/b24phpsdk). Announcements are published in the community channels listed on the [Support and Community for Developers](./support.md) page.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "Recordings reflect the API as it worked at the time of the webinar" %}

The approach and the logic of a solution stay relevant, but methods, parameters, and responses may have changed. Check the [REST API Reference](./api-reference/index.md) and [What's New](./whats-new.md) before you build.

{% endnote %}

## How to Choose a Recording {#how-to-choose}

#|
|| **Task** | **Recording** ||
|| Add your own activity to the CRM timeline | [Episode 9. Integrating Third-Party E-Signing Services into CRM Timeline Widgets](#e-signing-timeline) ||
|| Build a custom widget for the Bitrix24 Vibe start page | [Episode 8. Bitrix24 Vibe: Custom Widgets for the Start Page](#vibe-widgets) ||
|| Keep OAuth tokens in a local application between requests | [Episode 7. Quick Start: Local Application with Token Storage](#local-app-token-storage) ||
|| Build a local application that reads its token from the request | [Episode 6. Quick Start: Local Application](#local-app) ||
|| Make a first REST call without registering an application | [Episode 5. Quick Start with an Inbound Webhook](#incoming-webhook) ||
|| Receive changes from Bitrix24 while your service is offline | [Episode 4. Offline Events](#offline-events) ||
|| Register your own automation rules and triggers | [Episode 3. Automation Rules and Triggers](#automation-rules) ||
|| Embed your application into the Bitrix24 interface | [Episode 2. Widgets, Tips, and Tricks](#widgets) ||
|| Get an overview of the Bitrix24 developer ecosystem | [Episode 1. Bitrix24 Developer's Globe](#developers-globe) ||
|#

## Episode 9. Integrating Third-Party E-Signing Services into CRM Timeline Widgets {#e-signing-timeline}

Materials:

- [PHP example on GitHub](https://github.com/bitrix24/b24sdk-examples/tree/main/php/special/custom-activities) — source code of custom timeline activities
- [Configurable CRM Activities](./api-reference/crm/timeline/activities/configurable/index.md) — methods that create your own timeline activities

@[youtube](https://www.youtube.com/watch?v=IJ8s5T_htck)

## Episode 8. Bitrix24 Vibe: Custom Widgets for the Start Page {#vibe-widgets}

Materials:

- [PHP example on GitHub](https://github.com/bitrix24/b24sdk-examples/tree/main/php/special/vibe24-widgets) — source code of Vibe widgets
- [Vibe](./api-reference/vibe/index.md) — methods for widgets and interactive elements of Vibe

@[youtube](https://www.youtube.com/watch?v=jdZUN3ygWaE)

## Episode 7. Quick Start: Local Application with Token Storage {#local-app-token-storage}

Materials:

- [PHP example on GitHub](https://github.com/bitrix24/b24sdk-examples/tree/main/php/quick-start/local-apps/token-storage-in-file) — local application that stores tokens in a file
- [Local Applications](./local-integrations/local-apps.md) — types of local applications and how to register them
- [Security Recommendations](./settings/cloud-and-on-premise/security-recommendations.md) — rules for handling and verifying tokens

@[youtube](https://www.youtube.com/watch?v=eE-YqwxmzBk)

## Episode 6. Quick Start: Local Application {#local-app}

Materials:

- [PHP example on GitHub](https://github.com/bitrix24/b24sdk-examples/tree/main/php/quick-start/local-apps/token-from-request) — local application that takes the token from the request
- [Local Applications](./local-integrations/local-apps.md) — types of local applications and how to register them
- [Episode 7. Quick Start: Local Application with Token Storage](#local-app-token-storage) — next step in the same track: keeping tokens between requests

@[youtube](https://www.youtube.com/watch?v=bgbzmq63EsM)

## Episode 5. Quick Start with an Inbound Webhook {#incoming-webhook}

Materials:

- [PHP example on GitHub](https://github.com/bitrix24/b24sdk-examples/tree/main/php/quick-start/simple/02a-webhook-demo) — webhook demo built with the SDK
- [Inbound and Outbound Webhooks](./local-integrations/local-webhooks.md) — how to create a webhook and what it can access
- [Episode 6. Quick Start: Local Application](#local-app) — next step in the same track: a local application instead of a webhook

@[youtube](https://www.youtube.com/watch?v=H5rBky_DJ4c)

## Episode 4. Offline Events {#offline-events}

Speaker:

- Serg Vostrikov, Head of Market and Integrations

Materials:

- [Offline Events](./api-reference/events/offline-events.md) — how the event queue works
- [Events](./api-reference/events/index.md) — handler registration and the list of events

@[youtube](https://www.youtube.com/watch?v=mlWlP6SwHTg)

## Episode 3. Automation Rules and Triggers {#automation-rules}

Speaker:

- Serg Vostrikov, Head of Market and Integrations

Materials:

- [Application Automation Rules](./api-reference/bizproc/bizproc-robot/index.md) — methods that register your own automation rules
- [CRM Automation](./api-reference/crm/automation/index.md) — methods that register your own triggers

@[youtube](https://www.youtube.com/watch?v=vw2nVeXU4Pk)

## Episode 2. Widgets, Tips, and Tricks {#widgets}

Speaker:

- Serg Vostrikov, Head of Market and Integrations

Materials:

- [Widget Embedding Mechanism](./api-reference/widgets/index.md) — embedding points and widget registration

@[youtube](https://www.youtube.com/watch?v=AeuueGJ_5qg)

## Episode 1. Bitrix24 Developer's Globe {#developers-globe}

Materials:

- [Tools for Local Integrations](./local-integrations/index.md) — webhooks and local applications for your own Bitrix24

@[youtube](https://www.youtube.com/watch?v=57eaBHp2EuI)

## Continue learning {#next-steps}

- [What the Bitrix24 REST API Allows](./developing-with-rest-api.md) — overview of the platform capabilities
- [REST API Reference](./api-reference/index.md) — methods, events, and widgets by section
- [SDK for Bitrix24 Development](./sdk/index.md) — official libraries for PHP, JavaScript, Python, and Go
- [Support and Community for Developers](./support.md) — courses, chats, and technical support
- [What's New](./whats-new.md) — changes in the REST API and documentation
