# Landing zone - launchpad opensource

The open source launchpad provides the base gitops capabilities required to enable your infrastructure as code environment on Azure to deploy the Cloud Adoption Framework Terraform Landing Zones. It is a foundation set of service from a security, governance and provisioning perspective that will take ownership of the lifecycle management of your landing zones. Apart from the Azure running costs the open source launchpad does not require a specific license to run. The project is supported by the open source community and you can fill issues to request new features or repport a defect.


The main objectives of the launchpad are:
* Remote state management
* GitOps services to control everything through git


## Remote state management
The open source launchpad stores all landing zones states into a geo-replicated Azure storage account. You can initialize the launchpad using the CAF rover dev container toolbox. The CAF rover is a versioned container that contains the launchpad and rover command.
When you initialize the launchpad you are asked to define an Azure region. This Azure region will be the primary region used to store the remote state and any related secrets and keys. 

The following services are deployed:
- <b>Storage account </b> - stores all landing zones Terraform states
- <b>Virtual network </b> - isolates the traffic between the different services like storage account, keyvault, devops agents. 
- <b>Keyvault</b> - Azure HSM to store the secrets generated by the launchpad. Various access policies are defined on the keyvault:
  - Grant access to an Azure AD group. Members from that group (devops engineer) can access from the CAF rover to build or improve an existing landing zone. 
  - Grant access to the Azure devops agents to the secrets

## GitOps services
The current version only support Azure Devops organizations. To deploy the launchpad you need to reference an Azure Devops projects. When you deploy the launchpad the following services are enabled:
- <b>Git Repository</b> - stores a master repository of your private launchpad, your landing zones and blueprints.
- <b>Configuration Registry</b> - stores the configuration settings for the landing zones across different environments
- <b>Self hosted build agents</b> - build artifacts and push them into respositories
- <b>Self hosted release agents </b> - release agent to deploy the landing zones
- <b>Identity</b> - Create a set of Azure AD applications, security groups and managed service identities to enable a least privilege gitops environment. For example when a devops engineer deploys a landing zones the CAF rover uses the logged-in Azure session to check if the user has access to the Keyvault access policy. If the devops engineer is member of the Azure AD security group the rover will pull some secrets and impersonate the Terraform deployments under that Azure AD application's privileges. We use that pattern to simplify the transition to a pipeline execution that only support Azure AD applications or MSIs.

## Transitioning from manual steps to 100% automation
The problem the launchpad is solving is the transition from manual steps to manage the lifecycle of your Azure infrastructure services to automated, tested and secured lifecycle maanagement of your landing zones. The main challenges customers are facing when starting their journey to the cloud is to define the logical steps to follow to enable the cloud services and get their first applications live into production. That journey can be quite long depending on the complexity of the organization, internal compliance processes and regulation imposed to the industry.

The Microsoft Cloud Adoption Framework defines various steps to help the customer defining that journey and for each steps include guidance, checklist and recommendations. The objectives of the CAF Terraform Landing Zones is to provide an Azure environment that will help the customer accelerate that adoption journey by learning through building and deploying landing zones and blueprints.

Infrastructure as Code (IaC) needs an environment landscape to enable innovation whithout impacting a development or production environment. Therefore embracing the software industry best practices like building artifacts, using semantic versioning, testing through different dimensions like integration, performance, reliability, disaster revory, load and releasing to non-production and production is the new norm to deliver enterprise grade IaC.

The successful implementations tend to focus on building first a sandpit / innovation hub environment where all the stakeholders (IT operations, security, compliance, information protection, finance and business) define their requirements. The devops team build, automate, test modules, blueprints and landing zones to create an infrastructure environment that is good enough.

The CAF open source landing zones deliver those services to enable multiple devops developers to co-create, reuse and configure the various parts of the landing zones
* <b>CAF currated modules</b> - is a set of community-driven terraform module writen for a production grade environment. They are the building blocks of the blueprints. They include the traditional non-function requirements (NFR) like diagnostics, naming convention, tagging, cost management and security to name few of them. The modules guarantee some consistency across all blueprints. For example a module can be a virtual machine, a SQL database
* <b>CAF blueprints</b> - the blueprints or terraform services consumes multiples CAF currated modules to build an infrastructure solution. There are some examples in the CAF Terraform edition of public blueprints shared mainly to show our customers how to build a blueprint. Blueprints can be private. Some partner are already building industry specific blueprints. In the CAF Terraform framework we have different categories of blueprints like governance, security, operations, networking or infrastructure platforms like Azure Kubernetes Services, SAP Hana or Azure Web App to name few of them. 
* <b>CAF landing zone</b> - a landing zone orchestrates the deployment of multiple blueprints. It targets an Azure subscription to deploy the landing zones.

## Requirements
To support the feature set of the open source launchpad you need to get access to an Azure subscription created from an Azure Enterprise Agreement portal. 

Note if you deploy in an Azure subscription created outside an Enterprise Agreement you will not be able to leverage the subscription lifecycle management (create subscriptions as code).

The initial user require the following Azure Active Directory roles:
- <b>Application Administrator </b> - Create Azure AD application and service principals, grant admin consent 
- <b> User Administrator </b> - Create Azure AD group, add users to group
- <b> Guest Inviter </b> - Create user guest


## Proposed coming features
- Subscription management (create, delete) [github link #6](https://github.com/aztfmod/level0/issues/6)
- Cross-tenant management through Azure Light House (enterprise and managed service providers) [github link #7](https://github.com/aztfmod/level0/issues/7)
- Public IP address removal [github link #8](https://github.com/aztfmod/level0/issues/8)
- Password less to avoid password rotation [github link #9](https://github.com/aztfmod/level0/issues/9)
- Landing zone pipeline registration and execution [github link #10](https://github.com/aztfmod/level0/issues/10)
- Transparent data encryption with BYOK [github link #11](https://github.com/aztfmod/level0/issues/11)
- Private link / Service endpoint between services [github link #12](https://github.com/aztfmod/level0/issues/12)
- VPN Gateway server for point to site access to the launchpad environment from the CAF rover [github link #13](https://github.com/aztfmod/level0/issues/13)
- Azure Active Directory MFA
- Azure Active Directory Privileged Identity Management
- Bastion for troubleshooting and human investigation [github link #14](https://github.com/aztfmod/level0/issues/14)

Not finding your feature, fill an issue to document it and start contributing by submitting a PR

Ready to give it a go in your environment? Read the on-boarding guide

Interested in improving the open source launchpad? Read the following developer guide