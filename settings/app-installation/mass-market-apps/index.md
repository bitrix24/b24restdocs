# Overview of Installing Mass-Market Applications

Published mass-market solutions are installed by users on their Bitrix24 from the Bitrix24 Marketplace.

Additionally, during the development phase, you can install the application from the Developer's area on any Bitrix24 to which you have administrative access.

The typical scenario for creating mass-market solutions involves the developer first "creating" the application in the Developer's area, then writing and testing the code by installing the developed application on an accessible Bitrix24, and only after ensuring its full readiness, submitting the solution for moderation. More details on this can be found in the [relevant section](../../../market/preparing-to-publish/how-to-add-app.md).

When developing an application, you need to understand what the installation procedure will be and whether it is necessary at all.

In fact, "installing an application" on a specific Bitrix24 is not about uploading a solution or downloading it (except for site templates and other "configuration solutions" that work without REST API), but rather about "registering" the application on the Bitrix24 where the installation is taking place. This registration is solely needed for the authorization server to start issuing OAuth tokens for users of that specific Bitrix24.

Accordingly, depending on the onboarding scenario for users into your application, you should choose the installation scenario.

There are 4 options for the installation process that will be used during the application installation:

- Adding without an installation scenario;
- [Adding with the installation wizard](./installation-master.md);
- [Adding with the setup wizard for REST-only applications](./rest-only-installation-master.md);
- [Adding with a callback installation call](./installation-callback.md).

Most often, the second option is chosen for mass-market applications simply because "installing" the application often requires performing some "one-time" procedures such as registering widget embedding locations, registering event handlers, etc.

## Adding Without an Installation Scenario

To ensure that the installation procedure, as such, is absent and the application works immediately, it is sufficient to fill in the "Application Link" field in the version card, specifying the main application URL, which will later be used to embed the application's interface into Bitrix24 in the left menu.

Similarly, in the case of a static application, which is an archive with html/js files, you only need to have an index.html file in that archive.

In both cases, after the application is installed, it will already be available to users without any "special" installation procedure.

If your application does not have a user interface, you need to disable the option "Add your page and item to the main menu." And then, despite the fact that you still do not need an "installation," you will still need REST API tokens for further use. In this case, you cannot do without a [callback handler](./installation-callback.md), which will receive a call from Bitrix24 immediately after the application is installed.