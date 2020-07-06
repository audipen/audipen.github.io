---
title: "Implementing CIS Azure Foundations Benchmark"
date:   2020-03-22 
categories: governance
tags: 
  - security
  - Azure Automation
  - Azure Blueprints
  - Azure Policy
cis-create:
  - url: /assets/images/create-cis.png
    image_path: /assets/images/create-cis.png
    alt: "Create CIS blueprint"
    title: "CIS sample blueprint."
cis-initiative:
  - url: /assets/images/cis-initiative.png
    image_path: /assets/images/cis-initiative.png
    alt: "CIS policy initiative"
    title: "CIS policy initiative."
---

The 'Center of Internet Security' (CIS) is a non-profit entity that aim to protect private and public organizations against cyber threats. CIS has come up with benchmarks for various technology groups to safeguard them against evolving cyber threats. Of the various benchmarks there is one also for 'Microsoft Azure' which provides a set of guidelines for establishing a secure baseline configuration. 

The 'CIS Microsoft Azure Foundations Security Benchmark' is basically a collection of recommendations grouped into various categories. For example, there are 23 recommendations to follow when setting 'Identity and Access management' policies on an Azure Subscription. The ['CIS Benchmark (v1.1.0)'](https://learn.cisecurity.org/benchmarks) can be downloaded for free by filling out a form.

How can these recommendations be actually implemented and enforced by an organization on it's Azure subscription? There are a couple of ways to do this.
 
* **Azure Blueprint** : Microsoft provides a production ready sample Blueprint containing policies that implement various controls specfied in the benchmark.  
* **Azure Automation Runbooks** : If custom logic with more control is required the benchmark controls can be implemented using Automation Runbooks which can then be scheduled to run periodically.

 In this post I will describe the Blueprint approach in detail.

 An [Azure Blueprint](https://docs.microsoft.com/en-us/azure/governance/blueprints/overview) is a container that allows you to bring together various artifacts like Resource Groups, Policy Assignments, Role Assignments, ARM templates in order to define a set of repeatable resources that adhere to an organizations standards and rules. [Azure Policies](https://docs.microsoft.com/en-us/azure/governance/policy/overview) allow you to specify rules that resources must adhere to and once assigned to the respective resources, the service periodically checks whether the resources comply with the rules. 
	
 Microsoft provides several Blueprint samples and one of them is the ['CIS Microsoft Azure Foundations Benchmark' sample](https://docs.microsoft.com/en-us/azure/governance/blueprints/samples/cis-azure-1.1.0/) that contains several policies that implement various recommendations within the CIS benchmark.
  
 {% include gallery id="cis-create" caption="CIS sample blueprint." %}
 
 This sample can be easily deployed to an Azure subscription. [How the policies map to the recommendations](https://docs.microsoft.com/en-us/azure/governance/blueprints/samples/cis-azure-1.1.0/control-mapping) in the benchmark is detailed in the documentation. An important point worth noting is that not all recommendations have an equivalent policy in the blueprint i.e. if the result of a Azure Policy run shows '*Compliant*' it does not mean that the target subscription(s) is fully CIS compliant.

## Deploying the Blueprint	
 Let's see how to deploy the CIS sample blueprint. The first step is to create a new Blueprint based on the sample. Usually you would need to add artifacts to the Blueprint but the sample already contains a '*Policy Assignment*' artifact. This Policy Initiative contains all the policies that implement the CIS checks. 
 
 {% include gallery id="cis-initiative" caption="CIS policy initiative." %}

 Once created, the Blueprint is saved in 'Draft' mode and needs to be pulished before it can be assigned to a target resource. Detailed instructions on all these steps are provided in the [documentation](https://docs.microsoft.com/en-us/azure/governance/blueprints/samples/cis-azure-1.1.0/deploy).
  
  Once assigned the contained artifacts are deployed in the target resource. In this case, the policies are applied to all the subscriptions present in the Management Group,if you have selected a Management Group as the target, or the selected Subscription. The policies are evaluated for compliance every 24 hours and are automatically applied to all existing resources in the target and also to new ones that are added later on.
 
 After the first evaluation you should see the results as Activity logs under Azure Monitor with Operation name as '*'audit' Policy action*'. You might need to adjust the '*Timespan*' filter to '*Last 24 hours*' to see them all. Logs are grouped by the resource i.e. you should see all the non-compliant entries for a particular resource grouped together. There are several checks that are executed as a part of the policy initiative and you might want to get notified explicitly when certain checks fail rather than having to manually check the logs everytime. Let's look at how we can setup an email notification that is sent on non-compliance.
	
## Setting up Alerts
 Click on the Activity log entry for which you want to set an alert and subsequently on *'New alert rule'*. This opens the *'Create Rule'* form.
 An Alert rule is made up of 3 parts 
 - the target resource
 - an alert condition
 - the action to be performed when the condition is met.
    
  The target resource and condition are automatically populated based on the activity log. The condition would look something like '*Whenever the Activity Log has an event with Category='Policy', Signal name='All Policy operations', Level='warning', Status='succeeded', Initiated by='Microsoft Azure Policy Insights'*'. Instead of creating alert rules for each of the policy voilations it might make sense to create a single or few rules with a broader target.
 The action to be performed when the condition is met can be configured via Action Groups. 
 
 Action Groups let you group multiple actions together so that all of them can be executed when the alert is triggered. If you have an existing Action Group, click the '*Add*' button to select it and associate it with the Alert rule. Click '*Create*' to add a new Action Group. Specify a name and then select one or multiple actions that you want to perform as a part of this group. In addition to sending an Email or SMS, you can also trigger an Azure Function or Logic App and kick start a detailed workflow.
 Lastly, specify some Alert details such as the rule name, a description and a resource group where this alert will be saved. Click on '*Create alert rule*' to save the configuration and turn on the rule. 
 Next time the policies are evaluated and resources for which you have an alert rule configured don't comply, you should receive an email with details of non-compliance. 
 
 As previously mentioned, not all CIS recommendations are covered by the blueprint. Missing ones can be further implemented by adding appropriate Policy definitions to the initiative. 
	