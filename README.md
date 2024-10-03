# Microsoft Artifact Registry (MAR)

Microsoft Artifact Registry (also known as Microsoft Container Registry or MCR) is the primary registry for all Microsoft published artifacts (such as container images) that offers a reliable and trustworthy delivery of artifacts with a syndicated catalog, while maintaining the quality that customers expect from a Microsoft product offering. 

For more background on MAR:

- [Microsoft syndicates container catalog (mcr.microsoft.com)](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/)
- [Improved discovery experience for Microsoft containers on Docker Hub
](https://cloudblogs.microsoft.com/opensource/2019/01/17/improved-discovery-experience-microsoft-containers-docker-hub/)

## Browsing MAR Content

The discovery experience for MAR is provided through the MAR discovery UI on [mcr.microsoft.com](https://mcr.microsoft.com/) and [Docker Hub](https://hub.docker.com/u/microsoft).

1. To browse the list of repositories within MAR, visit https://mcr.microsoft.com/

2. To browse the list of tags, within a repository:

    a. Visit https://mcr.microsoft.com/
    
    b. Select an artifact from the catalog listing

    c. Select the `Tags` tab to view the list of tags for that particular artifact

## Additional Guidance for the Use of MAR

- [Guidance for using the official MAR endpoints](./docs/mcr-endpoints-guidance.md)
- [Client Firewall Rules](./docs/client-firewall-rules.md)
- [Consuming Public Content](https://opencontainers.org/posts/blog/2020-10-30-consuming-public-content/)
- [Egress Limits](./docs/egress-limits.md)

## Providing Feedback

Please [open issues](https://github.com/microsoft/containerregistry/issues) to provide your feedback on usage, as well as any suggestions or concerns with MAR.

## FAQ

* **What is the difference between MAR and MCR?**

    There is no difference, MAR and MCR are the same product offering.

* **Will the endpoints change from mcr.microsoft.com to mar.microsoft.com?**

    No, all endpoints will use the existing `mcr.microsoft.com` structure.

* **How does MAR work with Docker Hub?**  

    MAR is a public registry for housing Microsoft's official artifacts (such as container images). Docker Hub is one of the official sources (in addition to https://mcr.microsoft.com) for our customers to discover official Microsoft-published container images. For further details of this integration please visit our [blog](https://cloudblogs.microsoft.com/opensource/2019/01/17/improved-discovery-experience-microsoft-containers-docker-hub/).

* **What is the difference between MAR and ACR (Azure Container Registry)?**  

    MAR is a public registry for housing only Microsoft's official artifacts (such as container images). ACR is a private container registry for housing our customers' container images.

* **Can I browse MAR?**

    Yes, customers can browse the MAR discovery UI at [mcr.microsoft.com](https://mcr.microsoft.com/). Customers can view rich content like descriptions and tag listings for each artifact.

* **Can I onboard new images to MAR?** 

    No, Given that MAR is meant to host only official Microsoft container images, only internal Microsoft product teams will be able to onboard images to the registry. Once onboarded, the images are available to all public users.

** Does MAR have an service level agreement (SLA)?

    MAR is globally distributed offering and does not have an SLA. You can view SLAs such as Azure Container Registry on [Microsoft SLA for Online Services](https://www.microsoft.com/licensing/docs/view/Service-Level-Agreements-SLA-for-Online-Services).

## Legal Notices

Microsoft and any contributors grant you a license to the Microsoft documentation and other content
in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode),
see the [LICENSE](LICENSE) file, and grant you a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the
[LICENSE-CODE](LICENSE-CODE) file.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at https://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all other rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.
