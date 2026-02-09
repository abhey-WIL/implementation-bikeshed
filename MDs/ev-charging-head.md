Implementation Guide \- EV Charging \- Version 0.8 (DRAFT) <!-- omit from toc -->
==========================================================

## Table of Contents <!-- omit from toc -->

- [1. Request for Comments](#1-request-for-comments)
- [2. Copyright Notice](#2-copyright-notice)
- [3. Status of This Memo](#3-status-of-this-memo)
- [4. Overview](#4-overview)
- [5. Introduction](#5-introduction)
- [6. Scope](#6-scope)
- [7. Intended Audience](#7-intended-audience)
- [8. Conventions and Terminology](#8-conventions-and-terminology)
- [9. Terminology](#9-terminology)
- [10. Reference Architecture](#10-reference-architecture)
  - [10.1. Architecture Diagram](#101-architecture-diagram)
  - [10.2. Actors](#102-actors)
- [11. Creating an Open Network for EV Charging](#11-creating-an-open-network-for-ev-charging)
  - [11.1. Setting up a Registry](#111-setting-up-a-registry)
    - [11.1.1. For a Network Participant](#1111-for-a-network-participant)
      - [11.1.1.1. Step 1 :  Claiming a Namespace](#11111-step-1---claiming-a-namespace)
      - [11.1.1.2. Step 2 :  Setting up a Registry](#11112-step-2---setting-up-a-registry)
      - [11.1.1.3. Step 3 :  Publishing subscriber details](#11113-step-3---publishing-subscriber-details)
    - [11.1.2. Step 4 :  Share details of the registry created with the Beckn One team](#1112-step-4---share-details-of-the-registry-created-with-the-beckn-one-team)
    - [11.1.3. For a Network facilitator organization](#1113-for-a-network-facilitator-organization)
      - [11.1.3.1. Step 1 :  Claiming a Namespace](#11131-step-1---claiming-a-namespace)
      - [11.1.3.2. Step 2 :  Setting up a Registry](#11132-step-2---setting-up-a-registry)
      - [11.1.3.3. Step 3 :  Publishing subscriber details](#11133-step-3---publishing-subscriber-details)
      - [11.1.3.4. Step 4 :  Share details of the registry created with the Beckn One team](#11134-step-4---share-details-of-the-registry-created-with-the-beckn-one-team)
  - [11.2. Setting up the Protocol Endpoints](#112-setting-up-the-protocol-endpoints)
    - [11.2.1. Installing Beckn ONIX](#1121-installing-beckn-onix)
    - [11.2.2. Configuring Beckn ONIX for EV Charging Transactions](#1122-configuring-beckn-onix-for-ev-charging-transactions)
    - [11.2.3. 10.2.3 Performing a test EV charging transaction](#1123-1023-performing-a-test-ev-charging-transaction)
- [12. Implementing EV Charging Semantics on Beckn Protocol](#12-implementing-ev-charging-semantics-on-beckn-protocol)
  - [12.1. Key Assumptions](#121-key-assumptions)
  - [12.2. Semantic Model](#122-semantic-model)
  - [12.3. Example Category Codes](#123-example-category-codes)
- [13. Example Workflows (EV User’s Perspective)](#13-example-workflows-ev-users-perspective)
  - [13.1. Example 1 - Walk-In to a charging station without reservation.](#131-example-1---walk-in-to-a-charging-station-without-reservation)
    - [13.1.1. Consumer User Journey](#1311-consumer-user-journey)
    - [13.1.2. **API Calls and Schema**](#1312-api-calls-and-schema)
      - [13.1.2.1. `action: discover`](#13121-action-discover)
      - [13.1.2.2. `action: on_discover`](#13122-action-on_discover)
      - [13.1.2.3. `action: select`](#13123-action-select)
      - [13.1.2.4. `action: on_select`](#13124-action-on_select)
      - [13.1.2.5. `action: init`](#13125-action-init)
      - [13.1.2.6. `action: on_init`](#13126-action-on_init)
      - [13.1.2.7. 13.1.2.6.1 `action: on_status`](#13127-131261-action-on_status)
      - [13.1.2.8. `action: confirm`](#13128-action-confirm)
      - [13.1.2.9. `action: on_confirm`](#13129-action-on_confirm)
      - [13.1.2.10. `action: status`](#131210-action-status)
      - [13.1.2.11. `action: on_status`](#131211-action-on_status)
      - [13.1.2.12. `action: update` (start charging)](#131212-action-update-start-charging)
      - [13.1.2.13. `action: on_update` (start charging)](#131213-action-on_update-start-charging)
      - [13.1.2.14. `action: status` (charging-session progress)](#131214-action-status-charging-session-progress)
      - [13.1.2.15. `action: on_status`](#131215-action-on_status)
      - [13.1.2.16. async `action: on_status`](#131216-async-action-on_status)
      - [13.1.2.17. `action: on_update` (stop-charging)](#131217-action-on_update-stop-charging)
      - [13.1.2.18. async `action: on_update` (stop-charging)](#131218-async-action-on_update-stop-charging)
      - [13.1.2.19. `action: rating`](#131219-action-rating)
      - [13.1.2.20. `action: on_rating`](#131220-action-on_rating)
      - [13.1.2.21. `action: support`](#131221-action-support)
      - [13.1.2.22. `action: on_support`](#131222-action-on_support)
  - [13.2. Use case 2- Reservation of an EV charging time slot.](#132-use-case-2--reservation-of-an-ev-charging-time-slot)
      - [13.2.0.1. Context:](#13201-context)
      - [13.2.0.2. Discovery](#13202-discovery)
        - [13.2.0.2.1. Adam discovers nearby charging services](#132021-adam-discovers-nearby-charging-services)
      - [13.2.0.3. Order (Reservation)](#13203-order-reservation)
      - [13.2.0.4. Fulfilment (Session Start \& Tracking)](#13204-fulfilment-session-start--tracking)
      - [13.2.0.5. Post-Fulfilment](#13205-post-fulfilment)
    - [13.2.1. **API Calls and Schema**](#1321-api-calls-and-schema)
      - [13.2.1.1. `action: discover`](#13211-action-discover)
        - [13.2.1.1.1. Discovery of EV charging services within a circular boundary](#132111-discovery-of-ev-charging-services-within-a-circular-boundary)
        - [13.2.1.1.2. Discovery within circle + connector specs as filters](#132112-discovery-within-circle--connector-specs-as-filters)
        - [13.2.1.1.3. Discovery within circle + vehicle specifications as filters](#132113-discovery-within-circle--vehicle-specifications-as-filters)
        - [13.2.1.1.4. Discovery of services offered by a specific CPO](#132114-discovery-of-services-offered-by-a-specific-cpo)
        - [13.2.1.1.5. Viewing details of a single charging station (by its Item Identifier)](#132115-viewing-details-of-a-single-charging-station-by-its-item-identifier)
        - [13.2.1.1.6. Fetching details of a specific charger (EVSE) on-site (by its EVSE identifier)](#132116-fetching-details-of-a-specific-charger-evse-on-site-by-its-evse-identifier)
        - [13.2.1.1.7. Discovering chargers in a specific circular area, a specific connector type and availability time range](#132117-discovering-chargers-in-a-specific-circular-area-a-specific-connector-type-and-availability-time-range)
      - [13.2.1.2. `action: on_discover`](#13212-action-on_discover)
        - [13.2.1.2.1. Offers as part of the Catalog](#132121-offers-as-part-of-the-catalog)
      - [13.2.1.3. `action: select`](#13213-action-select)
      - [13.2.1.4. `action: on_select`](#13214-action-on_select)
        - [13.2.1.4.1. Surge Pricing](#132141-surge-pricing)
      - [13.2.1.5. `action: init`](#13215-action-init)
      - [13.2.1.6. `action: on_init`](#13216-action-on_init)
      - [13.2.1.7. `action: on_status` (payment)](#13217-action-on_status-payment)
      - [13.2.1.8. `action: confirm`](#13218-action-confirm)
      - [13.2.1.9. `action: on_confirm`](#13219-action-on_confirm)
      - [13.2.1.10. `action: update` (start charging)](#132110-action-update-start-charging)
      - [13.2.1.11. `action: on_update` (start charging)](#132111-action-on_update-start-charging)
      - [13.2.1.12. `action: status`](#132112-action-status)
      - [13.2.1.13. `action: on_status`](#132113-action-on_status)
      - [13.2.1.14. asynchronous `action: on_status` (temporary connection interruption)](#132114-asynchronous-action-on_status-temporary-connection-interruption)
          - [13.2.1.14.0.1. **A)Undercharge (Power Cut Mid-Session)**](#13211401-aundercharge-power-cut-mid-session)
          - [13.2.1.14.0.2. **B) Overcharge (Charger Offline to CMS; Keeps Dispensing)**](#13211402-b-overcharge-charger-offline-to-cms-keeps-dispensing)
      - [13.2.1.15. Asynchronous `action: on_update` (stop charging)](#132115-asynchronous-action-on_update-stop-charging)
      - [13.2.1.16. Synchronous/Asynchronous on\_update (stop charging)](#132116-synchronousasynchronous-on_update-stop-charging)
      - [13.2.1.17. `action: cancel`](#132117-action-cancel)
      - [13.2.1.18. `action: on_cancel`](#132118-action-on_cancel)
      - [13.2.1.19. `action: rating`](#132119-action-rating)
      - [13.2.1.20. `action: on_rating`](#132120-action-on_rating)
      - [13.2.1.21. `action: support`](#132121-action-support)
      - [13.2.1.22. `action: on_support`](#132122-action-on_support)
    - [13.2.2. **Integrating with your software**](#1322-integrating-with-your-software)
      - [13.2.2.1. **Integrating the BAP**](#13221-integrating-the-bap)
      - [13.2.2.2. **Integrating the BPP**](#13222-integrating-the-bpp)
  - [13.3. FAQs](#133-faqs)
  - [13.4. References](#134-references)

Table of contents and section auto-numbering was done using [Markdown-All-In-One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) vscode extension. Specifically `Markdown All in One: Create Table of Contents` and `Markdown All in One: Add/Update section numbers` commands accessible via vs code command pallete.

Example jsons were imported directly from source of truth elsewhere in this repo inline by inserting the pattern below within all json expand blocks, and running this [script](/scripts/embed_example_json.py), e.g. `python3 scripts/embed_example_json.py docs/implementation-guides/v2/EV_Charging/EV_Charging.md`.

```
<details><summary><a href="/path_to_file_from_root">txt_with_json_keyword</a></summary>

</details>
``` 


# 1. Request for Comments {#1-request-for-comments}

# 2. Copyright Notice {#2-copyright-notice}

**License: [CC-BY-NC-SA 4.0](https://becknprotocol.io/license/) becknprotocol.io**

# 3. Status of This Memo {#3-status-of-this-memo}

**This is a draft RFC for implementing EV charging use cases using the Beckn Protocol. It provides implementation guidance for anyone to build interoperable EV charging applications that integrate with each other on a decentralized network while maintaining compatibility with OCPI standards for CPO communication.**

## 4. Overview {#4-overview}

This document proposes a practical way to make EV charging services easier to find and use by applying the Beckn Protocol’s distributed commerce model. Instead of juggling multiple apps and accounts for different charging networks, EV users on any Beckn protocol-enabled consumer platform (a.k.a BAPs) – can discover and book charging services from Beckn protocol-enabled provider platforms (a.k.a BPPs) that have onboarded one or more Charge Point Operators (CPOs).

EV users can discover, compare options, view transparent pricing, and reserve a slot at any eligible charging station owned or operated by CPOs. By standardizing discovery, pricing, and booking, the approach tackles three persistent barriers to EV adoption: charging anxiety, network fragmentation, and payment complexity.

Built on Beckn’s commerce capabilities and aligned with OCPI for technical interoperability, the implementation lets e-Mobility Service Providers (eMSPs) aggregate services from multiple CPOs while delivering a consistent, app-agnostic experience to consumers. 

# 5. Introduction {#5-introduction}

This document provides an implementation guidance for deploying EV charging services using the Beckn Protocol ecosystem. It specifically addresses how consumer applications can provide unified access to charging infrastructure across multiple Charge Point Operators while maintaining technical compatibility with existing OCPI-based systems.

# 6. Scope {#6-scope}

This document covers:

* Architecture patterns for EV charging marketplace implementation using Beckn Protocol  
* Discovery and charging mechanisms for charging EVs across multiple CPOs  
* Some recommendations for BAPs, BPPs and NFOs on how to map protocol API calls to internal systems (or vice-versa).  
* Real-time availability and pricing integration with OCPI-based systems  
* Session management and billing coordination between Beckn and OCPI protocols

This document does NOT cover:

* Detailed OCPI protocol specifications (refer to OCPI 2.2.1 documentation)  
* Physical charging infrastructure requirements and standards  
* Regulatory compliance beyond technical implementation (varies by jurisdiction)  
* Smart grid integration and load management systems

# 7. Intended Audience {#7-intended-audience}

* Consumer Application Developers (BAPs): Building EV driver-facing charging applications with unified cross-network access  
* e-Mobility Service Providers (eMSPs/BPPs): Implementing charging service aggregation platforms across multiple CPO networks  
* Charge Point Operators (CPOs): Understanding integration requirements for Beckn-enabled marketplace participation  
* Technology Integrators: Building bridges between existing OCPI infrastructure and new Beckn-based marketplaces  
* System Architects: Designing scalable, interoperable EV charging ecosystems  
* Business Stakeholders: Understanding technical capabilities and implementation requirements for EV charging marketplace strategies  
* Standards Organizations: Evaluating interoperability approaches for future EV charging standards development

# 8. Conventions and Terminology {#8-conventions-and-terminology}

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described [here](https://github.com/beckn/protocol-specifications/blob/draft/docs/BECKN-010-Keyword-Definitions-for-Technical-Specifications.md).

# 9. Terminology {#9-terminology}

| Acronym | Full Form/Description             | Description                                                                                                           |
| ------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| BAP     | Beckn Application Platform        | Consumer-facing application that initiates transactions. Mapped to EV users and eMSPs.                                |
| BPP     | Beckn Provider Platform           | Service provider platform that responds to BAP requests. Mapped to CPOs.                                              |
| NFO     | Network Facilitator Organization  | Organization responsible for the adoption and growth of the network. Usually the custodian of the network’s registry. |
| CDS     | Catalog Discovery Service         | Enables discovery of charging services from BPPs in the network.                                                      |
| eMSP    | e-Mobility Service Provider       | Service provider that aggregates multiple CPOs. Generally onboarded by BAPs.                                          |
| CPO     | Charge Point Operator             | Entity that owns and operates charging infrastructure. Generally onboarded by BPPs.                                   |
| EVSE    | Electric Vehicle Supply Equipment | Individual charging station unit. Owned and operated by CPOs                                                          |
| OCPI    | Open Charge Point Interface       | Protocol for communication between eMSPs and CPOs.                                                                    |

> Note:
> This document does not detail the mapping between Beckn Protocol and OCPI. Please refer to [this](../../../../docs/implementation-guides/v1-EOS/DEG00x_Mapping-OCPI-and-Beckn-Protocol-for-EV-Charging-Interoperability.md) document for the same.
> BPPs are NOT aggregators. Any CPO that has implemented a Beckn Protocol endpoint is a BPP. 
> For all sense and purposes, CPOs are essentially BPPs and eMSPs are essentially BAPs.

# 10. Reference Architecture {#10-reference-architecture}

The section defines the reference ecosystem architecture that is used for building this implementation guide. 

## 10.1. Architecture Diagram {#101-architecture-diagram}

![](../assets/beckn-one-deg-arch.png)

## 10.2. Actors {#102-actors}

1. Beckn One Global Root Registry  
2. Beckn One Catalog Discovery Service  
3. Beckn Application Platforms  
4. Beckn Provider Platforms  
5. EV Charging Registry

# 11. Creating an Open Network for EV Charging {#11-creating-an-open-network-for-ev-charging}

To create an open network for EV charging requires all the EV charging BAPs, BPPs, to be able to discover each other and become part of a common club. This club is manifested in the form of a Registry maintained by an NFO. 

## 11.1. Setting up a Registry {#111-setting-up-a-registry}

The NP Registry serves as the root of addressability and trust for all network participants. It maintains comprehensive details such as the participant’s globally unique identifier (ID), network address (Beckn API URL), public key, operational domains, and assigned role (e.g., BAP, BPP, CDS). In addition to managing participant registration, authentication, authorization, and permission control, the Registry oversees participant verification, activation, and overall lifecycle management, ensuring that only validated and authorized entities can operate within the network.

![](../assets/registry-arch.png)

You can publish your registries at [DeDi.global](https://publish.dedi.global/).

### 11.1.1. For a Network Participant {#1111-for-a-network-participant}

#### 11.1.1.1. Step 1 :  Claiming a Namespace {#11111-step-1---claiming-a-namespace}

To get started, any platform that has implemented Beckn Protocol MUST create a globally unique namespace for themselves.   
All NPs (BAPs, BPPs, CDS’es) **MUST** register as a user on dedi.global and claim a unique namespace against their FQDN to become globally addressable. As part of the claiming process, the user must prove ownership of the namespace by verifying the ownership of their domain. Namespace would be at an organisation level. You can put your organisation name as the name of the namespace.

#### 11.1.1.2. Step 2 :  Setting up a Registry {#11112-step-2---setting-up-a-registry}

Once the namespace is claimed, each NP **MUST** create a Beckn NP registry in the namespace to list their subscriber details. While creating the registry, the user **MUST** configure it with the [subscriber schema](https://gist.githubusercontent.com/nirmalnr/a6e5b17522169ecea4f3ccdd831af7e4/raw/7744f2542034db9675901b61b41c8228ea239074/beckn-subscriber-no-refs.schema.json). Example of a registry name can be `subscription-details`.

#### 11.1.1.3. Step 3 :  Publishing subscriber details {#11113-step-3---publishing-subscriber-details}

In the registry that is created, NPs **MUST** publish their subscription details including their ID, network endpoints, public keys, operational domains and assigned roles (BAP, BPP) as records.

*Detailed steps to create namespaces and registries in dedi.global can be found [here](https://github.com/dedi-global/docs/blob/0976607aabc6641d330a3d41a3bd89ab8790ea09/user-guides/namespace%20and%20registry%20creation.md).*

### 11.1.2. Step 4 :  Share details of the registry created with the Beckn One team {#1112-step-4---share-details-of-the-registry-created-with-the-beckn-one-team}

Once the registry is created and details are published, the namespace and the registry name of the newly created registry should be shared with the beckn one team.

### 11.1.3. For a Network facilitator organization {#1113-for-a-network-facilitator-organization}

#### 11.1.3.1. Step 1 :  Claiming a Namespace {#11131-step-1---claiming-a-namespace}

An NFO **MAY** register as a user on dedi.global and claim a unique namespace against their FQDN. As part of the claiming process, the user must prove ownership of that namespace by verifying the ownership of that domain. The NFO name can be set as the name of the namespace. 
*Note: A calibrated roll out of this infrastructure is planned and hence before it is open to the general public NFOs are advised to share their own domain and the domains of their NPs to the Beckn One team so that they can be whitelisted which will allow the NPs to verify the same using TXT records in their DNS.*

#### 11.1.3.2. Step 2 :  Setting up a Registry {#11132-step-2---setting-up-a-registry}

Network facilitators **MAY** create registries under their own namespace using the [subscriber reference schema](https://gist.githubusercontent.com/nirmalnr/a6e5b17522169ecea4f3ccdd831af7e4/raw/b7cf8a47e6531ef22744b43e6305b8d8cc106e7b/beckn-subscriber-reference.schema.json) to point to either whole registries or records created by the NPs in their own namespaces.  Example of a registry name can be `subscription-details`.

#### 11.1.3.3. Step 3 :  Publishing subscriber details {#11133-step-3---publishing-subscriber-details}

In the registry that is created, NFOs **MAY** publish records which act as pointers to either whole registries or records created by the NPs records. The URL field in the record would be the lookup URL for a registry or a record as per DeDi protocol.

Example: For referencing another registry created by an NP, the record details created would be:

```json
{
  "url": "https://.dedi.global/dedi/lookup/example-company/subscription-details",
  "type": "Registry",
  "subscriber_id": "example-company.com"
}
```

Here `example-company` is the namespace of the NP, and all records added in the registry is referenced here. 

If only one record in the registry needs to be referenced, then the record details created would be:

```json
{
  "url": "https://.dedi.global/dedi/lookup/example-company/subscription-details/energy-bap",
  "type": "Record",
  "subscriber_id": "example-company.com"
}
```

Here `energy-bap` is the name of the record created by the NP in this registry. Only that record is referenced here.

*Detailed steps to create namespaces and registries in dedi.global can be found [here](https://github.com/dedi-global/docs/blob/0976607aabc6641d330a3d41a3bd89ab8790ea09/user-guides/namespace%20and%20registry%20creation.md).*

#### 11.1.3.4. Step 4 :  Share details of the registry created with the Beckn One team {#11134-step-4---share-details-of-the-registry-created-with-the-beckn-one-team}

Once the registry is created and details are published, the namespace and the registry name of the newly created registry should be shared with the beckn one team.

## 11.2. Setting up the Protocol Endpoints {#112-setting-up-the-protocol-endpoints}

This section contains instructions to set up and test the protocol stack for EV charging transactions. 

### 11.2.1. Installing Beckn ONIX

All NPs SHOULD install the Beckn ONIX adapter to quickly get set up and become Beckn Protocol compliant. Click [here](https://github.com/Beckn-One/beckn-onix?tab=readme-ov-file#automated-setup-recommended)) to learn how to set up Beckn ONIX.

### 11.2.2. Configuring Beckn ONIX for EV Charging Transactions

A detailed Configuration Guide is available [here](https://github.com/Beckn-One/beckn-onix/blob/main/CONFIG.md). A quick read of key concepts from the link is recommended.

Specifically, for EV Charging, please use the following configuration:
1. Configure dediregistry plugin instead of registry plugin. Read more [here](https://github.com/Beckn-One/beckn-onix/tree/main/pkg/plugin/implementation/dediregistry).
2. Start with using Simplekeymanager plugin during development, read more [here](https://github.com/Beckn-One/beckn-onix/tree/main/pkg/plugin/implementation/simplekeymanager). For production deployment, you may setup vault.
3. For routing calls to Catalog Discovery Service, refer to routing configuration [here](https://github.com/Beckn-One/beckn-onix/blob/main/config/local-simple-routing-BAPCaller.yaml).

### 11.2.3. 10.2.3 Performing a test EV charging transaction

Step 1 : Download the postman collection, from [here](/testnet/postman-collections/v2/EV_Charging/).

Step 2 : Run API calls

If you are a BAP

1. Configure the collection/environment variables to the newly installed Beckn ONIX adapter URL and other variables in the collection.
2. Select the discover example and hit send
3. You should see the EV charging service catalog response

If you are a BPP

1. Configure the collection/environment variables to the newly installed Beckn ONIX adapter URL and other variables in the collection.
2. Select the on_status example and hit send
3. You should see the response in your console

# 12. Implementing EV Charging Semantics on Beckn Protocol {#12-implementing-ev-charging-semantics-on-beckn-protocol}

This section contains recommendations and guidelines on how to implement EV Charging Services on Beckn Protocol enabled networks. To ensure global interoperability between actors of the EV charging network, the semantics of the EV charging industry need to be mapped to the core schema of Beckn Protocol. The below table summarizes key semantic mappings between the EV Charging Domain and Beckn Protocol domain.

## 12.1. Key Assumptions {#121-key-assumptions}

- **Assumption 1 :** EV charging is treated as a service, not as a physical object.  
- **Assumption 2:** All CPOs have implemented OCPI interfaces 

Each entity in the charging lifecycle — the service, the commercial terms, and the usage instance — maps to a well-defined semantic concept, enabling platforms to exchange information in a standardized, machine-readable way.

## 12.2. Semantic Model {#122-semantic-model}

| EV Charging Domain Entity | Charging Example                                                                       |                                                    Semantically maps to                                                     |
| ------------------------- | -------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------: |
| Charging Service Listing  | “DC Fast-Charging (60 kW CCS2)”                                                        | [Item](https://github.com/beckn/protocol-specifications-new/tree/main/schema/EvChargingService/v1/attributes.yaml)  |
| Charging service          | “₹18 per kWh”, “₹150 per hour”, “₹999 monthly pass”, “Off-peak discount 2 AM–5 AM”     |  [Offer](https://github.com/beckn/protocol-specifications-new/tree/main/schema/EvChargingOffer/v1/attributes.yaml)  |
| Charging Session          | A specific booking or usage instance created when the user plugs in or reserves a slot | [Order](https://github.com/beckn/protocol-specifications-new/tree/main/schema/EvChargingSession/v1/attributes.yaml) |

## 12.3. Example Category Codes {#123-example-category-codes}

The following section contains example category codes that can be 

Charger types

| Code      | What it means                                                                              |
| --------- | ------------------------------------------------------------------------------------------ |
| AC\_SLOW  | AC charge points in the “slow” band (≈ 3–7 kW).                                            |
| AC\_FAST  | AC “fast” public charging (≈ 7–22 kW; UK fast band 8–49 kW covers AC up to \~22 kW).       |
| DC\_FAST  | DC “rapid/fast” typically \~50–149 kW.                                                     |
| DC\_ULTRA | DC “ultra-rapid/ultra-fast”, ≥ 150 kW (also aligns with AFIR focus on ≥ 150 kW corridors). |

Connector types

| Code    | What it means                                                                           |
| ------- | --------------------------------------------------------------------------------------- |
| TYPE1   | Type 1 (SAE J1772) AC connector (North America/JP usage).                               |
| TYPE2   | Type 2 (IEC 62196-2) AC connector; EU standard.                                         |
| CCS2    | Combined Charging System “Combo 2” (IEC 62196 based) enabling high-power DC on Type 2\. |
| CHADEMO | CHAdeMO DC fast-charging standard.                                                      |
| GB\_T   | China’s GB/T charging standard (AC & DC).                                               |

Service types

| Code                     | What it means                                                                                                                  |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| GREEN\_ENERGY\_CERTIFIED | Flag that the site/session is supplied by verified green energy per catalog metadata (e.g., OCPI EnergyMix.is\_green\_energy). |
| GO\_EU                   | Energy backed by EU Guarantees of Origin (GOs).                                                                                |
| REGO\_UK                 | Energy backed by the UK Renewable Energy Guarantees of Origin (REGO) scheme.                                                   |
| I\_REC                   | Energy backed by I-REC certificates (international EACs).                                                                      |
| V2G\_ENABLED             | Charger/site supports bidirectional power transfer (V2G), e.g., per ISO 15118-20 implementations.                              |
| REMOTE\_START\_STOP      | Remote start/stop of sessions exposed via roaming interface (OCPI Commands module).                                            |


# 13. Example Workflows (EV User’s Perspective) {#13-example-workflows-ev-users-perspective}

