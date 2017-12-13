# MFA-Directory-Lookup-User-Token
Sample to create a multifactor authentication policy that queries user attributes and returns them to the requestor.

## Prerequisites

To follow this example you will need the following:

- A client public/private key pair with a single depth level.
- The issuer public certificate configured for mututal authentication.
- A directory to lookup customer attributes with user password and certificate defined.
- The policy export file that is part of this repository.
- Basic working knowledge of the Axway API Gateway software and Policy Studio.

Step-by-Step mutual auth configuration can be found here: https://marketplace.axway.com/apps/187190#!overview

## Step By Step

1. In Policy Studio, navigate to 'File --> Import --> Import Customization Fragment'. Select the policy fragment supplied in this repository. Resolve any dependency issues and complete import.
![alt text](https://github.com/Axway-API-Management-Plus/MFA_Auth_Directory_Lookup/tree/master/example/src/importFrag.png "Import Policy Fragment")
2. Review the policy config and modify the policy to meet your environmental configuration as defined in this repo's readme.
3. Deploy.
4. Ensure your client certificate is available to your test client and execute a request against your new service. Select the appropriate client key and submit your username and password. Your service should echo back attributes from the directory as shown below.
![alt text](https://github.com/Axway-API-Management-Plus/MFA_Auth_Directory_Lookup/tree/master/example/src/MFAResponse.png "Sample Response")
5. Log into the API Gateway Manager (https://HOST:8090), navigate to the Traffic Monitor, and review the policy execution and MFA steps.

## API Management Version Compatibility
This artefact was successfully tested for the following versions:
- V7.5.3

## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.


## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"

Contact - Daniel Wille: dwille@axway.com

## License
[Apache License 2.0](/LICENSE)
