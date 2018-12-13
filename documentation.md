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

2.2.1 Request Details

a.	DXC Offering Family: Lookup Select Box – Mandatory
-	Select Major Offering: Lookup Select Box – Mandatory 
Depending on the value selected in DXC Offering Family, the Select Major Offering 	shows up with help of a UI policy. Example, if Cloud Platform and Services is selected 	under DXC Offering Family then the options for major offerings – O365, Azure or AWS 	are visible.

b.	Select Business Service and Catalog Entitlement: List Collector – Mandatory 
- Selection of either of the major offering results in another field being visible where in 	the offering specific business services and catalog entitlement can be chosen.
