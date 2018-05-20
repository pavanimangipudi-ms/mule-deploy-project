# Intoruction

This Maven project can be used as a standalone independent deployment project to deploy any Mule Deplayble Archives (MDA) for different deployment targets, which includes

    1. Anypoint Runtime Manager (Mule Runtime related, cloud and on-premesies)
    2. Cloudhub
    3. MMC
    4. API Manager (post Anypoint Platform Crowd release)
  
1 - 3 are Mule Runtime Related deployment targets. During the deployment phase, target Mule runtime will be customzied (mostly related to Cloud and ARM deployment, on-premises deployment has limited customization during deployment, i.e only application scope variables and it requires Mule 3.9.x or after).
  
4 are API Manager related deployment target. It focuses on the provision and configure the API instance in API manager,  for various aspects of the API, such as
    1. Client Application
    2. Policies
    3. SLA
    4. Alert

Different Maven profile can be use for the respective deployment target. For instance, to deploy a MDA to ARM, the maven command will
be  "mvn deploy -P arm -Dargs=...". 

The following section will detail the arguments which can be used in each profile.

## Runtime Manager related deployment

(this section of documentation will be coming soon...)

## API Manager related deployment

The maven profile "crowd" is used to create or look up API Instance in Anypoint Platform API Manager. 

The implementation is using Groovy script and it is wrapped as a Maven task. So it can be used by any CICD products with Maven support, in different platforms (Windows, Linux)

The curent release is only scoping for the creat or lookup of an API instance in the API manager. The Groovy script will be extended in the future release to include the policies or client management functionalities.
 
### Exchange.json file in MDA

As prerequisite and assumption for this solution, the Mule Deployable Archive will need to contain a "exchange.json" file.  "exhange.json" is meta info file created by Anypoint Exchange. It is in JSON format and it contains the API spec's asset detail as stored in Exchange. 

This file is essential in binding the API Spec (RAML Files created by API Designers as maintained in Exchange) with the API implementation (Mule application implmentation as maintained by developers in SVC). 

### Command usage

The maven command will be used to create or lookup an API instance in API Manager's dedicated environment.

```
  mvn package -P crowd  \
  -Danypoint.user=derek.lin \
  -Danypoint.password=(ANYPOINT PASSWORD) \ 
  -DorgId=(ORG ID) \
  -DenvId=(ENV ID)  \
  -DexchangeFileName=(LOCATION OF THE exchange.json file) \
  -DtargetPropFile=(OUTPUT FILE for holding the api discovery detail)
```

Arguments:
  
  anypoint.user: Anypoint platform user name
  
  anypoint.password:  Anypoint platform password
   
   -- the above two arguments will be used to obtain Anypoint platform login tokens and all the platform API calls
  
  orgId:  target Anypoint platform organzaition id
  
  envId:  target Anypoint platform environment
  
  exchangeFileName: file path to the exchange.json file, this will take precedence over the exhchange.json file embedded in the Mule Deployable Archive
  
  targetPropFile: output file location as generated by the Groovy script for the API Manager's dicvoery task. This includes API name and version for Mule 3 and API id for Mule 4   

