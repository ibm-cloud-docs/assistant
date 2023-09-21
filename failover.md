---

copyright:
  years: 2021, 2023
lastupdated: "2023-01-26"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Failover options](/docs/watson-assistant?topic=watson-assistant-admin-failover){: external} To see all documentation for the new {{site.data.keyword.conversationshort}}, please go [here](https://cloud.ibm.com/docs/watson-assistant){: external}.
{: attention}

# Failover options
{: #failover}

Learn about options that you can use to increase the availability of {{site.data.keyword.assistant_classic_short}} for your organization.
{: shortdesc}

## Introduction
{: #failover-intro}

{{site.data.keyword.conversationfull}} instances are deployed across multizone regions (MZRs) in all data centers (except Seoul, Korea) and can withstand the loss of any individual zone within an MZR. However, regional outages can occur and while IBM strives to keep outages to a minimum, you might want to consider moving traffic to a different region for your business-critical applications.

You should have a copy of your {{site.data.keyword.assistant_classic_short}} instance in a different MZR (for example, deploy your assistant in US East and US South). You must purchase a second service instance of {{site.data.keyword.assistant_classic_short}}, and second instances of the necessary assistants and skills need to be instantiated. You must implement change management controls to synchronize the changes between the two regions. {{site.data.keyword.assistant_classic_short}} and Speech services have APIs available to export and import the definitions.

With two instances, consider two topologies:

- **Active/active**: Both instances always serving traffic with the load sprayed between the two.
- **Active/passive**: One instance is active and the passive site receives traffic only if a failover happens.  

Each approach has pros and cons. Considerations specific to {{site.data.keyword.assistant_classic_short}} are detailed in the sections that follow.

## Considerations
{: #failover-considerations}

{{site.data.keyword.assistant_classic_short}} instances in one region are unaware of instances in a second region, which can affect some features and capabilities in {{site.data.keyword.assistant_classic_short}}.

### Analytics
{: #failover-analytics}

{{site.data.keyword.assistant_classic_short}} analytics provide overview statistics on the number of interactions with users and containment rates. Analytics doesn't cumulate statistics across regions. With an active/passive topology, this approach to analytics is sufficient. However, using an active/active topology likely requires you to use [webhooks](/docs/watson-assistant?topic=watson-assistant-webhook-overview) to gather interaction data, and build custom data warehouses and reports to understand total usage.

### Session history for web chat and the v2 api
{: #failover-session-history}

Session history allows your web chats to maintain conversation history and context when users refresh a page or change to a different page on the same website. This feature doesn't work across instances, so in-progress conversations need to be restarted.

### Billing
{: #failover-billing}

IBM calculates your bill based on the IBM Cloud Account. {{site.data.keyword.assistant_classic_short}} calculates monthly average user (MAU) metrics by aggregating within a specific service instance as follows:
- The same MAU used in 2 different assistant resources in the **same** service instance counts as 1 MAU
- The same MAU used in 2 different assistant resources in **different** service instances counts as 2 MAUs

For an **active/active** topology, under the worst case scenario, the MAU count might end up being doubled for a billing period.

## Phone integration
{: #failover-phone}

A {{site.data.keyword.assistant_classic_short}} phone integration in one region is unaware of a phone integration in a different region. You need to ensure that your assistants are identically configured in both regions. You also need to rely on the upstream SIP trunking provider to detect and manage failing over between regions.

### Monitoring
{: #failover-phone-monitoring}

SIP trunking providers can be configured to actively health-check the {{site.data.keyword.assistant_classic_short}} session border controllers (SBCs) by sending periodic SIP OPTIONS messages to each zone within a region. A failure to receive a response can be used to either provide notification of a failure to trigger a manual failover, or to automate removal of the failed zone from the route list.

### Failover
{: #failover-phone-failover}

The SIP trunking provider plays an important role in detecting and managing a failover, especially if an automatic failover is expected between regions. In most cases, SIP trunking providers should be configured to treat each zone within a region as active/active and two regions where an assistant is configured as active/passive. SIP trunking providers should always be configured to balance the load and fail over between zones within a single region.

### Full outage
{: #failover-phone-full-outage}

Phone integration failures have two types. The first type is a full outage where the session border controllers in all 3 regional zones become unreachable. This type of outage is easier to detect and handle because the SIP trunking provider is immediately notified by SIP timeouts that the call fails and can be configured to either automatically fail over or the call routing can be manually reconfigured at the SIP trunking provider to direct traffic away from the failed region toward the passive backup region. If a failover is automated and a regional backup is enabled, it is always best to try a different zone first and redirect traffic to the passive backup region only if a preconfigured number of failures occur within a short period. This prevents an unnecessary failover between regions if only a short outage occurs. 

{{site.data.keyword.assistant_classic_short}} provides a round-robin fully qualified domain name (FQDN) that includes the IP addresses for each zone in the region. Many SIP trunking providers automatically retry each IP in the FQDN when failures occur. To support disaster recovery, the service provider might need to configure two separate SIP trunks, one for each region, and only when all the zones in a single region fail should the call be switched to the backup region. It's important to set the SIP INVITE failure timeouts at the SIP trunking provider low enough to avoid long call setup latencies when a failover is occurring. 

### Partial outage
{: #failover-phone-partial-outage}

The second type of failure is a partial service outage within the region. A partial outage is much harder to detect and manage because of the large number of variations in service failures that can occur within a region. In some cases, small issues affect the performance characteristics of the call but not cause the call to fail. 

For issues that ultimately cause a call to fail, two ways {{site.data.keyword.assistant_classic_short}} can handle the call. The first is to accept the call and then transfer it to a configured default SIP URI. You can configure this setting in the {{site.data.keyword.assistant_classic_short}} phone integration and is also used for mid-call failures. The default transfer target SIP URI is defined in the **SIP target when a call fails** field that is on the Advanced tab of the phone integration configuration.

The phone integration can also be configured to respond to a SIP INVITE with a SIP 500 (service unavailable) message if an outage is detected during call setup instead of transferring a call to a live agent. A SIP 500 can then be used to redirect the call to another zone, or if many SIP 500s are received, to another region. Using a SIP 500 INVITE error is a better way to signal a failure to an upstream SIP trunking provider because it gives the provider a way to reroute the call. Using only the default transfer target to handle call failures is acceptable for low call volume scenarios but can result in large numbers of calls that are directed to a customer contact center when bigger call volumes are handled by {{site.data.keyword.assistant_classic_short}}. To enable this 500 error response capability for a specific {{site.data.keyword.assistant_classic_short}} instance, you need to make a request to IBM. 

You should plan for both full and partial service outages. A good first step is to plan for a manual failover between regions before you enable automation. You need a complete replica of {{site.data.keyword.assistant_classic_short}} in both regions, including all custom speech model training. When automation is enabled, it is best to start with a strategy for detecting and failing over to the passive backup region when a complete regional outage is detected. After implementation, develop a strategy to deal with partial outages, which should cover most of the failure conditions that can occur with a phone integration deployment.

## Web chat
{: #failover-webchat}

### Monitoring
{: #failover-webchat-monitoring}

Web chat provides an `onError` listening feature that allows the host page to detect specific types of outage errors, in particular INITIAL_CONFIG, OPEN_CONFIG, and MESSAGE_COMMUNICATION errors.

For more information, see [Listening for errors](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#onerror-detail){: external}.

### Failover
{: #failover-webchat-failover}

Handling a failover for web chat is simple, assuming you set up an extra web chat integration in another region. When the failover needs to be manually triggered, make the following changes:

- The embed script that contains your integration ID, region, service instance ID, and subscription ID (if applicable) needs to be changed or updated to use the IDs for the new integration and region.
- If you are using Salesforce or ZenDesk integrations for connecting to human agents, update the configuration within those systems to make sure they can communicate with the correct integration. Follow the instructions on the **Live agent** tab in the web chat configuration for setting up those systems. This capability is only needed for obtaining the conversation history for the agent.
- If you enable web chat security and you are using encrypted payloads, the IBM-provided public key that is used for the encryption might be different depending on the region. If so, you need to update the system that generates the JSON Web Token (JWT) to use the correct key.

You can set up an active/active configuration of web chat by using different integration IDs in your embed scripts, and by ensuring the web chat integration is "sticky" by user. Otherwise, if the user fails over to a different integration, the conversation history might be lost.

## API
{: #failover-api}

### Monitoring
{: #failover-api-monitoring}

Capture and monitor response codes of `/message` calls. Response codes of live `/message` traffic should be captured and aggregated.

Ideally, save the response code statistics in an external persistence store. This shared store can aggregate response code results from all deployed instances of your application and provide the greatest fidelity in determining whether a failover should be triggered.

Alternatively, response codes can be aggregated and evaluated in memory for an instance of your application. As only a fraction of traffic is contributing to the failover decision, this might affect how often the failover decision is acted upon.

With either aggregation approach, exceeding a defined threshold might trigger a failover. Common approaches for determining a failover include:
- Percentage-based: greater than X% of requests return a non-200 response code
- Consecutive-based: X calls in a row return a non-200 response code
- Limit-based: X calls return a non-200 response in a time frame

To avoid an unnecessary failover between regions, make sure that a robust retry mechanism is present when calling the `/message` endpoint.

### Failover for v1 and v2 stateless API
{: #failover-api-stateless}

For a successful failover:
- Training data changes should be synchronized across regions. Avoid pushing changes delayed over a large window of time (such as days) to mitigate the risk of algorithm changes that are deployed by {{site.data.keyword.assistant_classic_short}} in between regions being updated.
- The same IBM Cloud account should be used for the service instances across regions to maintain a single overall bill for services.
- The client applications should support:
    - {{site.data.keyword.assistant_classic_short}} API hostname
    - Service instance credentials
    - v1: workspace_id
    - v2: assistant_id

Although it does not affect the runtime flow of calling `/message`, if you are using fine-grained access control that uses IBM Identity and Access Management (IAM), make sure that the IAM policies are synchronized across the regions. 

IAM is a global service, but the custom resources (assistants and skills) used by {{site.data.keyword.assistant_classic_short}} access control means each region, which has specific resources, requires specific policies.

For an **active/passive** topology, some form of a [circuit break pattern](https://martinfowler.com/bliki/CircuitBreaker.html) can be used. A single service instance in a region is used exclusively unless errors are detected. At that point, the system can respond by updating the relevant failover metadata to route traffic to the service instance in the other region. When a failover happens, you can decide to continue using the new region as the active instance, or if you want to resume using the initial region when it stabilizes. 

For an **active/active** topology, some form of a load balancing can be used, where two or more service instances in different regions always receive a percentage of traffic. Extra logic would need to be established to determine when to pull a region out of rotation. This monitoring logic might use a [circuit break pattern](https://martinfowler.com/bliki/CircuitBreaker.html) similar to the active/passive configuration or rely on a separate dedicated monitoring framework that determines region health. Similar to active/passive, determining when to insert a region back in rotation would need to be considered as well. 

### Failover for v2 stateful API
{: #failover-api-stateful}

Failover for the v2 stateful API is similar to stateless, with these details to consider:
- The state of a conversation is persisted by {{site.data.keyword.assistant_classic_short}} in a database that is tied to a particular region. As such, a failover for the stateful v2 `/message` may be more disruptive.
- For an **active/passive** topology, you should assume that all in-progress conversations are ended.
- For an **active/active** topology, given the region-locked persistence constraints of the v2 stateful `/message` architecture, all turns (`/message` API calls) of a conversation (session) should occur within the same region.
