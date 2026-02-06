# Lab 2 - Entitlement Management

The activities in this lab will all be executed within the Entitlement Management blade in Entra ID. For this you need at a bear minimum 'Identity Governance Administrator'.

## Lab 2.1 - Create an access package which can be requrested for internal users only

As access packages are an easy way to request access to multiple resources in one go, let's create an access pacakge which:
- Is created within the 'ELDK 2026' catalog
- Provides access to two or more resources
- Can be requested by all members in your directory (excluding guests)
- Doesn't have an approval process
- Has a lifecycle configured to either expire after 6 months or runs an Access Review bi-annualy.

**NOTE:** Please make sure that the access package can be requested by end users as we will use this access pacakge within the lifecycle workflows tasks.

## Lab 2.2 - Create an access package with an auto assignement policy

Now you've seen that access packages are an easy way to request access to multiple resources in one go, let's create another access package which will auto-assign users to the access package which:
- Is created within the 'ELDK 2026' catalog
- Provides access to two or more resources (different resources than used in the previous access package)
- Can be requested by no one (administrator direct assignments only)
- Doesn't have an approval process
- Doesn't have a lifecycle configured

Once the access package is created, open the access package and add a new 'auto assignmnet policy' underneath policies. Scope the auto assignment policy to the department 'ELDK 2026'.

## Lab 2.3 - Advanced: Create a custom extension for access pacakges

In some cases you perhaps want to add more advanced scenario's with low code to an access package, for that you can use custom extensions. If you've got time left during the lab you can create a custom extension which can be configured within one of the access packages created earlier. Do do this:
- Create a custom extension in the 'ELDK 2026' catalog.
- Configure the custom extension in one of the access packages created earlier to be triggered after the 'Assignment has been granted'
