# Microsoft Container Registry (MCR) Client Firewall Rules Configuration

MCR is an implementation of the [OCI Distribution Specification][oci-spec] which delivers [artifacts][oci-artifacts], such as container images. The Distribution Spec defines two endpoints:

- **REST Endpoint**: providing content discovery. This is the url users are most familiar with with pulling an image: `docker pull mcr.microsoft.com/windows/servercore:1909`. The REST endpoint is load balanced across multiple worldwide regions, providing consistent content addressable artifacts.
- **Data Endpoint**: providing content delivery. As a registry client discovers the content requried, it negotiates a set of content urls, pulling from the data endpoint.

## CDN Backed, Regionalized Data Endpoints

To provide reliable delivery of content, the data endpoint is backed by regionalzied CDN endpoints.

These endpoints follow the convention of: `[region].data.mcr.microsoft.com`.

## Configuring Client Firewall Rules

> ### **DATA ENDPOINT CHANGE**
> To provide a consistent FQDN between the REST and data endpoints, on **March 3, 2020** the data endpoint will be changing from `*.cdn.mscr.io` to `*.data.mcr.microsoft.com`  
  Once a change of the data endpoint is confirmed, clients can remove the `*.cdn.mscr.io` FQDN.

To access MCR, the following FQDNs are required. 

| Protocol | Target FQDN | Available |
| - | - | - |
| https | `mcr.microsoft.com` | Now |
| https | `*.cdn.mscr.io` | Now - March 3, 2020 |
| https | `*.data.mcr.microsoft.com` | March 3, 2020 |

![Azure Application Rule](./media/mcr-client-firewall-rules.png)

[More information on the Microsoft Container Registry][mcr]

[mcr]:            https://aka.ms/mcr
[oci-spec]:       https://github.com/opencontainers/distribution-spec
[oci-artifacts]:  https://github.com/opencontainers/artifacts

