# PDXC Client Offering Onboarding Master Workflow

The following documentation describes the Client Offering Onboarding Master WF process and the associated technical specifications as designed for DXC’s ServiceNow environment. The Master workflow was created in order to automate DXC’s manual process for different offerings.

**Catalog Item**

The workflow can be accessed using the Offering Onboarding V2 Catalog Item under the GBP Taxonomy Catalog in DXC’s environment. As the Offering Onboarding fulfillment process matures, the content of the catalog item will also continue to evolve, as is required by business needs. The Service Catalog Manager will have a sound understanding of the status of the offerings offered by DXC organization.

Catalog items in ServiceNow are defined using three interrelated components: Core elements, user inputs, and workflow stage definition. 

**1. Core Elements** include all of the information needed to describe the catalog item for display within the service catalog along with the group responsible for validating the item, if required. The elements are as follows: name, application scope, catalogs, description, availability, area, etc.

**2. User Inputs** include all of the fields published on the catalog item form. User inputs should be defined such that all required information is captured upon item submission so as to enable the fulfillment group(s) to provision the request without having to contact the requestor for additional information. For the purpose of this Catalog Item for Offering Onboarding, the following parameters are required for intake: 

The Offering Onboarding V2 form is divided into three sections - 
 - 2.1 Company Information 
 --------------------------
 ![Company SS](https://github.com/anudave/master-wf/blob/master/images/company.png)
 
 The values are being captured for Company Information are: (Single Line Text)
  a. Company: Name of Company (AEGON)
  b. Client ID: Salesforce ID (5172)
  c. Stock Ticker: Company CAP (AEG)
  d. Contract ID: If associated with any external contract
  e. Company Address: Street, City, State, Zip and Country

 - 2.2 PDXC Offering Intake Questions
 -------------------------------------
 ![Offering Ques](https://github.com/anudave/master-wf/blob/master/images/offering_ques.png)
 
a.	Account Type: Multiple Choice – Mandatory
-	Identify if its Commercial (Client-facing) or Internal (DXC)
	
b.	DXC Region: Lookup Select Box – Mandatory 
-	Select the region the request is associated with
	
c.	Offering Type: Multiple Choice – Mandatory
-	Select between Commercial Silver/Gold or Internal Silver/Gold or ANZ
	
d.	Chargeback Code/IWO: Single Line Text
	
e.	SDFC ID: Single Line Text
-	Required for Service Now Onboarding

f.	MSP Uplift Percentage: Single Line Text

**2.2.1 Request Details**

a.	DXC Offering Family: Lookup Select Box – Mandatory
-	Select Major Offering: Lookup Select Box – Mandatory 
Depending on the value selected in DXC Offering Family, the Select Major Offering 	shows up with help of a UI policy. Example, if Cloud Platform and Services is selected 	under DXC Offering Family then the options for major offerings – O365, Azure or AWS 	are visible.

b.	Select Business Service and Catalog Entitlement: List Collector – Mandatory 
- Selection of either of the major offering results in another field being visible where in 	the offering specific business services and catalog entitlement can be chosen.

![Bus Serv Cat Ent] (https://github.com/anudave/master-wf/blob/master/images/bs_cat_item.png)

 - 2.3 User Information
 --------------------------
 ![User] (https://github.com/anudave/master-wf/blob/master/images/user.png)
 
User information up to three users is captured to identify and provide access to concerned people from the client company and up to two admins can also be added on the request form for the offering that was selected


**3. Workflow** stages define the activities and corresponding order of precedence (or concurrency) that are executed for a particular catalog item as it iterates through its lifecycle. This includes all necessary approvals, catalog (fulfillment) tasks, purchasing activities, change requests, and specialized notifications.

**Sub-Flows used** 
1. PDXC Onboarding DMAR Sub-flow
2. PDXC Onboarding Domain Creation/ConnectNow Sub-flow

![WF] (https://github.com/anudave/master-wf/blob/master/images/wf.png)

1.	Initialize Core Data 
-	This activity allows the core inputs from the form to be added into an input array that will be utilized in all the following workflow activities.

2.	PDXC Onboarding DMAR Sub-flow
-	This sub-flow makes a ConnectNow call to DMAR to check whether the Company is already listed in PDXC’s Dev environment as a Manufacturer or Vendor. Failure at any of the checkpoints within the sub-flow are handled by Catalog Tasks where responsible groups can make changes accordingly to move on to the next steps in the workflow. 

3.	Set Company Name to Scratchpad
-	From the data output of previous sub-flow, the value for Company Name is then set to a scratchpad. 

4.	PDXC Onboarding Domain Creation/ConnectNow Sub-flow
-	Moving on in the workflow after validating the Client and their Account and creating the scratchpad, a separate domain for each of the clients gets created in this activity.

5.	Setting Values to Scratchpad from Domain Creation Sub-flow 
-	This activity ensures and sets the Domain return information to Company and Domain scratchpads. It sets the sys_id(s) and name of Company as well as Domain respectively. 


For the following activities in the workflow, Script includes are leveraged for each in order to avoid hard-coding in DXC environment. OOSS compliances are met with in each of the functions within the script includes for error-handling purposes. The return values are associated with a scratchpad to utilize them throughout the workflow as needed. Success at every Run Script moves the workflow further to completion while Catalog Tasks for Failure have been added to ensure intervention and investigation from associated assignment groups

6.	Company Department and Location Records
-	Inserts the records for company specific department and location specific to the offering selected on the form. 
-	Catalog Task: Data Load Errors
         
7.	Group Visibility and Users Creation 
-	Users and Domain Visibility for Admins are created in this activity. 
-	Catalog Task: Data Load Errors
        
8.	Load Business Services 
-	Business Service selected on the form are populated under customer domain. Category and Sub-category for offering’s Type 1 and Type 2 relationship in customer domain is also created under this step in the workflow.
-	Catalog Task: Business Services Errors

9.	Catalog Entitlement and Portal 
-	Company Styling for Portal in Customer company and Company Name is added to applicable catalogue entitlement respective to the offering selected on the form.
Catalog Task: Catalog Configuration Errors 
