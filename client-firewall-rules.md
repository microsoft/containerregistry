# Microsoft Container Registry (MCR) Client Firewall Rules Configuration

MCR is an implementation of the [OCI Distribution Specification][oci-spec] which delivers [artifacts][oci-artifacts], such as container images. The Distribution Spec defines two endpoints:

- **REST Endpoint**: providing content discovery. This is the url users are most familiar with with pulling an image: `docker pull mcr.microsoft.com/windows/servercore:1909`. The REST endpoint is load balanced across multiple worldwide regions, providing consistent content addressable artifacts.
- **Data Endpoint**: providing content delivery. As a registry client discovers the content requried, it negotiates a set of content urls, pulling from the data endpoint.

## CDN Backed, Regionalized Data Endpoints

To provide reliable delivery of content, the data endpoint is backed by regionalzied CDN endpoints.

These endpoints follow the convention of: `[region].data.mcr.microsoft.com`.

## Configuring Client Firewall Rules



> ### **DATA ENDPOINT CHANGE**
> To provide a consistent FQDN between the REST and data endpoints, beginning ~~March 3,~~ **April 13 2020** the data endpoint will be changing from `*.cdn.mscr.io` to `*.data.mcr.microsoft.com`  
Deployments follow [Azure Safe Deployment Practices][azure-safe-deployment], which means changes will rollout, per region, over several days.  
Once a change of the data endpoint is confirmed, clients can remove the `*.cdn.mscr.io` FQDN.  
**March 9th Update:**  
During the multi-region deployment, we discovered AKS node scaling and service updates could fail. To minimize the impact, we are rolling back the data endpoint change which will complete Friday, March 13, 2020.  
**New Deployment:** To assure all MCR customers have consistent and trusted domains between the registry and data endpoints, we will start a new `*.data.mcr.microsoft.com` data endpoint deployment **Monday April 13, 2020**.

To access MCR, the following FQDNs are required. 

| Purpose | Protocol | Target FQDN | Available |
| - | - | - | - |
| Registry Endpoint | https | `mcr.microsoft.com` | Now |
| Data Endpoint | https | `*.cdn.mscr.io` | Now - April 20, 2020 |
| Data Endpoint | https | `*.data.mcr.microsoft.com` | Starting April  13, 2020 |

If you're updating this prior to April 13, adding both date endpoints will assure a smooth transition. Once the consolidated data endpoint deployment is complete, we'll update this page.

![Azure Application Rule](./media/mcr-client-firewall-rules.png)


More information on the [Microsoft Container Registry][mcr]

[azure-safe-deployment]: https://azure.microsoft.com/blog/advancing-safe-deployment-practices/
[mcr]:                   https://aka.ms/mcr
[oci-spec]:              https://github.com/opencontainers/distribution-spec
[oci-artifacts]:         https://github.com/opencontainers/artifacts
