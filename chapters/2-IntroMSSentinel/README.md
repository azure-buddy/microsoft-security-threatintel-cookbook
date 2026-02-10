# Understanding STIX and TAXII in Microsoft Sentinel

## Overview

Before diving into implementation, it's essential to understand the foundations of STIX and TAXII, and how they integrate with Microsoft Sentinel. This chapter provides the conceptual framework you'll need for all subsequent chapters.

# What is STIX?
STIX (Structured Threat Information eXpression) is a standardized language for describing cybersecurity threat information. Think of it as a common vocabulary that allows security teams to share threat intelligence in a consistent, machine-readable format.

# STIX 2.1 Objects in Microsoft Sentinel

> [!NOTE]  
> As of April 2025, Microsoft Sentinel supports the following STIX 2.1 objects:
> - Indicators: Observable patterns that indicate malicious activity (IPs, domains, file hashes)
> - Threat Actors: Individuals or groups conducting malicious activities
> - Attack Patterns: Methods and techniques used by adversaries (aligned with MITRE ATT&CK)
> - Identities: Organizations, individuals, or systems targeted by threats
> - Relationships: Connections between STIX objects (e.g., 'Threat Actor uses Attack Pattern')

> [!IMPORTANT]  
> Microsoft introduced two new tables in April 2025: ThreatIntelIndicators and ThreatIntelObjects. The legacy ThreatIntelligenceIndicator table will be deprecated on July 31, 2025. All custom queries, analytics rules, and workbooks must be migrated to the new tables by this date.

## What is TAXII?

TAXII (Trusted Automated eXchange of Intelligence Information) is a protocol for exchanging cyber threat intelligence. If STIX is the language, TAXII is the transport mechanism that delivers it.

## TAXII 2.x Architecture

TAXII 2.x uses a client-server model with these key components:

- API Root: The base URL that hosts threat intelligence collections
- Collections: Groups of threat intelligence objects (like folders)
- Discovery Endpoint: URL that advertises available API Endpoints:
  - API Root: https://taxii.vendor.com/api/v21/
  - Collection: https://taxii.vendor.com/api/v21/collections/malware/ 
