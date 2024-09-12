# Local Applications

Local applications are those that are described and added directly to a specific Bitrix24. This is the essence of the term "local."

The account administrator decides what permissions to grant such an application and how it will be named in the interface.

There are two types of such applications:

- "Static" applications based on HTML/JS. In fact, using these technologies, you can implement single-page applications by accessing the B24 REST API with the JS SDK. They are represented in the interface as a separate page (with a link from the left menu). Static applications cannot receive Bitrix24 events.
- "Server" applications with a back-end in any suitable programming languages (PHP, Python, and others). They can access the REST API using the OAuth 2.0 protocol and can also receive events from Bitrix24 in their handlers. They are presented in the interface as a separate page, as well as in the form of embedded popup dialogs in the available embedding objects of Bitrix24. There is an option where the application does not manifest itself in the interface but uses REST.

## Continue Learning

- [{#T}](static-local-app.md)
- [{#T}](serverside-local-app-with-ui.md)
- [{#T}](serverside-local-app-with-no-ui.md)