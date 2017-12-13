# MFA-Directory-Lookup-User-Token
Sample to create a multifactor authentication policy that queries user attributes and returns them to the requestor.

## Description

This policy is meant to serve as a template for customers looking to enforce multifactor authentication (MFA) with HTTP Basic and PKI authentication. The authorization policy is minial here, but can be fortified via our PKI and Fine Grain Auth policy, also on the Axway Marketplace: https://marketplace.axway.com/apps/187190#!overview

This policy enforces MFA, queries the directory server for some basic user attributes, and returns them as a JSON formatted token.

This policy will only pass if mutual authentication is configured in the API Gateway, allowing a client certificate can be presented. This requires the API Gateway HTTPS Listener to Allow or Require Client Certificates. More information on this configuration can be found under the "Configure Mutual Authentication Settings" section of the "Configure HTTP Services" topic on the Axway public documentation site (https://docs.axway.com/bundle/APIGateway_753_PolicyDevGuide_allOS_en_HTML5/page/Content/PolicyDevTopics/general_services.htm). A step by step configuration of mutual authentication can be found in the PKI policy mentioned above.

The basic policy flow is as follows:

- **SSL Filter**: This filter, renamed "Require SSL/TLS with Client Certificate" to more accurately represent the functionality it provides, checks to ensure that a client certificate was presented as part of the SSL handshake when connecting to the HTTPS listener port, and extracts it for further processing. The certificate is stored as an attribute, which by default will be "${certificate}". 
- **Extract Certificate Attributes Filter**: This filter, renamed "Extract Attributes From Client Certificate", parses the client certificate to make a large number of certificate attributes available for processing. The created attributes can be easily viewed by right clicking this filter and selecting the "Show All Attributes" option in the policy configuration window.
- **LDAP RBAC Filter**: This filter, renamed "Perform RBAC Check Against LDAP", does a repository lookup to ensure the client certificate maps to a role or group as defined in the search base. Additional attribute based authentication can be configured as well to add further fine grain authorization. To use this filter, you will need to set up access to a directory in Policy Studio under 'External Connections --> LDAP Connections' and an associated repository under 'External Connections --> LDAP Repositories'. Once your repository is configured, you will need to modify the filter to update the Connection, Search Base, and Filter, as well as update or remove the attribute checks.
- **HTTP Basic Filter**: This filter, renamed "Ensure user presents and authenticates with username and password", authenticates a user via username and password, or HTTP Basic, with configuration to lock out user authentication attempts based on configurable lockout parameters.
- **Retrieve from Directory Server**: This filter, renamed "Retrieve attributes from Directory Server", is configured to query a directory server post-user authentication to collect additional attributes about the associated user. These attributes can be used for a basic response, as shown in this sample policy, or for populating security tokens as part of a credential mediation service, such as SAML or JWT security tokens.
![alt text](https://github.com/Axway-API-Management-Plus/MFA_Auth_Directory_Lookup/blob/master/example/src/directoryScreenshot.png "Directory Attributes")
- **Set Message**: This filter, renamed "Create JSON user attribute token", takes an application/json content type message or token schema, and dynamically populates it with values retrived from the user attribute directory search.
![alt text](https://github.com/Axway-API-Management-Plus/MFA_Auth_Directory_Lookup/blob/master/example/src/JSONMessage.png "Set JSON Token")
- **Reflect Message**: This filter reflects the configured message back to the requestor.

These last two filters are mostly for research or debug purposes. Generally user information would be passed to an authorization server, used to create a security token, or to populate application requests as part of API Gateway integration or orchestration initatives.

Once updated to fit your environment and by removing the set message and response filters, this policy can be used in several ways:
- As a standalone authentication policy by assigning it a relative path in Policy Studio where it can be reached.
- It can be called as a policy shortcut as part of other policies created in Policy Studio.
- It can be registered as a REST API via either the API Gateway Rest API Repository  configuration in Policy Studio or by creating a new API in API Manager.
- It can be registered as an Inbound Security Policy for use within API Manager via 'Server Settings --> API Manager --> Inbound Security Policies' in Policy Studio.

## API Management Version ## API Management Version Compatibility
This artefact was successfully tested for the following versions:
- V7.5.3


## Install

```
• Download the MultifactorAuth-JSONUserToken*.xml policy file.
• There are several options to import this file, including Team Development, REST API, and sample scripted options included with the Axway solution. The simplest way however is to open Policy Studio and select 'File --> Import --> Import Configuration Fragment'.
```

## Usage

```
• Create or configure an HTTPS Listener with Mutual Authentication configured to accept client certificates.
• Update policy relative to your environment.
• Configure policy for your environment.
• Call resource and authenticate with your certificate and basic credentials.
```

## Bug and Caveats

```
N/A
```

## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.


## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"

Contact - Daniel Wille: dwille@axway.com

## License
[Apache License 2.0](/LICENSE)

