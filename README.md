# Overview
This Contact Synchronisation application performs uni-directional contact synchronisation from Salesforce to Workday. It's a process API that depends on 2 system APIs;

 - workday-erp-app   
 - salesforce-erp-app

It runs periodically and;

 1. Checks to see if any contacts have been created or modified since
    the last check 
 2. If they have then it looks for corresponding accounts
    in Workday 
 3. If a corresponding account is matched/found, using the
    account name, the contact is updated or added

The `salesforce-erp-app` returns contacts sorted in ascending order (oldest modified contacts first) To ensure that no contact changes are missed,  as each is processed their timestamp is saved. That way if processing fails for any reason, it will pick up from the last successful synchronised contact, as the last processed contact timestamp is used to filter modified/new contacts

The very first time this process is run it will synchronise all contacts

# Configuration
Before deploying the application it needs to be configured with the correct ERP and CRM service connection parameters. These are set in the 3 configuration files in the dire src/main/resources These  have the name `<env>-config.properties`, where `<env>` is the environment the application is deployed to (`dev`, `test` or `prod`).

The environment parameter that determines which configuration file is used is set using a system property, `mule.env`, that can be set at runtime. The default environment is `test`

# Additional Notes
In the current version of this integration process contact deletions are not synchronised, because its proven difficult to understand the mechanism for this in the Workday WS API