# Microsoft Artifact Registry (MAR) Client Firewall Rules Configuration

Microsoft Artifact Registry (also known as Microsoft Container Registry or MCR) is an implementation of the [OCI Distribution Specification][oci-spec] which delivers [artifacts][oci-artifacts], such as container images, helm charts and other assets distributed by Microsoft and Azure. The Distribution Spec defines two endpoints:

- **REST Endpoint**: providing content discovery. This is the url users are most familiar with when pulling an image: `docker pull mcr.microsoft.com/windows/servercore:1909`. The REST endpoint is load balanced across multiple worldwide regions, providing consistent content addressable artifacts.
- **Data Endpoint**: providing content delivery. As a registry client discovers the content required, it negotiates a set of content urls, pulling from the data endpoint. To provide reliable delivery of content, the data endpoint is backed by regionalized CDN endpoints.

## Configuring Client Firewall Rules

To access Registry Endpoint and MAR discovery UI, the following FQDNs are required.

| Purpose | Protocol | Target FQDN |
| - | - | - |
| Registry Endpoint and MAR discovery UI | https | `mcr.microsoft.com` |
| Data Endpoint | https | `*.data.mcr.microsoft.com` |
| Registry Endpoint for China Region | https | `mcr.azure.cn`
| Data Endpoint for China Region | https | `*.data.mcr.azure.cn` |

> **Note:** The China region endpoints (`mcr.azure.cn`) and (`*.data.mcr.azure.cn`) are counterparts to (`mcr.microsoft.com`) and (`*.data.mcr.microsoft.com`).


> **Note:** MAR provides global coverage through Azure Traffic Manager for the registry REST endpoint, with regional CDNs for the data endpoints.
> Over time, MAR regional endpoints _will change_ assuring the best performance for our customers. It is strongly recommended to avoid using region specific rules. The `*.data.mcr.microsoft.com` firewall rules will avoid disruption as we alter regions for performance & readability.

Within Azure, the following configuration may be applied:

![Azure Application Rule](/media/mcr-client-firewall-rules.png)

## Service Tag Usage
Microsoft Artifact Registry uses [Azure Front Door](https://learn.microsoft.com/en-us/azure/frontdoor/front-door-overview) as its entry point. If using service tags for firewall configuration, **both MicrosoftContainerRegistry and AzureFrontDoor.FirstParty [service tags](https://learn.microsoft.com/en-us/azure/virtual-network/service-tags-overview#available-service-tags) should be enabled.**
## Support

To contact Microsoft Support: [azure.microsoft.com/support/create-ticket/](https://azure.microsoft.com/support/create-ticket/).

More information on the [Microsoft Artifact Registry][mcr]

[azure-safe-deployment]: https://azure.microsoft.com/blog/advancing-safe-deployment-practices/
[mcr]:                   https://aka.ms/mcr
[oci-spec]:              https://github.com/opencontainers/distribution-spec
[oci-artifacts]:         https://github.com/opencontainers/artifacts
