--- 

title: "Adobe Experience Manager(AEM) Assets Microsoft 365 Copilot connector (preview)" 
ms.author: rantang
author: ranran1998
manager: jecui
audience: Admin
ms.audience: Admin 
ms.topic: article 
ms.service: mssearch 
ms.localizationpriority: Medium 
search.appverid: 
- BFB160 
- MET150 
- MOE150 
description: "Set up the Adobe Experience Manager Assets Microsoft 365 Copilot connector." 
ms.date: 03/14/2025
---

# Adobe Experience Manager Assets Microsoft 365 Copilot connector (preview)

With the Adobe Experience Manager Assets Microsoft 365 Copilot connector, your organization can index published assets of your AEM Assets instance. After you configure the connector and index content from AEM Assets, end users can search for those published assets in Microsoft Copilot and from any Microsoft Search client. 

This article is for Microsoft 365 administrators or anyone who configures, runs, and monitors an Adobe Experience Manager Assets Copilot connector. 

## Capabilities
- Index the published assets of your Adobe Experience Manager Assets.
- Support ingestion filters based on page paths, allowing for exact matching and phrase matching using regular expressions.
- Customize your crawl frequency.
- Create workflows using this connection and plugins from Microsoft Copilot Studio.  
- Use [Semantic search in Copilot](semantic-index-for-copilot.md) to enable users to find relevant content.

## Limitations
- Does not index comments.
- Does not crawl user identities and access permissions. All published assets indexed using the Adobe Experience Manager Assets Copilot connector are visible to all Microsoft 365 users in your tenant, from Microsoft Search or Copilot.   

## Prerequisites
- You must be the **search admin** for your organization's Microsoft 365 tenant.
- To connect to your Adobe Experience Manager Assets data, you need your organization's Adobe Experience Cloud instance author environment URL and publish environment URL.
  Your organization's Adobe Experience Cloud instance author environment URL typically looks like: `https://author-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.<REGION>.adobeaemcloud.com`.
  Your organization's Adobe Experience Cloud instance publish environment URL typically looks like: `https://publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.<REGION>.adobeaemcloud.com`. 
- To connect to Adobe Experience Cloud and allow the Adobe Experience Manager Assets Copilot connector to update published assets and metadata regularly, you need a technical account of your Adobe Experience Manager Assets with the credentials to access published assets and metadata. The technical account is the secure, service-based account for external access to Adobe Experience Manager Assets. Find more details [here](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis#generate-a-jwt-token-and-exchange-it-for-an-access-token).

## Get started

### Choose display name 
A display name is used to identify each citation in Copilot, helping users easily recognize the associated file or item. Display name also signifies trusted content. The display name is also used as a [content source filter](/MicrosoftSearch/custom-filters#content-source-filters). A default value is present for this field, but you can customize it to a name that users in your organization recognize.

### Add Adobe Experience Cloud Instance URL
To correctly access and update data from the Adobe Experience Manager Assets, both the author and publish environment URLs are essential.   

### Provide authentication Type
Authentication Type - We support the technical account for Adobe Experience Cloud. To enable and configure the technical account for Adobe Experience Manager Assets. For more information, see [Adobe Experience Cloud documentation](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis#generate-a-jwt-token-and-exchange-it-for-an-access-token).

### Staged rollout to a limited audience
Deploy this connection to a limited user base if you want to validate it in Copilot and other Search surfaces before expanding the rollout to a broader audience.

To create the connection for AEM Assets, click **Create** to publish your connection and index published web assets from your AEM Assets. 

For other settings, like Access Permissions, Data inclusion rules, Schema, Crawl frequency, etc., we set defaults based on what works best with AEM Assets data. You can see the default values below: 

|Page|Settings|Default values|
|--- | ---- | ---|
|Users | Access permissions | All published assets or posts indexed using the Adobe Experience Manager Assets Copilot connector are visible to all M365 users in your tenant, from Microsoft Search or Copilot.|
|Content | Index content | All published assets are selected by default. |
|Content | Manage properties | To check default properties and their schema, [click here](#content).|
|Sync | Incremental crawl | Frequency: Every 15 mins|
|Sync | Full crawl | Frequency: Every day|


## Custom setup 

In custom setup,  you can edit any of the default values for users, content, and sync.

### Users 

#### Access permissions

Currently, only published assets from your AEM Assets are indexed. All data indexed using the Adobe Experience Manager Assets Copilot tor is visible to all Microsoft 365 users in your tenant, from Microsoft Search or Copilot.

### Content 

#### Content ingestion filters   
You can choose to include or exclude certain content paths.  

- Content paths that should be fetched: Only support input exact paths. A valid content path must have at least three levels, starting with "/content/dam" as the first two segments. 
- Content paths that should not be fetched: Only support input Java regular expression for paths. The priority of excluding content paths is higher than that of including content paths. 
Use the preview results button to verify the sample values of the selected properties and filters. 

#### Manage properties

To check available properties from your Adobe Experience Manager Assets, assign a schema to the property (define whether a property is searchable, queryable, retrievable, or refinable), change the semantic label, and add an alias to the property. The following properties are indexed by default.

|Default property|Semantic label|Description|Schema|
|----------------------|--------------------------|---------------------------------------------------------------------------------|-----------------------------|
| AltText          |             | the "Alt Text" field in AEM Assets metadata                     | Query, Retrieve, Search.     |
| AssetType         |             | The file type of the asset (e.g., Image, Multimedia, Document, Archive etc.).                    |Retrieve.  |
| Content         | Content                 | The content of documents, not available for images             | Search.                      |
| CreatedBy           | Created by              | Date and time that the item was created in the data source                      | Query, Retrieve, Search.     |
| CreatedTime         | Created date time       | Date and time that the item was created in the data source                      | Query, Retrieve             |
| Description         | Description             | A brief summary of the asset's content                                           | Query, Retrieve.             |
| Description         | Description             | A brief summary of the asset's content                                           | Query, Retrieve.             |
| FileExtension         |File extension      | the suffix at the end of a file name, e.g., .txt, .jpg, .exe             | Query, Refine, Retrieve.           |
|FileName            |Title     | The file name of the asset           | Query, Retrieve, Search.          |
| LastModifiedBy      | Last modified by        | Name of the person who most recently edited the item in the data source         | Search, Query, Retrieve.     |
| Length    |     | Image Length        | Query, Retrieve   |
| Link                | URL                     | The target URL of the item in the data source                                   | Retrieve.             |
| ModifiedTime        | Last modified date time | Date and time the item was last modified in the data source                     | Query, Refine, Retrieve.         |
| PublishedBy         |           | Name of the person who published the item in the data source                    | Query, Retrieve, Search.           |
| PublishedTime       |    | Date and time the item was published in the data source                         | Query, Retrieve.             |
|title            |               | The title of the items                                                      |Query, Retrieve, Search.           |
| Width   |     | Width         | Query, Retrieve   |
| Tags                | Tags                    | Tags defined in AEM Assets metadata. In AEM, tags are organized hierarchically   | Query, Retrieve, Search.     |

### Sync 
You can configure full and incremental crawls based on the scheduling options present here. By default, incremental crawl is set for every 15 minutes, and full crawl is set for every day. If needed, you can adjust these schedules to fit your data refresh needs.

## Troubleshooting
After publishing your connector, you can review the status under **Data sources** in the [admin center](https://admin.microsoft.com). To learn how to make updates and deletions, see [Manage your connector](manage-connector.md).

If you have issues or want to provide feedback, see [Microsoft Graph support](https://developer.microsoft.com/graph/support).
