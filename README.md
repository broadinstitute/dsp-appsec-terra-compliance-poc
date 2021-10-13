# PoC of Infra Security Compliance for Terra on Azure 

These Azure Managed App deployment templates provide an example of how to integrate regulatory compliance for infrastructure security of Terra on Azure.
The following _Azure Policy Sets_ are included:
- [Azure Security Benchmark](https://docs.microsoft.com/en-us/azure/governance/policy/samples/azure-security-benchmark) - this set is technically enabled by default in Azure, but we need to explicitly enable it here so that we get the results reported centrally in the cross-tenant Azure Security Center (ASC) located in the publisher tenant
- [CIS Microsoft Azure Foundations Benchmark 1.3.0](https://docs.microsoft.com/en-us/azure/governance/policy/samples/cis-azure-1-3-0) - provides a standards-based set of security controls that are valued by compliance
- [Azure FedRAMP Moderate Regulatory Compliance](https://docs.microsoft.com/en-us/azure/governance/policy/samples/fedramp-moderate) - strictest set of formal regulatory compliance requirements for Terra

There is a significant overlap between these, but they all in combination should provide a good set of controls that will help us satisfy our FedRAMP Moderate (and possibly other) compliance requirements.

Note that _most_ of the policies in these sets work in an "audit only" mode, meaning that they will not interfere with your workloads; they are just used for compliance reporting in ASC (and ultimately will need to be enforced via standard development/DevSecOps practices).

However, a few controls are applied in an "enforcement" mode, which _may_ modify your existing deployments or prevent some resources from being deployed, if there're any deviations. You can check which controls are enforced using the links above (look for _Deny_, _modify_ and _deployIfNotExists_ policy effects).

Note that applying these Policy Sets as part of the Managed App deployment template serves several goals:
- The compliance controls are applied/enforced/monitored from the very beginning, before other resources have a chance to be deployed outside of the initial deployment, thus serving as a "gate" for it.
- These Policy Sets are maintained by Microsoft as a vendor, and are thus considered "complete". As a result, we have an easier time satisfying our compliance audits, because no controls are excluded or included from these "official" sets. In other words, we can defer to these sets for most of our infrastructure compliance reporting requirements.
- It's seamless and easy to apply as a "serverless" solution, since no additional infrastructure or user interaction is required for it to be applied.

Here's how the resulting ASC Recommendations based on these sets look like:
![image](https://user-images.githubusercontent.com/137337/137196157-4391bbe1-0f04-4e51-ab50-25230c702609.png)

Note that these controls are reported in a _centralized_ fashion and are thus easy for the publisher to review/scope down across all tenants. However, it _may_ take up to 24-48 hours for the findings to be _visible_ in publisher's ASC portal.

Also note that there _may_ be additional requirements we'll ask select customers to accept/satisfy in the future, e.g. to enable Azure Sentinel at Subscription level, for a more in-depth security monitoring in compliance-sensitive environments.
