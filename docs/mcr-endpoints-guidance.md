---
title: Guidance for using the official Microsoft Container Registry (MCR) endpoints
description: This articles provides information about the official endpoints of Microsoft Container Registry (MCR) and provides guidance how to use the REST and CDN endpoints to pull official Microsoft artifacts around the world.
ms.topic: guidance
ms.date: 02/03/2021
ms.author: memladen
author: toddysm
ms.custom:
---

# Guidance for using the official Microsoft Container Registry (MCR) endpoints

Microsoft Container Registry (MCR) is an implementation of the [OCI Distribution Specification][oci-spec] which delivers [artifacts][oci-artifacts], such as container images, helm charts and other assets distributed by Microsoft. The Distribution Spec linked above defines two endpoints:

- **Registry Endpoint**: Providing content discovery. This is also referred to as *REST Endpoint* and it is the endpoint users are most familiar with when pulling an image; for example: `docker pull mcr.microsoft.com/windows/servercore:1909`. The registry endpoint is load balanced across multiple worldwide regions, providing reliable and consistent access to the MCR artifacts catalog.
- **Data Endpoint**: Providing content delivery. As a registry client discovers the content required, it negotiates a set of content URLs, pulling from the data endpoint. To provide reliable delivery of content, the data endpoint is backed by regional CDN endpoints.

## Official FQDNs for MCR

To access MCR, the following FQDNs should be used.

| Purpose | Protocol | Target FQDN |
| - | - | - |
| Registry Endpoint | https | `mcr.microsoft.com` |
| Data Endpoint | https | `*.data.mcr.microsoft.com` |

> **Note:** MCR provides global coverage through Azure Traffic Manager for the registry endpoint, with regional CDNs managed by Azure Front Door for the data endpoints.
> Over time, MCR team will continue adding regional endpoints to ensure the best performance for our customers. Check the [MCR Firewall Rules][mcr-firewall-rules] article for details how to configure your firewall rules.

## MCR Registry Endpoint Locations

MCR registry REST endpoints are available in the following nine locations:

- Asia East
- Asia South East
- Europe North
- Europe West
- US Central
- US East
- US West
- US West 2
- US West-Central

## MCR Data Endpoint Locations

MCR uses Azure Front Door CDN point of presence (POP) locations to provide reliable delivery of Microsoft content. Single CDN POP location can have multiple nodes serving the MCR content. To obtain the list of CDN POP locations for MCR use the following articles from Azure Front Door:

- [Azure CDN Coverage by Metro - Worldwide][azure-cdn]
- [Azure CDN Coverage by Metro - China][azure-cdn-china]

## Use of Mirrors

MCR delivers content via the official REST and data endpoints listed above. The data endpoints are backed by the Azure Front Door CDN POP locations around the world. MCR **does not provide official mirrors** for serving content.

*Microsoft does not endorse any mirrors that serve MCR content and highly encourages its customers to use the official endpoints. The use of mirrors may result in altered content and supply chain exploits.*

## Support

If you experience issues with pulling artifact from MCR, [contact Microsoft Support](https://azure.microsoft.com/support/create-ticket/). You can also [submit an issue](https://github.com/microsoft/containerregistry/issues/new) in the official MCR GitHub repository.

[azure-cdn]:            https://docs.microsoft.com/azure/cdn/cdn-pop-locations
[azure-cdn-china]:      https://docs.azure.cn/cdn/cdn-pops
[oci-spec]:             https://github.com/opencontainers/distribution-spec
[oci-artifacts]:        https://github.com/opencontainers/artifacts
[mcr-firewall-rules]:   ../client-firewall-rules.md
