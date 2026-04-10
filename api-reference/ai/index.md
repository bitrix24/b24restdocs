# AI in Bitrix24: Overview of Methods

The `ai` section helps connect your own AI service to Bitrix24. The methods in this section allow you to register new services, retrieve their list, and remove selected ones.

{% note warning "Important" %}

Before registration, check the availability of the external handler URL in the `completions_url` parameter and the requirements for processing requests.

{% endnote %}

> Quick navigation: [all methods](#all-methods)

## What to Consider Before Registration

- Specify a valid URL in `completions_url` that returns an HTTP status of `200` upon verification.

- For the `image` category, use asynchronous processing.

- Choose the `category` value based on the service scenario: `text`, `image`, `audio`, or `call`.

- Pass additional parameters in `settings`.

## How to Get Started

1. Prepare the external endpoint.

2. Register the service via [ai.engine.register](./ai-engine-register.md).

3. Verify the registration and service parameters through [ai.engine.list](./ai-engine-list.md).

4. Remove the service via [ai.engine.unregister](./ai-engine-unregister.md) if it is no longer needed.

## How Integration Works

**Endpoint.** The application sends requests from Bitrix24 to the external endpoint via `completions_url`.

**Callback Mechanism.** After processing, the service sends the result to `callbackUrl` and error information to `errorCallbackUrl`.

{% note tip "Endpoint Template" %}

You can use this template as a basis for your own service.

[Download Template](https://helpdesk.bitrix24.com/examples/endpoint.zip)

{% endnote %}

## Connection with the Application and Access

**Application.** The service is linked to the application via the `APP_CODE` field. This can be obtained in the response from the [ai.engine.list](./ai-engine-list.md) method. In the context of an OAuth application, the list contains only the services of the current application.

**Webhook.** You can filter services by any `APP_CODE` through the webhook.

## Overview of Methods   {#all-methods}

> Scope: [`ai_admin`](../scopes/permissions.md)
>
> Who can execute the method: administrator

#| 
|| **Method** | **Description** ||
|| [ai.engine.register](./ai-engine-register.md) | Registers a custom AI service ||
|| [ai.engine.list](./ai-engine-list.md) | Retrieves a list of registered AI services ||
|| [ai.engine.unregister](./ai-engine-unregister.md) | Removes a registered AI service ||
|#