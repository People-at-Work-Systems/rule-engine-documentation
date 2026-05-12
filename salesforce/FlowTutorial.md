# Flow API Tutorial

This document presents a small Tutorial on how to create a simple Rule in the Rule Engine and call it from a Flow.

## Setting up the Rule

Let's create a Rule that assigns Countries to Regions.

First, we create a template in the Rule Engine. Go to `Rule Engine > Templates > New Template` and name it `CountryRegion`.

> Hint: Everything we do inside the Rule Engine is case-sensitive.

![01.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\01.png)

Now, we add an input parameter `ShippingCountryCode` and an output parameter `Region`. The input parameter equals the API Name of the Account Field, so that we can directly send an Account SObject to the Rule Engine without constructing a custom JSON.

![02.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\02.png)

Afterwards, go to `Rule Engine > Rules` and Create a new Folder under Root with the Name `Account`. Then select the Account Folder and click on `New Rule`. As the Rule template choose `CountryRegion` and keep the Rule Name also `CountryRegion`.

![03.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\03.png)

We can start with filling our Rule Definition with some country codes and regions.

![04.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\04.png)

We added some Country Codes and their regions. With the last line, we also added the logic that any other Country Code gets the `World` region assigned. Now, save the Rule.

## Create a Flow

Let's go to Salesforce and create a Flow. We want to set the Region (a custom Field on Account called Region__c) whenever an Account is updated and the Shipping Country is set or changed.

Create a new Record-Triggered Flow:

![05.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\05.png)

For the Start Element, define the following:

- Account Object

- Trigger when a record is updated

- Entry Condition: Field Shipping Country Code IS Changed true

- Optimize Flow for Actions and Related Records

- Add asynchronous Path

Then add a Get Records Element in the asynchronous Path, where we will query the Account and in the section **How to Store Record Data**, select `Choose fields and let Salesforce do the rest` and add the Field `ShippingCountryCode`.

![06.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\06.png)

As the next step, add an Action Element below and search for **Execute Rule (Single SObject)**. Set the Inputs as depicted below:

![07.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\07.png)

We use the Rule Name `CountryRegion` and the Account we queried in the Get Records Element before and also specify our previously created Account Folder in the Path input.

Now, we have to parse the Result from the Rule Engine. Add another Action Element and search for **Extract String**. As the input, provide the following:

- JSON: Execute Country Region Rule > Evaluation Output > singleLineResult

- Path: Region

The Path equals our output parameter defined inside the Rule Engine. If you have a more complex JSON output structure, you can also access a value for example by: `object1.object2.myValue`. In case you have multiple output parameters, add a new **Extract** Element for each parameter.

![08.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\08.png)

After extracting the Region, we can finally update the Account with the automatic output of the Extract String element and save/activate the Flow.

![09.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\09.png)

## Testing the Flow

Let's update an Account, that has its current Shipping Country set to Japan:

![12.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\12.png)

And after the async Flow runs, the Region has changed to **DACH**:

![13.png](C:\Users\Clifford%20Beul\Documents\rule-engine-documentation\salesforce\images\13.png)
