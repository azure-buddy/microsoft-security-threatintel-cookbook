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

## Step 2: Locate TAXII Server Information
Before configuring the connector, gather the following information from your threat intelligence provider:

-  API Root URL: The base endpoint hosting collections
-  Collection ID: Unique identifier for the threat intelligence collection
-  Username: Authentication credential (if required)
-  Password: Authentication credential (if required)

### Example: Pulsedive TAXII Server
API Root: https://pulsedive.com/api/explore/threat/taxii/ Collection ID: indicators Username: [Your Pulsedive API Key] Password: [Leave blank or use API key]

### Using cURL to Discover API Roots
Some providers only advertise a discovery endpoint. You can use cURL to find the API root:
```
curl -X GET https://taxii.provider.com/taxii2/ \   -H "Accept: application/taxii+json;version=2.1"  # Response will include API roots: {   "api_roots": [     "https://taxii.provider.com/api/v21/"   ] }
```

## Step 3: Configure the TAXII Data Connector

### In Azure Portal:
1. Navigate to Microsoft Sentinel > Configuration > Data connectors
2. Search for 'Threat Intelligence - TAXII'
3. Select the connector and click 'Open connector page'
4. Fill in the configuration form (see below)
5. Click 'Add'

### In Defender Portal:
1. Navigate to Microsoft Sentinel > Content management > Content hub
2. Select 'Data connectors' from the filters
3. Find 'Threat Intelligence - TAXII'
4. Click 'Open connector page'
5. Fill in the configuration form and click 'Add'

### Configuration Form Fields

| **Field**             | **Description**                                                                  |
| --------------------- | -------------------------------------------------------------------------------- |
| **Friendly Name**     | Descriptive name for this connection (e.g., 'Pulsedive Threat Feed')             |
| **API Root URL**      | Full TAXII API root endpoint (must include protocol: https://)                   |
| **Collection ID**     | UUID or string identifying the threat intelligence collection                    |
| **Username**          | Authentication username (leave blank if not required)                            |
| **Password**          | Authentication password or API key                                               |
| **Import Indicators** | Select indicator types to import (All, IP, URL, Domain, Hash, Email)             |
| **Polling Frequency** | How often to poll for new indicators (Once an hour, Once a day, Once per minute) |

> [!Important]
> Choose polling frequency carefully. More frequent polling increases costs and may hit provider rate limits. For most feeds, 'Once an hour' is sufficient.
