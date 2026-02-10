# Introduction

Welcome to the Microsoft Security Threat Intelligence Cookbook for using STIX-TAXII in Microsoft Security products like *Microsoft Defender* and *Microsoft Sentinel*. This comprehensive guide is designed for security professionals who want to master those Threat Intelligence integrations available in *Unified Security Operations*.

## About This Cookbook

This cookbook provides practical, action-oriented solutions for implementing and managing STIX (Structured Threat Information eXpression) and TAXII (Trusted Automated eXchange of Intelligence Information) services in Microsoft Sentinel. Each chapter contains step-by-step instructions, code snippets, and real-world examples to help you quickly implement features without wading through extensive documentation.

## Who Should Use This Cookbook

- Security Operations Center (SOC) Analysts
- Threat Intelligence Analysts
- Security Engineers and Architects
- Microsoft Sentinel Administrators
- Security Automation Developers

## What You'll Learn

By the end of this cookbook, you will be able to:

1. Configure and manage STIX/TAXII data connectors in Microsoft Sentinel
2. Integrate commercial and open-source threat intelligence feeds
3. Work with the new `ThreatIntelIndicators` and `ThreatIntelObjects` tables
4. Build custom integrations using the Threat Intelligence Upload API
5. Create automated workflows for threat intelligence curation
6. Query and analyze STIX objects for threat hunting
7. Export threat intelligence to external platforms
8. Leverage the Microsoft Unified Security Operations portal for TI management

## Important Context: Microsoft's Unified Security Operations Platform

As of 2024, Microsoft has unified its security operations capabilities in the Microsoft Defender portal. This unified platform combines Microsoft Sentinel, Microsoft Defender XDR, Microsoft Security Exposure Management, and Microsoft Security Copilot into a single, integrated experience.

> [!WARNING]  
> Starting in July 2025, many new customers are automatically onboarded to the Defender portal. After March 31, 2027, Microsoft Sentinel will only be available in the Microsoft Defender portal. All examples in this cookbook work in both the Azure portal and the unified Defender portal.

Throughout this cookbook, we'll reference both environments where relevant, ensuring you can follow along regardless of which portal you're using.

# Prerequisites

To get the most out of this cookbook, you should have:

- Access to a Microsoft Sentinel workspace
- Microsoft Sentinel Contributor role at the resource group level
- Basic understanding of threat intelligence concepts
- Familiarity with KQL (Kusto Query Language)
- Understanding of REST APIs (for advanced chapters)

How to Use This Cookbook

Each chapter follows a consistent structure:

1. Overview: Context and key concepts
2.	Prerequisites: What you need before starting
3.	Step-by-Step Instructions: Detailed, actionable guidance
4.	Code Snippets: Ready-to-use examples
5.	Troubleshooting: Common issues and solutions
6.	References: Links to official documentation
7.	Advanced Topics: Optional challenges for deeper learning

You can work through chapters sequentially or jump to specific topics based on your needs. Let's get started!
