# Microsoft Container Registry (MCR) Client Firewall Rules Configuration

MCR is an implementation of the [OCI Distribution Specification][oci-spec] which delivers [artifacts][oci-artifacts], such as container images. The Distribution Spec defines two endpoints:

- **REST Endpoint**: providing content discovery. This is the url users are most familiar with with pulling an image: `docker pull mcr.microsoft.com/windows/servercore:1909`. The REST endpoint is load balanced across multiple worldwide regions, providing consistent content addressable artifacts.
- **Data Endpoint**: providing content delivery. As a registry client discovers the content required, it negotiates a set of content urls, pulling from the data endpoint. To provide reliable delivery of content, the data endpoint is backed by regionalized CDN endpoints.

## Table of Contents

- [Configuring Client Firewall Rules](#configuring-client-firewall-rules)
- [Data Endpoint Change](#data-endpoint-change)
- [Registry FQDN Endpoints](#registry-fqdn-endpoints)
- [Testing the `*.cdn.mscr.io` Data Endpoint](#testing-the-cdnmscrio-data-endpoint)
- [Testing the `*.data.mcr.microsoft.com` Data Endpoint](#testing-the-datamcrmicrosoftcom-data-endpoint)
- [Q&A](#qa)
- [Rollout Status](#rollout-status)

## Configuring Client Firewall Rules

> ### **PROACTIVE DATA ENDPOINT SECURITY CHANGE**
>
> **Rollouts have begun.**  See [Rollout Status](#rollout-status) for regional deployments.
>
> To proactively address security risks and concerns, MCR will provide a consistent FQDN between the REST and data endpoints. Beginning **June 15, 2020** the data endpoint will change from `*.cdn.mscr.io` to `*.data.mcr.microsoft.com`

## Registry FQDN Endpoints

To access MCR, the following FQDNs are required.

| Purpose | Protocol | Target FQDN | Available |
| - | - | - | - |
| Registry Endpoint | https | `mcr.microsoft.com` | Now |
| Data Endpoint | https | `*.cdn.mscr.io` | Now |
| Data Endpoint | https | `*.data.mcr.microsoft.com` | Starting June  15, 2020 |

If you're updating this before the transition is complete, adding both data endpoints will assure a smooth transition. Once the deployment is complete, we'll update this page enabling the removal of `*.cdn.mscr.io`. We recommend keeping both endpoints for a few weeks post the change to account for any extra ordinary situation where we might have to rollback.

![Azure Application Rule](./media/mcr-client-firewall-rules.png)

## Testing the `*.cdn.mscr.io` Data Endpoint

To validate configurations pre June 15th, 2020, the following multi-arch images are available for testing.

### Windows & Linux

To test pulling an image:  
`docker pull mcr.microsoft.com/mcr/hello-world`

Visualizing the data urls:  
`curl https://mcr.microsoft.com/v2/hello-world/blobs/sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e`

produces a url with `[oldRegionReference].cdn.mscr.io` :

```html
<a href="https://mcrwcus0.cdn.mscr.io/f439446e126348d29d8a8c39253da1ca-gqcvpppbjg//docker/registry/v2/blobs/...
```

## Testing the `*.data.mcr.microsoft.com` Data Endpoint

To validate changes will work post June 15, 2020, the following images are available for testing today.

**Note:** Separate repos for Windows and Linux are provided for verification:

### Windows

`docker pull mcr.microsoft.com/mcrlabscdnone/hello-world`

Visualizing the data urls:  
`curl  https://mcr.microsoft.com/v2/mcrlabscdnone/hello-world/blobs/sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e`

produces a url with `[region].data.mcr.microsoft.com` :

```html
<a href="https://westcentralus.data.mcr.microsoft.com/f439446e126348d29d8a8c39253da1ca-gqcvpppbjg//docker/registry/v2/blobs/...
```

### Linux

`docker pull mcr.microsoft.com/mcrlabscdnone/hello-world-linux`

Visualizing the data urls:  
`curl  https://mcr.microsoft.com/v2/mcrlabscdnone/hello-world-linux/blobs/sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e`

produces a url with `[region].data.mcr.microsoft.com` :

```html
<a href="https://westcentralus.data.mcr.microsoft.com/f439446e126348d29d8a8c39253da1ca-gqcvpppbjg//docker/registry/v2/blobs/...
```

### Q&A

- **Q: If both domains are secure, why are you making this change?**  
  **A:** We've received feedback that having multiple root domains is confusing as it's not obvious that registries require multiple endpoints. To proactively address any potential security issues we are putting both endpoints under `microsoft.com` providing more trust and confidence that these are operated by Microsoft.

- **Q: What should I do to prepare for this change?**  
  **A:** If you require client firewall rules, follow the above guidance, adding both data endpoints to your outbound firewall rules. If you do not use outbound firewall rules, no change is necessary.
- **Q: Are there any risks to adding both endpoints?**  
  **A:** Both data endpoints are owned and maintained by Microsoft. Both are secure. Adding both domains eases the transition.
- **Q: How can I test my firewall configurations before the change?"**  
  **A:** See [Testing the `*.data.mcr.microsoft.com` Data Endpoint](#testing-the-datamcrmicrosoftcom-data-endpoint) above.

## Support

To contact Microsoft Support: [azure.microsoft.com/support/create-ticket/](https://azure.microsoft.com/support/create-ticket/).

## Rollout Status

### June 19, 2020

MCR Data Endpoint changes rolling out.
The following chart represents regional rollout, following [Azure Safe Deployment Practices](https://azure.microsoft.com/en-us/blog/advancing-safe-deployment-practices). For this impactful change, we will take a pause between regions to monitor customer feedback.

| Date                  | Regions         | Data Endpoints                       | Status   |
| -                     | -               | -                                    | -        |
| 6/12/2020 - Friday    | EUAP Central US | N/A                                  | Complete |
| 6/13/2020 - Saturday  | EUAP East US2   | N/A                                  | Complete |
| 6/15/2020 - Monday    | West Central US | westcentralus.data.mcr.microsoft.com | Complete |
| 6/18/2020 - Thursday  | West US         | westus.data.mcr.microsoft.com        | Complete |
| 6/22/2020 - Monday    | East US         | eastus.data.mcr.microsoft.com        | Queued   |
|                       | North Europe    | northeurope.data.mcr.microsoft.com   | Queued   |
|                       | Southeast Asia  | southeastasia.data.mcr.microsoft.com | Queued   |
| 6/24/2020 - Wednesday | West US2        | westcentralus.data.mcr.microsoft.com | Queued   |
|                       | Central US      | centralus.data.mcr.microsoft.com     | Queued   |
|                       | East Asia       | eastasia.data.mcr.microsoft.com      | Queued   |
|                       | West Europe     | westeurope.data.mcr.microsoft.com    | Queued   |

> **NOTE:** Regions are provided here for troubleshooting the endpoint change from `mscr.io`. Over time, MCR regional endpoints _will change_ assuring the best performance for our customers. It is strongly recommended to use `*.data.mcr.microsoft.com` for firewall rules to avoid disruption as we alter regions for performance & readability.

### March 3rd, 2020 Update

Regional rollout changes began, switching from `*.cdn.mscr.io` to `*.data.mcr.microsoft.com`.

### March 9th, 2020 Update

During the multi-region deployment, we discovered AKS node scaling and service updates could fail. To minimize the impact, we are rolling back the data endpoint change which completed Friday, March 13, 2020.

### March 16th, 2020 Update

As of March 13, 2020, all regions have been rolled back with data endpoints being served from `*.cdn.mscr.io`

To minimize disruptions, allowing teams to focus on other matters, the MCR team has extended the change date to June 15, 2020.  
*See [Q&A](#qa) above*

More information on the [Microsoft Container Registry][mcr]

[azure-safe-deployment]: https://azure.microsoft.com/blog/advancing-safe-deployment-practices/
[mcr]:                   https://aka.ms/mcr
[oci-spec]:              https://github.com/opencontainers/distribution-spec
[oci-artifacts]:         https://github.com/opencontainers/artifacts
