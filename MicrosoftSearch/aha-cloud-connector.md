--- 
title: "Aha! Microsoft Graph connector" 
ms.author: raynezou
author: leizi2015
manager: zezhangzhao
audience: Admin
ms.audience: Admin 
ms.topic: article 
ms.service: mssearch 
ms.localizationpriority: Medium 
search.appverid: 
- BFB160 
- MET150 
- MOE150 
description: "Set up the Aha! Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot" 
ms.date: 03/27/2025
---

# Aha! Microsoft Graph connector (preview)

The Aha! Microsoft Graph connector empowers your organization to index and search Aha! features across your enterprise. Once configured, the connector automatically crawls Aha! features, making them easily discoverable through Microsoft Copilot and any Microsoft Search client.

This article is for Microsoft 365 administrators or anyone who configures, runs, and monitors an Aha! Microsoft Graph connector.

>[!NOTE]
>The Aha! Microsoft Graph connector is in public preview. If you wish to get access to try it, enable [Targeted release](/microsoft-365/admin/manage/release-options-in-office-365#set-up-the-release-option-in-the-admin-center) ring for your Admin account.

## Capabilities
- Index features from your Aha! Workspace.
- Customize your crawl frequency.  
- Create workflows using this connection and plugins from Microsoft Copilot Studio.  
- Use [Semantic search in Copilot](semantic-index-for-copilot.md) to enable users to find relevant content.

## Limitations
- The connector doesn't index comments.
- The connector doesn't index customized fields.

## Get Started

### 1. Display name 
A display name is used to identify each citation in Copilot, helping users easily recognize the associated file or item. Display name also signifies trusted content. Display name is also used as a [content source filter](/MicrosoftSearch/custom-filters#Content-source-filters). A default value is present for this field, but you can customize it to a name that users in your organization recognize.

### 2. Instant URL
An Aha! instance URL is the unique web address assigned to each Aha! instance, allowing you to access your specific Aha! environment. The URL follows the format, https://contoso.aha.io. 

### 3. Authentication Type
To use Aha! OAuth for authentication, an Aha! admin needs to create an Aha! OAuth2.0 app in the [Aha! developer console](https://secure.aha.io/session/new).

**Create Aha! OAuth 2.0 application**
To properly register the Aha! OAuth application for GCS access, first navigate to **Personal settings**, choose **Developer**, and click **OAuth applications**. Click "Create" to generate the **Client ID** and **Secret** after entering the redirect URI.
- For M365 Enterprise, copy and paste: `https://gcs.office.com/v1.0/admin/oauth/callback`.
- For M365 Government, copy and paste: `https://gcsgcc.office.com/v1.0/admin/oauth/callback`.

Copy the **Client ID** and **Secret** from the OAuth tab in the Aha! app and paste it in the connector setup. Choose Authorize, and use the same Aha! admin account credential to authenticate permission to crawl. For more information, see [Aha! OAuth2 Authentication](https://www.aha.io/api/oauth2#registering-an-application).


### 4. Roll out to limited audience
Deploy this connection to a limited user base if you want to validate it in Copilot and other Search surfaces before expanding the rollout to a broader audience. For more information, see [Staged rollout for Microsoft Graph connectors](staged-rollout-for-graph-connectors.md).

At this point, you're ready to create the connection for Aha!. Click "Create" to publish your connection and index features from your Aha! account.

For other settings, like **Access permissions**, **Schema**, and **Crawl frequency**, we have default values based on what works best with Aha! data.


| Users | Description |
|----|---|
| Access permissions | Only users with access to content in Data source. |
| Map identities | Data source identities mapped using Microsoft Entra IDs. |

| Content | Description |
|---|---|
| Manage properties | For information about the default properties and their schema, see [content](#content). |

| Sync | Description |
|---|---|
| Incremental crawl | Runs every 15 minutes. |
| Full crawl | Runs every day. |

If you want to edit any of these values, choose the **Custom setup**.

## Custom setup

Custom setup is for those admins who want to edit the default values for settings listed in the [Roll out to limited audience](#4-roll-out-to-limited-audience) section. Once you click **Custom setup**, you see **Users**, **Content**, and **Sync**.

### Users

**Access permissions**
The Aha! Microsoft Graph connector allows your organization to index features from Aha! After you configure the connector, end users can search for these features from Aha! in Microsoft 365 Copilot and from any Microsoft Search client.

### Content

**Manage properties**

Add or remove available properties from your Aha!, assign a schema to the property (define whether a property is searchable, queryable, retrievable, or refinable), change the semantic label, and add an alias to the property. These are the properties that are selected by default.

|Source property|Label|Description|Schema|
|---|---|---|---|
| AssignToUser       |                      |        | Search                        |
| AssignToUserEmail  |                      |        | Search                        |
| CreateTime         | Created date time    | Date and time that the item was created in the data source       | Query, Retrieve                  |
| CreatedBy          | Created by           |        | Query, Retrieve, Search         |
| Description        |     Content |  The description of the issue      | search                        |
| DueDate            |                      |        |  Query                 |
| EpicName           |                      |        |  Retrieve, Search  |
| Id                 |                      |        |  Query, Retrieve                       |
| Link               |                      |        |   Query, Retrieve, Search                       |
| Name               | Title                | 	The title of the item that you want shown in Copilot and other search experiences| Query, Retrieve, Search |
| Reference          |                      |        | Query         |
| ReleaseName        |                      |        |         Search                |
| Score        |                      |        |         Query, Search                |
| StartDate          |                      |        |       Query                |
| Status             |                      |        |   Query, Refine, Search                      |
| Tag                |                      |        |          Query, Retrieve, Search                |
| UpdateTime         | Last modified date time |Date and time the item was last modified in the data source.|      Query, Retrieve                   |
| Url         | url |The target URL of the item in the data source|    Retrieve, Search                      |

**Preview data**

Go to "Results" to verify the sample values of the selected properties and query filter.

### Sync

The refresh interval determines how often your data is synced between the data source and the Aha! Microsoft Graph connector index. There are two types of refresh intervals - full crawl and incremental crawl. For more information, see [Refresh settings](configure-connector.md#guidelines-for-sync-settings).

You can change the default refresh interval here if needed.
If you have issues or want to provide feedback, contact [Microsoft Graph support](https://developer.microsoft.com/graph/support).
