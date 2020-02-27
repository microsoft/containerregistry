# Microsoft Container Registry (MCR) Client Firewall Rules Configuration

MCR is an implementation of the [OCI Distribution Specification][oci-spec] which delivers [artifacts][oci-artifacts], such as container images. The Distribution Spec defines two endpoints:

- **REST Endpoint**: providing content discovery. This is the url users are most familiar with with pulling an image: `docker pull mcr.microsoft.com/windows/servercore:1909`. The REST endpoint is load balanced across multiple worldwide regions, providing consistent content addressable artifacts.
- **Data Endpoint**: providing content delivery. As a registry client discovers the content requried, it negotiates a set of content urls, pulling from the data endpoint.

## CDN Backed, Regionalized Data Endpoints

To provide reliable delivery of content, the data endpoint is backed by regionalzied CDN endpoints.

These endpoints follow the convention of: `[region].data.mcr.microsoft.com`.

## Configuring Client Firewall Rules

> ### **DATA ENDPOINT CHANGE**
> To provide a consistent FQDN between the REST and data endpoints, beginning **March 3, 2020** the data endpoint will be changing from `*.cdn.mscr.io` to `*.data.mcr.microsoft.com`  
Depoloyments follow [Azure Safe Deploymnet Pracitces][azure-safe-deployment], which means changes will roll out, per region, over several days.  
Once a change of the data endpoint is confirmed, clients can remove the `*.cdn.mscr.io` FQDN.

To access MCR, the following FQDNs are required. 

| Protocol | Target FQDN | Available |
| - | - | - |
| https | `mcr.microsoft.com` | Now |
| https | `*.cdn.mscr.io` | Now - March 3, 2020 |
| https | `*.data.mcr.microsoft.com` | Starting March 3, 2020 |

![Azure Application Rule](./media/mcr-client-firewall-rules.png)

## Testing the `*.cdn.mscr.io` Endpoint

To validate configurations pre March 3rd, 2020, the following multi-arch images are availalble for testing. 

### Windows & Linux
To test pulling an image:  
`docker pull mcr.microsoft.com/mcr/hello-world`

Visualizing the data urls:  
`curl https://mcr.microsoft.com/v2/hello-world/blobs/sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e`

produces a url with `[oldRegionReference].cdn.mscr.io` :
```
<a href="https://mcrwcus0.cdn.mscr.io/f439446e126348d29d8a8c39253da1ca-gqcvpppbjg//docker/registry/v2/blobs/...
```

## Testing the `*.data.mcr.microsoft.com` Data Endpoint

To validate changes will work post March 3rd, 2020, the following images are availalble for testing today.

**Note:** Seperate repos for Windows and Linux are provided for verification:

### Windows

`docker pull mcr.microsoft.com/mcrlabscdnone/hello-world`

Visualizing the data urls:  
`curl  https://mcr.microsoft.com/v2/mcrlabscdnone/hello-world/blobs/sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e`

produces a url with `[region].data.mcr.microsoft.com` :
```
<a href="https://westcentralus.data.mcr.microsoft.com/f439446e126348d29d8a8c39253da1ca-gqcvpppbjg//docker/registry/v2/blobs/...
```

### Linux

`docker pull mcr.microsoft.com/mcrlabscdnone/hello-world-linux`

Visualizing the data urls:  
`curl  https://mcr.microsoft.com/v2/mcrlabscdnone/hello-world-linux/blobs/sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e`

produces a url with `[region].data.mcr.microsoft.com` :
```
<a href="https://westcentralus.data.mcr.microsoft.com/f439446e126348d29d8a8c39253da1ca-gqcvpppbjg//docker/registry/v2/blobs/...
```

More information on the [Microsoft Container Registry][mcr]

[azure-safe-deployment]: https://azure.microsoft.com/blog/advancing-safe-deployment-practices/
[mcr]:                   https://aka.ms/mcr
[oci-spec]:              https://github.com/opencontainers/distribution-spec
[oci-artifacts]:         https://github.com/opencontainers/artifacts
