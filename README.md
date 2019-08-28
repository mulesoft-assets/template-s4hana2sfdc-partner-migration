
# Anypoint Template: SAP S4HANA to Salesforce Partners Migration	

<!-- Header (start) -->

<!-- Header (end) -->

# License Agreement
This template is subject to the conditions of the <a href="https://s3.amazonaws.com/templates-examples/AnypointTemplateLicense.pdf">MuleSoft License Agreement</a>. Review the terms of the license before downloading and using this template. You can use this template for free with the Mule Enterprise Edition, CloudHub, or as a trial in Anypoint Studio. 
# Use Case
<!-- Use Case (start) -->
This template helps you migrate partners from a SAP S4HANA to a Salesforce instance, specify a filtering criteria, and specify a behaviour when an account already exists in the destination instance.

As implemented, this template leverages the Mule batch module.
The batch job is divided into *Process* and *On Complete* stages.

The migration process starts by fetching all the existing partners that match the filter criteria from SAP S4HANA.
On *Process* stage matches accounts that are grouped and inserted or updated into Salesforce.

Finally during the On Complete stage, the template outputs statistics data into the console and sends a notification email with the results of the batch execution.
<!-- Use Case (end) -->

# Considerations
<!-- Default Considerations (start) -->

<!-- Default Considerations (end) -->

<!-- Considerations (start) -->
To make this template run, there are certain preconditions that must be considered. All of them deal with the preparations in both, that must be made for all to run smoothly. 
Failing to do so can lead to unexpected behavior of the template.
<!-- Considerations (end) -->

## SAP S/4HANA Considerations

Here's what you need to know to get this template to work with SAP S/4HANA.

### As a Data Source

There are no considerations with using SAP S/4HANA as a data destination.

## Salesforce Considerations

Here's what you need to know about Salesforce to get this template to work:

- Where can I check that the field configuration for my Salesforce instance is the right one? See: <a href="https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US">Salesforce: Checking Field Accessibility for a Particular Field</a>.
- Can I modify the Field Access Settings? How? See: <a href="https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US">Salesforce: Modifying Field Access Settings</a>.


### As a Data Destination

There are no considerations with using Salesforce as a data destination.
# Run it!
Simple steps to get this template running.
<!-- Run it (start) -->

<!-- Run it (end) -->

## Running On Premises
In this section we help you run this template on your computer.
<!-- Running on premise (start) -->

<!-- Running on premise (end) -->

### Where to Download Anypoint Studio and the Mule Runtime
If you are new to Mule, download this software:

+ [Download Anypoint Studio](https://www.mulesoft.com/platform/studio)
+ [Download Mule runtime](https://www.mulesoft.com/lp/dl/mule-esb-enterprise)

**Note:** Anypoint Studio requires JDK 8.
<!-- Where to download (start) -->

<!-- Where to download (end) -->

### Importing a Template into Studio
In Studio, click the Exchange X icon in the upper left of the taskbar, log in with your Anypoint Platform credentials, search for the template, and click Open.
<!-- Importing into Studio (start) -->

<!-- Importing into Studio (end) -->

### Running on Studio
After you import your template into Anypoint Studio, follow these steps to run it:

+ Locate the properties file `mule.dev.properties`, in src/main/resources.
+ Complete all the properties required as per the examples in the "Properties to Configure" section.
+ Right click the template project folder.
+ Hover your mouse over `Run as`.
+ Click `Mule Application (configure)`.
+ Inside the dialog, select Environment and set the variable `mule.env` to the value `dev`.
+ Click `Run`.
<!-- Running on Studio (start) -->

<!-- Running on Studio (end) -->

### Running on Mule Standalone
Update the properties in one of the property files, for example in mule.prod.properties, and run your app with a corresponding environment variable. In this example, use `mule.env=prod`. 


## Running on CloudHub
When creating your application in CloudHub, go to Runtime Manager > Manage Application > Properties to set the environment variables listed in "Properties to Configure" as well as the mule.env value.
<!-- Running on Cloudhub (start) -->
After your app is running, if you chose `s4hanapartnresmigration` as the domain name to trigger the use case, you can browse to `http://s4hanapartnresmigration.cloudhub.io/migratepartners` to run the app, and the report gets sent to the emails you configured.
<!-- Running on Cloudhub (end) -->

### Deploying a Template in CloudHub
In Studio, right click your project name in Package Explorer and select Anypoint Platform > Deploy on CloudHub.
<!-- Deploying on Cloudhub (start) -->

<!-- Deploying on Cloudhub (end) -->

## Properties to Configure
To use this template, configure properties such as credentials, configurations, etc.) in the properties file or in CloudHub from Runtime Manager > Manage Application > Properties. The sections that follow list example values.
### Application Configuration
<!-- Application Configuration (start) -->
**HTTP Connector Configuration**
+ http.port `9090` 

**Batch Aggregator Configuration**
+ page.size `1000`

**SAP S4Hana Connector configuration**
+ s4hana.baseUrl `your.sap4hana.address.com`
+ s4hana.username `your.sap4hana.username`
+ s4hana.password `your.sap4hana.password`
+ s4hana.partners.limit `100`

**Salesforce Connector Configuration**
+ sfdc.username `bob.dylan@orga`
+ sfdc.password `DylanPassword123`
+ sfdc.securityToken `avsfwCUl7apQs56Xq2AKi3X`

**SMTP Services Configuration**
+ smtp.host `smtp.example.com`
+ smtp.port `587`
+ smtp.user `exampleuser`
+ smtp.password `examplepassword`

**Email Details**
+ mail.from `your.email@example.com`
+ mail.to `your.email@example.com`
+ mail.subject `Mail subject`
<!-- Application Configuration (end) -->

# API Calls
<!-- API Calls (start) -->
Salesforce imposes limits on the number of API Calls that can be made. Therefore calculating this amount is important. This template calls the API using this formula:

***2X / ${page.size}***

***X*** is the number of accounts to be synchronized on each run. 

Divide by ***${page.size}*** because by default, accounts are gathered in groups of ${page.size} for each Upsert API Call in the aggregation step.	

For instance if 10 records are fetched from origin instance, then 2 API calls are made (1 + 1).
<!-- API Calls (end) -->

# Customize It!
This brief guide provides a high level understanding of how this template is built and how you can change it according to your needs. As Mule applications are based on XML files, this page describes the XML files used with this template. More files are available such as test classes and Mule application files, but to keep it simple, we focus on these XML files:

* config.xml
* businessLogic.xml
* endpoints.xml
* errorHandling.xml<!-- Customize it (start) -->

<!-- Customize it (end) -->

## config.xml
<!-- Default Config XML (start) -->
This file provides the configuration for connectors and configuration properties. Only change this file to make core changes to the connector processing logic. Otherwise, all parameters that can be modified should instead be in a properties file, which is the recommended place to make changes.<!-- Default Config XML (end) -->

<!-- Config XML (start) -->

<!-- Config XML (end) -->

## businessLogic.xml
<!-- Default Business Logic XML (start) -->
The functional aspect of the template is implemented in this XML file, directed by one flow responsible of excecuting the logic.
For the purpose of this particular template the *mainFlow* uses a Mule batch job, which handles all the logic.<!-- Default Business Logic XML (end) -->

<!-- Business Logic XML (start) -->

<!-- Business Logic XML (end) -->

## endpoints.xml
<!-- Default Endpoints XML (start) -->
This is the file where you specify the inbound and outbound sides of your integration app.
This template only uses an HTTP Listener to trigger the use case.

**HTTP Listener** - Start Report Generation
+ `${http.port}` is set as a property to be defined either in a property file or in the CloudHub environment variables.
+ The path configured by default is `migratepartners` and you are free to change it to one you prefer.
+ The host name for all endpoints in your CloudHub configuration should be defined as `localhost`. CloudHub routes requests from your application domain URL to the endpoint.
+ The endpoint is configured as a *request-response* since as a result of calling it the response is the total of accounts synced and filtered by the criteria specified.<!-- Default Endpoints XML (end) -->

<!-- Endpoints XML (start) -->

<!-- Endpoints XML (end) -->

## errorHandling.xml
<!-- Default Error Handling XML (start) -->
This file handles how your integration reacts depending on the different exceptions. This file provides error handling that is referenced by the main flow in the business logic.<!-- Default Error Handling XML (end) -->

<!-- Error Handling XML (start) -->

<!-- Error Handling XML (end) -->

<!-- Extras (start) -->

<!-- Extras (end) -->
