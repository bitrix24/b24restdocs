# Overview of Installing Mass-Market Applications

Published mass-market solutions are installed by users on their Bitrix24 from the Bitrix24 Marketplace.

Additionally, during the development phase, you can install the application from the Developer's area on any Bitrix24 to which you have administrative access.

The typical scenario for creating mass-market solutions involves the developer first "creating" the application in the Developer's area, then writing code and testing it by installing the developed application on an accessible Bitrix24. Only after ensuring its full readiness does the developer submit the solution for moderation. More details on this can be found in the [relevant section](../../../market/preparing-to-publish/how-to-add-app.md).

When developing an application, you need to understand what the installation procedure will be and whether it is even necessary.

In fact, "installing the application" on a specific Bitrix24 is not about uploading a solution or downloading it (except for website templates and other "configuration solutions" that work without REST API), but rather about "registering" the application on the Bitrix24 where the installation occurs. This registration is solely needed for the authorization server to start issuing OAuth tokens for users of that specific Bitrix24.

Accordingly, depending on the user onboarding scenario for your application, you must choose the installation scenario.

There are 3 options for the installation process that will be used during the application installation:

- Addition without an installation scenario;
- [Addition with an installation wizard](./installation-master.md);
- [Addition with a callback installation](./installation-callback.md).

Most often, the second option is chosen for mass-market applications simply because during the "installation" of the application, some "one-time" procedures are often required, such as registering widget embedding locations, registering event handlers, etc.

## Addition Without an Installation Scenario

To ensure that the installation procedure is absent and the application works immediately, it is sufficient to fill in the "Application Link" field in the version card, specifying the main application URL, which will later be used for embedding the application interface into Bitrix24's left menu.

Similarly, in the case of a static application, which is an archive with HTML/JS files, you only need to have an index.html file in that archive.

In both cases, after the application is installed, it will already be available to users without any "special" installation procedure.

If your application does not have a user interface, you need to disable the option "Add your page and item to the main menu." Even though you still do not need an "installation" as such, you will still need REST API tokens for further use. In this case, you cannot do without a [callback handler](./installation-callback.md), which will receive a call from Bitrix24 immediately after the application is installed.