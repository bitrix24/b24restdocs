# Key Provisions

The Bitrix24 API reference contains descriptions of all methods and events of the REST API, as well as a number of additional topics important for a deep understanding and effective use of the API in development.

## Reading Sequence of Materials

Key sections of the API that provide an overview of the most important capabilities:

```mermaid
%%{init: { "theme": "forest" } }%%
flowchart LR
    1[1. Authorization, 
    OAuth 2.0]
    2[2. Installing and uninstalling 
    applications]
    3[3. Embedding 
    widgets]
    4[4. Handling 
    events]
    5[5. Differences between cloud 
    and on-premise]
    1 --> 2
    2 --> 3
    3 --> 4
    4 --> 5
```

To facilitate the process of studying and using the Bitrix24 REST API, the following reading sequence is suggested:

1. [{#T}](how-to-call-rest-api/authorization.md): This section will help you quickly check how the REST API works and familiarize you with the basic principles of making requests.
2. [{#T}](data-types.md): Understanding the data types used in the REST API is critically important for correctly working with methods and events.
3. [{#T}](oauth/index.md): This section describes the authorization mechanisms based on the OAuth 2.0 standard, obtaining and renewing tokens in applications.
4. [{#T}](app-installation/index.md) and [{#T}](app-uninstallation.md): This section discusses the processes of installing and uninstalling applications in Bitrix24 accounts, which is especially important for developers of mass-market applications.
5. [{#T}](scopes/permissions.md): It explains the specifics of application and webhook access to various REST API methods, allowing you to configure the necessary level of access.
6. [{#T}](widgets/index.md): This section describes ways to integrate custom widgets into the Bitrix24 interface to enhance its functionality.
7. [{#T}](events/index.md): This section is dedicated to the REST API event mechanism, which allows tracking changes in data and responding to them with special server handlers.
8. [{#T}](interactivity/index.md): It discusses ways to create interactive applications that utilize the REST API capabilities for interaction between back-end applications and their front-end.
9. [{#T}](performance/limits.md): Important aspects related to application performance and limitations imposed on the use of the REST API.
10. [{#T}](bx24-js-sdk/index.md): A section dedicated to the JavaScript SDK, which simplifies working with the REST API on the client side.
11. [{#T}](crest-php-sdk/index.md): An overview of the PHP SDK for working with the REST API, providing convenient tools for server-side development.
12. [{#T}](cloud-and-on-premise/index.md): It clarifies the key differences between the cloud and on-premise versions of the platform in terms of using the REST API.
13. [{#T}](common/index.md): A description of common methods available in the REST API that can be used in various applications.

{.large-list}

## Bitrix24 Tool

Bitrix24 is a comprehensive product that combines many different tools integrated with each other. This integration allows developers to offer users complete business scenarios using multiple tools.

The API reference contains descriptions of available methods, events, and widgets for the corresponding Bitrix24 tools.

1. [{#T}](./common/index.md)
2. [{#T}](./biconnector/index.md)
3. [{#T}](./crm/index.md)
4. [{#T}](./ai/index.md)
5. [News Feed](./log/index.md)
6. [{#T}](./sale/index.md)
7. [Users](./user/index.md)
8. [Workflows](./bizproc/index.md)
9. [Tasks](./tasks/index.md)
10. [Document Generator](./document-generator/index.md)
11. [{#T}](./calendar/index.md)
12. [Payment Systems](./pay-system/index.md)
13. [{#T}](./departments/index.md)
14. [{#T}](./user-consent/index.md)
15. [Workgroups and Projects](./sonet-group/sonet-group-create.md)
16. [Open Channels](./imopenlines/index.md)
17. [Online Booking](./booking/index.md)
18. [Chatbots](./chat-bots/index.md)
19. [Chats](./chats/index.md)
20. [Sites and Stores](./landing/index.md)
21. [Message Providers, SMS Providers](./messageservice/index.md)
22. [Universal Lists](./lists/index.md)
23. [Time Management](./timeman/index.md)
24. [Data Storage](./entity/index.md)
25. [Product Catalog](./catalog/index.md)
26. [Telephony](./telephony/index.md)
27. [Drive](./disk/index.md)
28. [Mail Services](./mailservice/index.md)