# Setting Up Your First TAXII Connection

## Overview

This chapter walks you through connecting Microsoft Sentinel to a TAXII 2.x server to import threat indicators. We'll use a free threat intelligence feed (Pulsedive) as our example, giving you hands-on experience without requiring a commercial subscription.

## Prerequisites
- Microsoft Sentinel Contributor role at resource group level
- Threat Intelligence solution installed from Content Hub
- TAXII server credentials (API root, collection ID, username/password)

> [!NOTE]  
> If you don't have TAXII credentials yet, you can use free feeds from Pulsedive (https://pulsedive.com) or other
> OSINT providers. Learn more about the free [Pulsedrive Feeds here.](https://pulsedive.com/about/feed) and don't forget to register for a free account to get API
> credentials.

## Step 1: Install the Threat Intelligence Solution

The Threat Intelligence solution contains all necessary connectors and analytics rules.

### In Azure Portal:
1. Navigate to your Microsoft Sentinel workspace.
2. Select Content management > Content hub.
3. Search for 'Threat Intelligence'.
4. Select the Threat Intelligence (NEW) solution.
5. Click Install/Update.

### In Defender Portal:
1. Navigate to Microsoft Sentinel > Content management > Content hub.
2. Search for 'Threat Intelligence'.
3. Select the Threat Intelligence (NEW) solution.
4. Click Install/Update.

> [!NOTE]
> The solution installation typically takes 2-3 minutes. You'll see a notification when it completes.

## Step 2: Locate TAXII Server Information
Before configuring the connector, gather the following information from your threat intelligence provider:

-  API Root URL: The base endpoint hosting collections
-  Collection ID: Unique identifier for the threat intelligence collection
-  Username: Authentication credential (if required)
-  Password: Authentication credential (if required)

### Example looking up available API-root and Collections using Browser
This example below shows how to lookup the available API Roots. Just copy/paste this into your favorite browser and don't forget to include your key.

```
https://pulsedive.com/taxii2/?accept=application%2Ftaxii%2Bjson%3Bversion%3D2.1&pretty=1&key=xxxyyyzzz
```

Now that you known the API Root you can lookup the test collection.

```
https://pulsedive.com/taxii2/api/collections/?accept=application%2Ftaxii%2Bjson%3Bversion%3D2.1&pretty=1&key=xxxyyyzzz
```


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
| **Password**          | Authentication password or in case of Pulsedrive you may provide the API key     |
| **Import Indicators** | Select indicator types to import (All, IP, URL, Domain, Hash, Email)             |
| **Polling Frequency** | How often to poll for new indicators (Once an hour, Once a day, Once per minute) |


> [!Important]
> Choose polling frequency carefully. More frequent polling increases costs and may hit provider rate limits. For most feeds, 'Once an hour' is sufficient.

## Step 4: Verify Connection

After clicking 'Add', you should see a confirmation message: 'Connection to TAXII server established successfully.' Within a few minutes, indicators should begin appearing in your workspace.

### Check Indicator Import Status

Run this KQL query in Log Analytics or Advanced Hunting:

```
ThreatIntelligenceIndicator | where SourceSystem contains "Pulsedive"  // Replace with your source name | summarize Count=count() by bin(TimeGenerated, 1h) | order by TimeGenerated desc | take 24
```

This shows the number of indicators imported per hour over the last 24 hours.

### View Imported Indicators

To see the actual indicators:

```
ThreatIntelligenceIndicator | where SourceSystem contains "Pulsedive" | extend IndicatorValue = coalesce(     NetworkIP,     DomainName,     Url,     FileHashValue ) | project      TimeGenerated,     IndicatorValue,     ThreatType,     Confidence,     Description | take 100
```

## Step 5: Connect Additional Feeds

You can connect multiple TAXII feeds to the same workspace. Simply repeat Step 3 for each feed, using a unique friendly name for each connection.

### Recommended Free Feeds for Testing

- Pulsedive: Community-driven threat intelligence
- Microsoft Defender Threat Intelligence: Free Microsoft TI feed
- AlienVault OTX: Open Threat Exchange community feed

## Troubleshooting Common Issues

### Issue: Connection Fails with '401 Unauthorized'

Solution: Verify your username/password or API key. Some providers require the API key in the username field with a blank password.

### Issue: Connection Succeeds but No Indicators Appear

Potential causes:
- Collection is empty or doesn't contain compatible indicators
- Polling hasn't occurred yet (wait for the next scheduled poll)
- Indicator group filter excludes all indicators
- Verify with: `ThreatIntelligenceIndicator | where SourceSystem contains "[YourSourceName]" | take 1`

### Issue: 'Invalid API Root' Error

Solution: Ensure the API root URL:
- Includes the protocol (https://)
- Does NOT include the collection ID (that goes in a separate field)
- Ends with a trailing slash if required by the provider

### Issue: Need to Allowlist Microsoft Sentinel IP

Some TAXII servers (like FS-ISAC) require allowlisting the Microsoft Sentinel TAXII client IP addresses. Contact your TAXII provider for their allowlisting process. Microsoft's documentation doesn't publish these IPs publicly, so you'll need to work with Microsoft Support if required.

## Advanced: Configuring Ingestion Rules

Ingestion rules help reduce noise by filtering indicators before they're stored in your workspace. This is covered in detail in Chapter 5.

## References

| **Resource**                      | **Link**                                                                                                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Connect to STIX/TAXII Feeds**   | [https://learn.microsoft.com/en-us/azure/sentinel/connect-threat-intelligence-taxii](https://learn.microsoft.com/en-us/azure/sentinel/connect-threat-intelligence-taxii) |
| **Threat Intelligence Solution**  | [https://learn.microsoft.com/en-us/azure/sentinel/understand-threat-intelligence](https://learn.microsoft.com/en-us/azure/sentinel/understand-threat-intelligence)       |
| **Pulsedive TAXII Documentation** | [https://docs.pulsedive.com/taxii](https://pulsedive.com/api/)                                                                                                                 |

## Challenge

Advanced topic: Set up multiple TAXII connections to compare different threat feeds. Create a KQL query that shows which feed provides the most unique indicators versus overlapping indicators across feeds.

```
// Hint: Use summarize with dcount() and make_set() to identify unique vs. shared indicators


```
