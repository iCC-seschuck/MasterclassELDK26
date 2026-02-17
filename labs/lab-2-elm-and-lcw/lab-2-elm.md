# Lab 2 - Entitlement Management

The activities in this lab will all be executed within the [Entitlement Management](https://entra.microsoft.com/#view/Microsoft_AAD_ERM/DashboardBlade/~/GettingStarted) & [Lifecycle Workflows](https://entra.microsoft.com/#view/Microsoft_AAD_LifecycleManagement/CommonMenuBlade/~/overview) blade in Entra ID. For this you need at a bear minimum 'Identity Governance Administrator' and 'Lifecycle Workflow Administrator'.

In this lab you will create separate privileged accounts via an Access Package request, and that request will upon approval and successful delivery, provision a separate privileged account for the targeted user.

The lab will use the API-driven Provisioning API created in Lab 1, and will consist of:

1. An Access Package for requesting a Privileged Account.
    1. A self-service, direct assignment or manager assigned policy for requesting the Access Package for the target user.
    1. Add Approvals as needed to govern who are approved to get a Privileged Role.
    1. No resource roles are needed for the Access Package, but you can optionally add a Team or similar for maintaining and documenting routines, processes and guidelines for privileged users.
1. A Custom Extension connected to the Access Package for the Stage "Access is Granted".
    1. This Custom Extension Logic App must be pre-created in the Catalog.
    1. The Logic App will retrieve the target user of the request for Privileged Access, get user details from Graph API, and push a SCIM payload to the Provisioning API for creating the Privileged Account.
    1. The Logic App will use a Managed Identity authorized to request the necessary Graph API resources and permissions.

Creating a Logic App requires access to an Azure Subscription for your Test/Demo tenant. If you do not have an Azure Subscription directly connected to your Entra ID tenant, but can access an Azure Subscription in another tenant, please look into the description of [using Azure Lighthouse here](../../resources/resource-1-azure-lighthouse/readme.md).

If you don't have access to any Azure subscription at all, skip to the last part of this lab exercise, and create a privileged user account directly via Graph Explorer to the Provisioning API, similar to what you did in Lab 1 earlier.

## Lab 2.1 - Create an access package which can be requrested for internal users only

As access packages are an easy way to request access to multiple resources in one go, let's create an access package which:

- Is created within the 'ELDK 2026' catalog
- Provides access to two or more resources
- Can be requested by all members in your directory (excluding guests)
- Doesn't have an approval process
- Has a lifecycle configured to either expire after 6 months or runs an Access Review bi-annualy.

**NOTE:** Please make sure that the access package can be requested by end users as we will use this access package within the lifecycle workflows tasks.

Microsoft Learn source: [Entitlement Management - Access Package Create](https://learn.microsoft.com/en-us/entra/id-governance/entitlement-management-access-package-create)

&nbsp;

## Lab 2.2 - Add the assignment of this access package to the pre-hire workflow as a task
BLABLABLA

&nbsp;

## Lab 2.3 - Create an access package with an auto assignment policy

Now you've seen that access packages are an easy way to request access to multiple resources in one go, let's create another access package which will auto-assign users to the access package which:

- Is created within the 'ELDK 2026' catalog
- Provides access to two or more resources (different resources than used in the previous access package)
- Can be requested by no one (administrator direct assignments only)
- Doesn't have an approval process
- Doesn't have a lifecycle configured

Once the access package is created, open the access package and add a new 'auto assignment policy' underneath policies. Scope the auto assignment policy to the department 'ELDK 2026'.

Microsoft Learn source: [Entitlement Management - Access Package Auto Assignment Policy](https://learn.microsoft.com/en-us/entra/id-governance/entitlement-management-access-package-auto-assignment-policy)

&nbsp;

## Lab 2.4 - Create a custom extension for access packages

In some cases you perhaps want to add more advanced scenario's with low code to an access package, for that you can use custom extensions. For the next lab create a custom extension for requesting a privileged account and create a new access package which uses the custom extension on the 'assignment granted' stage. To do this:

- Create a custom extension for 'privileged account requests' in the 'ELDK 2026' catalog.
- Create an accesss package for 'privileged account requests' in the 'ELDK 2026' catalog.
- Provides access to no resources.
- Can be requested by all members in your directory (excluding guests and prefferaby a dynamic group which only contains enabled users with an employeeID).
- Doesn't have an approval process (prefferably it has, but for testing purposes we skip this step).
- Has no lifecycle or access review configured.
- Configure the custom extension created earlier to be triggered after the 'Assignment has been granted' stage.

Microsoft Learn source: [Entitlement Management - Custom Extension](https://learn.microsoft.com/en-us/entra/id-governance/entitlement-management-logic-apps-integration)

&nbsp;

### 3.1.1 - Deploy Logic App and Managed Identity with Bicep

We have prepared a Bicep deployment, [main.bicep](../../resources/resource-3-bicep-custom-extensions/main.bicep), that creates two Logic Apps for Custom Extensions, one for Lifecycle Workflows and one for Access Package usage. Change all TODO references to match your environment, and deploy using guidelines documented in the [readme](../../resources/resource-3-bicep-custom-extensions/readme.md)

This Bicep deployment, in addition to the Logic App Workflows, also create an User Assigned Managed Identity, gives the UAMI Graph Permissions, and connects the UAMI to the Logic Apps.

These Logic Apps can be used as a starting point for the next excercises in this lab.

### 3.1.2 - Add Custom Extension to Entitlement Management Catalog

PS! When creating a Logic App in a multi-tenant scenario, the Authorization Policy of the Logic App will point to the wrong tenant. Please change the Tenant ID to the Entra ID tenant you are setting up the custom extensions for, and not for the Entra ID tenant where the Azure Subscription is.

(This is handled by the Bicep deployment above, but if you create the Logic App and Custom Extension in the Portal ypu have to manually verify this).

### 3.1.3 - Create Access Package for Privileged Access using Custom Extension

More details coming...

### 3.1.4 - Edit Logic App with HTTP Requests for Getting User Details from Target and Build SCIM Payload

More details coming...

### 3.1.5 - Add SCIM Payload and Send to Provisioning API

More details coming..., referring to [privileged-user.json](../../resources/resource-2-scim-sample-payloads/privileged-user.json)

### 3.1.6 - Test an Access Package assignment to Request a Privileged Account

More details coming...

PS! If you don't have access to an Azure Subscription, paste and modify [privileged-user.json](../../resources/resource-2-scim-sample-payloads/privileged-user.json) directly into Graph Explorer for the Provisioning API, similar to Lab 1 above.

## Lab 2.5 - Enable the passkey authentication method

Now the onboarding has finished let's make sure that once the end user is starting to use their TAP they can onboard a passkey to their account. Within Entra ID go to Authentication Methods with an admin account which has the authentication policy administrator role. Here make sure that:

- There is passkey profile configured for all users whereby device-bound passkeys are enabled
- There is passkey profile configured for all users whereby synced passkeys are enabled, this can be targetted against a dynamic group containing all regular user accounts, or a test group.

Microsoft Learn source:  [Entra ID - Passkey Profiles](https://learn.microsoft.com/en-us/entra/identity/authentication/how-to-authentication-passkey-profiles)

&nbsp;
