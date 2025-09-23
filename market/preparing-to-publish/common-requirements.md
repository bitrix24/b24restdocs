# Requirements for Formatting and Content of Solutions in Bitrix24 Market

## 1. Formatting Requirements

### A. Publication Region

- When creating a new application, select the region where your solution will be available in the future.
- You can also choose an existing solution that was previously published in another region when adding a solution to a specific region, provided that the pricing policy of your solution is available for both publication regions.

### B. Solution Card

- The title must be concise and clear for Bitrix24 users.
- The "Category Selection" list must contain a list of sections from the Bitrix24 Market catalog where your solution will be displayed:
  1. No more than 5 categories;
  2. The category **"Miscellaneous"** is used only if you cannot find a suitable category for the main functionality of your solution;
  3. To be included in the **"Integrations - Telephony"** category, the solution must meet [additional requirements](./requirements-telephony.md);
  4. To be included in the **"Vertical Solutions - Solution presets"** category, the solution must meet [additional requirements](./requirements-vertical-crm.md);
  5. To be included in the **"Automation - Workflows"** category, your solution must create [its activities for Workflows](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md);
  6. To be included in the **"Automation - Automation Rules"** or **"Automation - Triggers"** category, your solution must create [its automation rules](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md) or [CRM triggers](../../api-reference/crm/automation/triggers/crm-automation-trigger-add.md);
  7. To be included in the **"Automation - Chat bots"** category, your solution must [create chatbots](../../api-reference/chat-bots/index.md) for the Bitrix24 messenger or Open Channels.

{% note alert "" %}

In the **"Automation - Workflow"**, **"Automation - Automation Rules"**, and **"Automation - Triggers"** categories, the publication of separate applications from the same developer is not allowed if each of these applications adds only a minor number of workflow activities, automation rules, and triggers. We recommend consolidating such functionality within a single application in the form of a "collection" of activities and automation rules.

{% endnote %}

- In the Tags section for collections, select up to 5 tags from the list that correspond to the theme of your solution.

{% note info "" %}

These tags are not keywords for search but are intended for user navigation within collections and categories. Therefore, you should choose tags that help group your solution with other solutions that address similar user needs.

If you feel that you lack the right tags, please contact the moderators in the chat.

{% endnote %}

#### Application Description Block:

1. In the **Application Description** block, depending on the publication region, descriptions are required in the mandatory language for that region. Descriptions in other available languages are optional.
2. The **Short Description** of the application should contain 1-2 sentences describing the main purpose of your solution.
3. It is not recommended to duplicate the application title in the short description. Avoid introductory words; formulate the description so that it complements the title. Remember that the short description will be displayed in application lists and should help you attract the user's interest to open your solution card.
4. The **Full Description** of the solution should contain a list of tasks that your product solves and the capabilities it provides to Bitrix24 users. **You must indicate in the description the features of the solution that limit or affect its user properties, in particular:**
   - Requirements for a specific Bitrix24 tariff plan;
   - The need for additional software for the solution to work;
   - The need to connect/pay for external systems and services, as well as additional functionality of the application (see point 4 below);
   - Specific requirements for user qualifications for installation and operation with the solution;
   - Specific requirements for user rights (rights to administer the account, or CRM administration, etc.);
   - The need to install additional partner solutions;
   - Functional limitations of the solution related to its technical features (for example, "Supports no more than 1000 deals," "Processes only the last 200 tasks," etc.).
5. If your solution requires or offers an **optional opportunity to purchase additional functionality of the solution**, payment for external paid services and integrations, etc., then the Description of the solution must explicitly indicate the possibility or necessity of such additional payment, including a clear indication of pricing options.
6. When mentioning the trade names of the Bitrix24 company, please use the correct names, for the market – Bitrix24 Market.
7. Use the **Keywords** field to list search words that will help users find your solution in the application catalog. **Requirements for filling:**
   - Keywords must correspond to the actual functionality of the solution; arbitrary sets of words cannot be used;
   - No more than 2000 characters;
   - No search spam (including mentioning "Bitrix24" and similar brands related to your integration);
   - Keywords and phrases must be separated by commas.
8. The text in the **"Installation Process Description"** field should include a list of actions necessary for using the solution:
   - If the solution contains settings, indicate where to find them and describe specific options;
   - If the solution includes integration with an external system/service, specify how the integration is configured within the application and on the external service side; the description should be written for a user who is not familiar with the system/service – links to general user documentation are not sufficient.
9. The **"Support and Feedback Information"** field must contain all available channels for official communication with clients: e-mail, phone, link to feedback form or helpdesk, social media groups. Be sure to specify the technical support operating schedule – working days and hours, response time to incoming requests. If the solution is published in English, this field should not contain links to Russian-language resources (website, helpdesk, etc.).
10. **Icon** - file in JPEG and PNG format (without a transparent background), square, size no less than 250x250 pixels and no more than 650x650 pixels. Low-quality images with blurriness, noticeable pixelation, and those using third-party trademarks, including trademarks and logos owned by 1C-Bitrix, are not allowed.
11. **Screenshots:**
    - Must contain examples of your solution's interface demonstrating demo data relevant to the solution in the context of Bitrix24.
    - Demo data must be as realistic as possible and demonstrate to the end user the process of setting up and using the application. Screenshots with empty fields, test users, empty reports, etc., are not accepted.
    - If the solution has its interface within Bitrix24, the application must be open in the slider on the screenshots. Screenshots highlighting details of the internal interface (e.g., enlarged images of options, etc.) are allowed as a supplement to full-screen screenshots.
    - The size of screenshots must not exceed 1280 by 720 pixels.
    - Supported formats are JPEG and PNG.
12. You must provide a link to your own user license agreement in the **"Use my license agreement"** field, as well as a link to your own privacy policy in the **"Use my privacy policy"** field, drafted in accordance with the current legislation of the publication region. The license agreement defining the rights of the user to use your application must contain the name of the solution and the licensor. For your convenience, we have provided links to examples of such documents nearby. **Important:** the link must lead not to a download but to view the corresponding documents in the browser. Supported document formats: html, pdf, txt.
13. Optionally, you can provide links for **"Contact the Developer"** and **"Request a Demo."** These should be links to a page about your product with any contact form (messenger, online chat, etc.).

#### Pricing Policy Block

- **"The application will be distributed for free"** - users will be able to install the solution for free and without time limitations.
- **"Users must pay for the external service"** - an additional option for free solutions when payment for an external service is required for the application to work.

- **"Contains additional paid functionality"** - an additional option for subscription applications that must be included if your solution offers users additional functionality for an additional payment.

{% note info "" %}

Be sure to familiarize yourself with the [document flow rules](../agreements.md) for each type of sales terms you use.

{% endnote %}

### C. Solution Version Card

- In the **"The application will work with system sections"** field, include only those rights that are truly necessary for the functionality of the current version of the solution. It is prohibited to include sections that will only be needed in future versions or updates.
- The system section **"Users"** in the basic and full version is granted only if their absence would make the main functionality of the solution impossible; in other cases, only the minimum scope is allowed. You can familiarize yourself with the differences in the [documentation](../../api-reference/user/index.md).
- **Select the technological type of your solution:**
  1. **Use REST API** – if your solution works on the Bitrix24 REST API;
  2. **Solution preset** – if you are publishing an industry-specific CRM obtained through the built-in CRM settings export mechanism. Please note that there is [additional regulation](./requirements-vertical-crm.md) for this type of solution;
  3. **Add Smart Scripts** – if you are publishing a solution with smart scripts obtained through the built-in settings export mechanism in Bitrix24. Please note that there is [additional regulation](./requirements-smart-scripts.md) for this type of solution;
  4. **Website Templates** – if you are publishing a template for Website Templates. Please note that there is [additional regulation](./requirements-sites.md) for this type of solution;
  5. **Reports for BI Constructor** – if you are publishing a configured report template. Please note that there is [additional regulation](./requirements-superset.md) for this type of solution;
- **In "Use REST API" mode**
  1. Enable the **"Add custom page and menu item"** option only if your solution needs a link to appear in the left menu of Bitrix24 accounts. This option is usually not used for solutions that do not imply a user interface within Bitrix24 accounts, particularly for chatbots.
  2. Enable the **"Embed to BitrixMobile"** option only if your application adds its page and item to the main menu, and the layout of this page is fully responsive.
  3. Enable the **"Add widgets"** option only if your application adds widgets to available embedding locations. More about embedding can be found in the [REST API guide](../../api-reference/widgets/index.md).
  4. Enable the **"Can be installed by user without administrative rights"** option if the logic of your application does not require administrator privileges for the corresponding REST API calls and will work fully with the tokens of a regular Bitrix24 user.
  5. The **"Application URL"** field must contain the URL of the server-type application, or you must upload an archive with a static HTML/JS application using the corresponding button below.
  6. The **"Application installer URL"** field must contain the URL that will be called when the user installs the solution on their Bitrix24. Details about installation can be found [here](../../settings/app-installation/mass-market-apps/installation-master.md).
- **What's New in the Application** – must contain a list of differences of the current version from previous ones. If the solution description was made in several languages, then the version changes need to be in all these languages.

{% note info "" %}

For the first version of the application, this field does not need to be filled; the information will be automatically taken from the application description.

{% endnote %}

### D. The solution will not be published in the Bitrix24 Market catalog if

- **The solution card contains:**
  1. Offensive, obscene information in the card fields: calls to violence, pornography, racism, and other materials prohibited by government legislation;
  2. Information that violates Federal laws in the solution description.
  3. Unfilled fields "Solution Installation," "Solution Description," "Support," icon, and screenshots;
  4. Information that does not correspond to the fields (contacts in the description, solution description in support, discounts in the installation description, etc.), or meaningless information;
  5. Offers to purchase the solution, partner services, and/or Bitrix24 products directly from the partner;
  6. Descriptions of promotions, special offers, and discounts on partner solutions and/or Bitrix24 products when purchased directly from the partner;
  7. Obvious grammatical errors in the description fields;
  8. Texts in descriptions or screenshots that do not correspond to the selected description language.
- **ii. The version card will contain:**
  1. Offensive, obscene information in the version fields: calls to violence, pornography, racism, and other materials prohibited by Russian legislation;
  2. Unfilled "What's New in the Application" field, or text that does not correspond to the selected description language;
  3. Information that does not correspond to the fields (title in the description, discounts in the version description, etc.), or meaningless information;
  4. Offers to purchase the solution, partner services, and/or 1C-Bitrix products directly from the partner;
  5. Descriptions of promotions, special offers, and discounts on partner solutions and/or Bitrix24 products when purchased directly from the partner;
  6. Excessive rights to Bitrix24 modules without obvious connection to the functionality of the solution;
  7. Obvious grammatical errors in the title and description;

## 2. Content and Functionality Requirements

- The solution must expand the user functionality of Bitrix24, using the built-in tools of the platform or integrating Bitrix24 with external systems.
- Integration implies automatic data exchange between Bitrix24 and an external system. A simple call to the interface of an external system from the Bitrix24 interface within an iframe is not considered integration – such solutions may be published in exceptional cases at the discretion of Bitrix24.
- The solution is not allowed to request the user's login and password for authorization in Bitrix24. For explicit authorization, the OAuth 2.0 protocol must be used, the implementation of which is described in the corresponding [section](../../settings/oauth/index.md).
- A modern and aesthetically appealing design, close to the Bitrix24 interface, is expected. Applications with a simple HTML-based interface will not be published.
- All demo data of the solution must not be real. Real phone numbers, email addresses, links to social networks, etc., are not allowed.
- If the solution implements the collection of personal data from clients:
  1. This must not violate the provisions of the law regulations;
  2. For organizing advertising mailings, separate consent from the users of the application must be obtained. Consent is also optional, and refusal cannot be a reason for partial or complete disabling of the application's functionality.
- Additional requirements for chatbot functionality:
  1. If the solution implements a chatbot, after inviting the chatbot to the chat, the chatbot must display a short reference about its functionality as a greeting, as well as a list of available commands or provide the user with a clear opportunity to obtain extended information about its commands;
  2. The chatbot must have its own avatar.
- The server part of the solution must use a valid SSL certificate, at least a Domain Validation (DV) Certificate.
- The solution must work correctly in the on-premise Bitrix24.
- The application must not overload the server with excessive requests to the REST API:
  1. Always pre-validate requests on the application side and do not allow requests with inherently incorrect data;
  2. Control and handle errors in API interactions and do not resend incorrectly formed requests;
  3. If possible, cache data on the application side and do not make repeated requests whose results cannot change during the user's session with the application;
  4. If multiple requests need to be made in succession, use the [batch request mechanism](../../settings/how-to-call-rest-api/batch.md) whenever possible.
- Solutions making requests to the Bitrix24 REST API from the server must keep logs of API requests and responses for at least the last 3 days.
- The solution will not be published in the Bitrix24 Market catalog if:
  1. Upon installation, the solution generates an empty page within the frame or a page with content without automatic redirection to the solution interface after installation is complete;
  2. The security scanner detects issues during the solution check;
  3. Offensive, obscene information is present in the interface or demo data: calls to violence, pornography, racism, and other materials prohibited by government legislation;
  4. The functionality of the solution does not correspond to what is stated in the title and description;
  5. The solution is a clone of other solutions from the same developer in terms of graphic design and/or demo content;
- The functionality of the solution reveals:
  1. Texts with incorrect encoding;
  2. PHP Warnings, Fatal Errors, Syntax Errors, and/or Parse Errors, and similar messages from another server platform on which the solution is implemented;
  3. Database errors;
  4. Incorrect layout of the user interface, vertical or horizontal scroll bars (at a resolution of at least 1280 px) in the frame;
  5. JavaScript errors;
  6. Links to non-existent or empty pages;
  7. Links to non-existent images, style files, etc.;
  8. Descriptions of promotions, special offers, and discounts on partner modules and/or Bitrix24 products when purchased directly from the partner;
  9. Obvious grammatical errors in the user interface;
  10. The interface of the solution is made in simple HTML;
  11. Real demo data is used when installing the solution. Links, phone numbers, etc.;

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./requirements-sites.md)
- [{#T}](./requirements-smart-scripts.md)
- [{#T}](./requirements-superset.md)
- [{#T}](./requirements-telephony.md)
- [{#T}](./requirements-vertical-crm.md)