# Setting Up Your First TAXII Connection

## Overview

This chapter walks you through connecting Microsoft Sentinel to a TAXII 2.x server to import threat indicators. We'll use a free threat intelligence feed (Pulsedive) as our example, giving you hands-on experience without requiring a commercial subscription.

## Prerequisites
- Microsoft Sentinel Contributor role at resource group level
- Threat Intelligence solution installed from Content Hub
- TAXII server credentials (API root, collection ID, username/password)

> [!NOTE]  
> If you don't have TAXII credentials yet, you can use free feeds from Pulsedive (https://pulsedive.com) or other
> OSINT providers. Register for a free account to get API credentials.

## Step 1: Install the Threat Intelligence Solution

The Threat Intelligence solution contains all necessary connectors and analytics rules.

### In Azure Portal:
1. Navigate to your Microsoft Sentinel workspace.
2. Select Content management > Content hub.
3. Search for 'Threat Intelligence'.
4. Select the Threat Intelligence solution.
5. Click Install/Update.

### In Defender Portal:

1. Navigate to Microsoft Sentinel > Content management > Content hub.
2. Search for 'Threat Intelligence'.
3. Select the solution and click Install/Update.

> [!NOTE]
> The solution installation typically takes 2-3 minutes. You'll see a notification when it completes.
