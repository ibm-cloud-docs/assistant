---

copyright:
  years: 2021
lastupdated: "2021-06-18"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:preview: .preview}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Failover options
{: #failover}

This document explains some actions you can take to increase the availability of Watson Assistant for your organization.
{: shortdesc}

## Introduction
{: #failover-intro}

IBM Watson Assistant instances are deployed across multizone regions (MZRs) in all data centers (except Seoul, Korea) and can withstand the loss of any individual zone within an MZR. However, regional outages can occur and while IBM strives to keep these to a minimum, you may want to consider moving traffic to a different region for your business-critical applications.

To achieve this, you should have a copy of your Watson Assistant instance in a different MZR (for example, have your assistant deployed in US East and US South). To do this, you must purchase a second service instance of Watson Assistant, and second instances of the necessary assistants and skills need to be instantiated. You must implement change management controls to synchronize the changes between the two regions. Watson Assistant and Speech services have APIs available to export and import the definitions.

With two instances, there are two topologies to consider:

- **Active/active**: Both instances always serving traffic with the load sprayed between the two.
- **Active/passive**: One instance is active and the passive site only receives traffic in the event of a failover.  

There are pros and cons to both approaches, and considerations specific to Watson Assistant are detailed in the sections that follow.

## Considerations
{: #failover-considerations}

Watson Assistant instances in one region are unaware of instances in a second region, which can affect some features and capabilities in Watson Assistant.

### Analytics
{: #failover-analytics}

Watson Assistant analytics provides overview statistics on the number of interactions with users and containment rates. Analytics doesn't cumulate statistics across regions. With an active/passive topology, this approach to analytics should be sufficient. However, using an active/active topology likely requires using [webhooks](/docs/assistant?topic=assistant-webhook-log) to gather interaction data, and build custom data warehouses and reports to understand total usage.

### Recommendations
{: #failover-recommendations}

The intent and user example recommendation features use production data to make the recommendations. The available recommendations may vary by instance due to differences in the data across regions.

### Autolearning
{: #failover-autolearning}

The autolearning feature uses production data to train an improved intent recognizer. The autolearning results may vary by instance due to differences in the data across regions.

### Session history for web chat and the v2 api
{: #failover-session-history}

Session history allows your web chats to maintain conversation history and context when users refresh a page or change to a different page on the same website. This feature doesn't work across instances, so in-progress conversations need to be restarted.

### Billing
{: #failover-billing}

IBM calculates your bill based on the IBM Cloud Account. Watson Assistant calculates monthly average user (MAU) metrics by aggregating within a given service instance as follows:
- The same MAU used in 2 different assistant resources in the **same** service instance counts as 1 MAU
- The same MAU used in 2 different assistant resources in **different** service instances counts as 2 MAUs

Note that for an **active/active** topology, under the worst case scenario, the MAU count could end up being doubled for a given billing period.

## Phone integration
{: #failover-phone}

A Watson Assistant phone integration in one region is unaware of a phone integration in a different region. You need to ensure that your assistants are identically configured in both regions. You also need to rely on the upstream SIP trunking provider to detect and manage failing over between regions.

### Monitoring
{: #failover-phone-monitoring}

SIP trunking providers can be configured to actively health-check the Watson Assistant session border controllers (SBCs) by sending periodic SIP OPTIONS messages to each zone within a region. A failure to receive a response from one of the zones being health-checked can be used to either provide notification of a failure to trigger a manual failover, or it can be used to automate removal of the failed zone from the route list.

### Failover
{: #failover-phone-failover}

The SIP trunking provider plays an important role in detecting and managing a failover, especially if an automatic failover is expected between regions. In most cases, SIP trunking providers should be configured to treat each zone within a region as active/active and two regions where an assistant is configured as active/passive. SIP trunking providers should always be configured to load balance and fail over between zones within a single region. The remainder of this section focuses on handling a failover between regions. 

### Full service outage
{: #failover-phone-full-outage}

Phone integration failures have two types. The first type is a full outage where the session border controllers in all 3 regional zones become unreachable. This is the easier of the two types to detect and handle because the SIP trunking provider is immediately notified via SIP timeouts that the call fails and can be configured to either automatically fail over or the call routing can be manually reconfigured at the SIP trunking provider to direct traffic away from the failed region towards the passive backup region. If a failover is automated and a regional backup is enabled, it is always best to try a different zone first and only redirect traffic to the passive backup region if a preconfigured number of failures occur within a short period of time. This prevents an unnecessary failover between regions if only a short outage occurs. 

Note that Watson Assistant provides a round-robin fully qualified domain name (FQDN) that includes the IPs for each zone in the region. Many SIP trunking providers automatically retry each IP in the FQDN when failures occur. To support disaster recovery, the service provider may need to configure two separate SIP trunks, one for each region, and only when all the zones in a single region fail should the call be switched to the backup region. It's important to set the SIP INVITE failure timeouts at the SIP trunking provider low enough to avoid long call setup latencies when a failover is occurring. 

### Partial service outage
{: #failover-phone-partial-outage}

The second type of failure is a partial service outage within the region. This is much harder to detect and manage because of the large number of variations in service failures that can occur within a region. In some cases, these may be small issues that affect the performance characteristics of the call but not cause the call to fail. 

For issues like this that ultimately cause a call to fail, there are two ways Watson Assistant can handle the call. The first is to accept the call and then transfer it to a configured default SIP URI. This can be configured in the Watson Assistant phone integration settings and is also used if there is a mid-call failure. The default transfer target SIP URI is defined in the **SIP target when a call fails** field located on the Advanced tab of the phone integration configuration panel.

The phone integration can also be configured to respond to a SIP INVITE with a SIP 500 (service unavailable) message if an outage is detected during call setup instead of transferring a call to a live agent. A SIP 500 can then be used to redirect the call to another zone, or if many SIP 500s are received, to another region. Using a SIP 500 INVITE error is a better way to signal a failure to an upstream SIP trunking provider because it gives the provider a way to reroute the call. Using only the default transfer target to handle call failures is acceptable for low call volume scenarios but can result in large numbers of calls being directed to a customer contact center when bigger call volumes are handled by Watson Assistant. To enable this 500 error response capability for a specific Watson Assistant instance, you need to make a request to IBM. 

You should plan for both full and partial service outages. A good first step is to plan for a manual failover between regions before enabling automation. This requires a complete replica of Watson Assistant in both regions, including all custom speech model training. When automation is enabled it is best to start with a strategy for detecting and failing over to the passive backup region when a complete regional outage is detected. After this is in place, a strategy can be implemented to deal with partial outages, which should cover the vast majority of failure conditions that can occur with a phone integration deployment.

## Web chat
{: #failover-webchat}

### Monitoring
{: #failover-webchat-monitoring}

Web chat provides an `onError` listening feature that allows the host page to detect specific types of outage errors, in particular INITIAL_CONFIG, OPEN_CONFIG, and MESSAGE_COMMUNICATION errors.

You can find the documentation for this feature here: https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#onerror-detail

### Failover
{: #failover-webchat-failover}


Handling a failover for web chat is simple assuming you have set up an additional web chat integration in another region. When the failover needs to be manually triggered, make the following changes:

- The embed script that contains your integration ID, region, service instance ID, and subscription ID (if applicable) needs be changed or updated to use the IDs for the new integration and region.
- If you are using Salesforce or ZenDesk integrations for connecting to human agents, update the configuration within those systems to make sure they can communicate with the correct integration. Follow the instructions on the **Live agent** tab in the web chat configuration for setting up those systems. This is only needed for obtaining the conversation history for the agent.
- If you have web chat security enabled and you are using encrypted payloads, the IBM-provided public key used for the encryption may be different depending on region. If so, you need to update the system that generates the JSON Web Token (JWT) to use the correct key.

You can set up an active/active configuration of web chat by using different integration IDs in your embed scripts, and by ensuring the web chat integration is "sticky" by user. Otherwise, if the user fails over to a different integration, the conversation history might be lost.

## API
{: #failover-api}

### Monitoring

Capture and monitor response codes of /message calls. Response codes of live /message traffic should be captured and aggregated.

Ideally, save the response code statistics in an external persistence store. This shared store can aggregate response code results from all deployed instances of a your  application and provide the greatest fidelity in determining if a failover should be triggered.

Alternatively, response codes can be aggregated and evaluated in memory for a given instance of your application.  As only a fraction of traffic is contributing to the failover decision, this could lead to different behavior with respect to how often the failover decision is acted upon.

With either aggregation approach, exceeding a defined threshold could trigger a failover. Common approaches for determining a failover include:
- Percentage-based: greater than X% of requests return a non-200 response code
- Consecutive-based: X calls in a row return a non-200 response code
- Limit-based: X calls return a non-200 response in a given timeframe

To avoid an unnecessary failover between regions, make sure a robust retry mechanism is present when calling the /message endpoint.


### Failover for v1 and v2 stateless API

For a successful failover, ensure the following:
- Training data changes should be synchronized across regions. Avoid pushing changes delayed over a large window of time (such as days) to mitigate risk of algorithm changes being deployed by Watson Assistant in between regions being updated.
- The same IBM Cloud account should be used for the service instances across regions to maintain a single overall bill for services.
- The client applications should support:
  - Watson Assistant API hostname
  - Service instance credentials
  - v1: workspace_id
  - v2: assistant_id

While it does not affect the runtime flow (calling /message), if you are using IBM Access Control (IAM), make sure IAM policies are appropriately synchronized across the regions as well if you are using fine-grained access control. 

Note that IAM is a global service, but the custom resources (assistants and skills) used by {{site.data.keyword.conversationshort}} access control means each region, which has specific resources, requires specific policies.

For an **active/passive** topology, some form of a [circuit break pattern](https://martinfowler.com/bliki/CircuitBreaker.html) can be used. A single service instance in a given region is used exclusively unless errors are detected. At that point, the system can respond by updating the relevant failover metadata to route traffic to the service instance in the other region. Once a failover happens, you can decide to continue using the new region as the active instance, or if you want to resume using the initial region once it has stabilized. 

For an **active/active** topology, some form of a load balancing can be used, where two or more service instances in unique regions always receive a percentage of traffic. Additional logic would need to be established to determine when to pull a region out of rotation. This monitoring logic could use a [circuit break pattern](https://martinfowler.com/bliki/CircuitBreaker.html) similar to the active/passive configuration or rely on a separate dedicated monitoring framework that determines region health. Also similar to active/passive, determining when to insert a region back in rotation would need to be considered as well. 

### Failover for v2 stateful API

Failover for the v2 stateful API is similar to stateless, with these details to consider:
- The state of a given conversation is persisted by {{site.data.keyword.conversationshort}} in a database that is tied to a particular region.  As such, a failover for the stateful v2 /message may more disruptive.
- For an **active/passive** topology, you should assume that all in-progress conversations are ended.
- For an **active/active** topology, given the persistence constraints (region-locked) of the v2 stateful /message architecture, all turns (/message API calls) of a given conversation (session) should occur within the same region.
