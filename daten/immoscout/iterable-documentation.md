---
type: doc
tags:
  - archived
  - immoscout
status: archived
source: notion-import
---
- 1 campaign (maybe multiple messages) → 1 placement (message priority/ multiple messages display)

### What we need from Marketing

- API key (generated in Iterable UI) → AWS Parameter Store
    - Client side key

- **placements**: Where the embedded messages will appear
- **type**: how many messages to display at a time
- **campaign limit** (carousel or feed): max messages in a placement


### **data requirements**: What data the embedded messages will display

- **Title** – A title, usually for prominent display in your message.
- **Body** – Message content.
- **Media URL** – A URL to an image to display in your message.
- **Data Feeds** – External URLs Iterable should query before delivering an embedded message to a device. The data returned by these URLs can be used to personalize the message.
- **Text Fields** – Extra data to include with your message.
Text fields are for data, not content. Similar to **Raw data (JSON)**, your app can use the values of these fields to style your embedded message, or to trigger custom functionality in your app.
Click **Add Field** to add custom field text fields. In the text input, enter the label that should appear in Iterable as someone creates a template or campaign. The person creating the template or campaign can then enter a value for the field.
- **Open Action** – An action (link or custom action) to invoke when an embedded message is tapped.
- **Action Buttons** – Buttons to include in your message (and associated URLs or custom actions to invoke when they're clicked). Click the pencil button next to each button to change its name.
- **Raw Data (JSON)** – The value entered here is used as the default value for the corresponding **Raw Data (JSON)** input on new templates and campaigns associated with this placement. You can edit the value before activating the campaign.
- **message design**: How the messages will display that data
    - Out-of-the-box-views
    - <u>custom message displays</u>

### Web API Key

