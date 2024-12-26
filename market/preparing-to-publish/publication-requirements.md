# Procedure for Publishing Solutions in Bitrix24 Market

## 1. General Information

**A**. Solutions that expand the capabilities of existing Bitrix24 tools are accepted for publication in Bitrix24 Market, such as:

- Integrations of Bitrix24 with various external systems (telephony, SMS providers, payment systems, e-mail newsletters, etc.);
- Industry-specific solutions based on Bitrix24;
- Templates for Sites24;
- Smart scenarios;
- Additional reporting systems, document management tools, tools for automating specific user scenarios, chatbots for Bitrix24, etc.;
- Actions for the standard business process builder, automation rules, and CRM triggers;
- Connectors for Open Channels to messengers and social networks;
- Reports for the BI constructor.

**B**. Published solutions can be free or paid:

- **Free solutions** can be applications that provide users with full functionality all at once, without time limitations. The available functionality of the application should not change during operation, nor should it be turned on or off based on additional conditions or payments. Such solutions may include:
  1. Solutions for migrating user data from third-party products and platforms to Bitrix24.
  2. Integrations with free or paid external systems;
  3. Others;
- Developers of solutions may also offer users to pay for additional functionality, removal of built-in limits, etc.** Payments from clients for additional functionality can be accepted independently. Prices and all conditions related to payment must be clearly and thoroughly stated in the solution description so that users can review them in advance.

**C**. The publication of a solution in the Market occurs in stages:

- You add the solution yourself;
- You fill out the solution card and version of the solution yourself;
- You conduct installation testing of the solution on your Bitrix24;
- You independently check the functionality;
- You submit the solution for review by the moderator by clicking the **"Submit For Moderation"** button in the list of solutions;
- The moderator approves the solution/version or sends it back for revision, changing the solution to the appropriate status;

## 2. Adding a Solution

- A solution can be added in the [Developer's area](https://vendors.bitrix24.com/).
- Familiarize yourself with the technical [documentation on REST API](../../api-reference/index.md) and the [video course](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268&INDEX=Y).
- When developing and publishing your solution, adhere to the [“Requirements for the Design and Content of Solutions in Bitrix24 Market”](./publication-requirements.md), which you agree to when submitting for moderation.

## 3. Independent Testing of the Solution

**A**. You must ensure that your solution installs successfully in Bitrix24. To do this, you should conduct testing from the card of the published version of the solution by clicking the "Test" button and specifying the necessary Bitrix24 to which you have access:

- Conduct installation testing on a "clean" Bitrix24 where your solution has never been installed before;
- After verification, delete the solution and ensure that any saved user or authorization data from the current Bitrix24 has been removed from internal databases (if your solution is a server application);
- Reinstall the solution on the same Bitrix24 to ensure the installation script works in situations where users will attempt to install your solution multiple times;

**B**. You must ensure the functionality of your solution works:

- Conduct functionality testing on a "clean" Bitrix24 where your solution has never been installed before;
- If the functionality of the solution depends on the rights of a specific user, check user scenarios separately as a Bitrix24 administrator and separately as a regular user;
- Delete and reinstall the solution on the same Bitrix24 to ensure its functionality in situations where users will attempt to install your solution multiple times;
- Do not test your solution before submission on the same Bitrix24 where it was originally developed.

## 4. Moderation

- Adding a solution to the developer's list does not mean it automatically enters the moderation queue. You must click the **"Submit For Moderation"** button for the solution you wish to publish.
- Moderators return the solution for revision if they find at least one error or non-compliance with the [“Requirements for the Design and Content of Solutions in Bitrix24 Market”](./publication-requirements.md). If the solution does not pass moderation, you will need to resubmit it after correcting the issues. The solution will go to the end of the moderation queue.
- The moderator will report any found errors in the chat available in the Developer's area.
- Moderation timelines are not regulated and depend on the current workload of the moderators.
- A successfully moderated solution may be removed from the catalog due to subsequently discovered issues.
- A solution may be denied publication in the Bitrix24 Market catalog without explanation by the moderators.
- If the solution implements integration with an external system/service, you must provide test access to the system/service in advance, which will allow testing the integration's functionality. Access is provided in the chat with the moderator after submitting the solution for moderation or in a special "Test Data" field in the solution description (this data will not be published). It is necessary that within the integration setup, the moderator can configure the connection between the system/service and their test Bitrix24 (or several in turn). In other words, if the integration only works with one specific Bitrix24 prepared by the solution developer, this will not be sufficient grounds for passing moderation.
- Moderation is not a full testing process for the technical and user quality of your solutions, and moderators are not obligated to provide exhaustive information about the technical conditions under which errors occur. All solutions are tested in the standard cloud Bitrix24 on the highest tariff. In cases where additional software installation is required, the moderator has the right to refuse publication of the solution.

## 5. Publication

- After moderation, you will receive a notification of the review result in the moderator's chat;
- After successful moderation of the first version of the new application, the solution will be automatically published in the catalog.
- Upon successful moderation of subsequent versions, you can independently manage the state of the **"Available in the catalog"** checkbox in the solution card.

## 6. Modifying Published Solutions, Updates

- You can independently change the solution description, installation and setup description, technical support description and contact information, logo, and screenshots;
- You can upload updates to the solution by going to the **"Version History"** tab in the solution card and clicking the **"Add New Version"** button.
- Changes in descriptions, as well as new versions, must be submitted for moderation to become available to end users.

## 7. Procedure for Providing Technical Support to Users

**A**. The developer is obliged to make efforts to resolve technical issues related to the interaction of their solutions with Bitrix24, without shifting this task onto end users:

- If, as a result of investigating the end user's problem, you believe it is not caused by an error in your code, but rather that the Bitrix24 REST API is not functioning or is not working as it did before, you must contact [1C-Bitrix support](https://helpdesk.bitrix24.com/ticket.php) and provide logs of specific HTTP requests (with all used parameters) demonstrating the problem.
- Bitrix24 does not consult end users on the scenarios and functionality of your solutions and is not responsible for their functionality.
- Bitrix24 is responsible for the functioning of the REST API within the framework described in the relevant documentation.
- The resolution of technical issues related to the operation of the REST API should always be handled directly between you and 1C-Bitrix technical support without involving end users.

**B**. Bitrix24 reserves the right to remove from publication solutions whose developers do not adhere to the above approach.

## Continue Learning

- [{#T}](./common-requirements.md)