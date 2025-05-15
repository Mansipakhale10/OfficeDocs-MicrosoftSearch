--- 
title: "Monday.com Microsoft Graph connector (preview)" 
ms.author: rantang
author: rantang
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
description: "Set up the Monday.com Microsoft Graph connector for Microsoft Search and Microsoft 365 Copilot." 
ms.date: 05/15/2025
---
# Monday.com Microsoft Graph Connector (preview)

The Monday.com Microsoft Graph Connector enables organizations to index board content from Monday.com into Microsoft Graph, making it accessible across Microsoft 365 experiences, including Microsoft 365 Copilot and Microsoft Search.

The connector integrates the Monday.com permission model to ensure that users only access authorized content. It enhances productivity by enabling better task discovery, automated workflows, and AI-assisted project tracking. By indexing Monday.com data, the connector helps teams streamline collaboration and improve decision-making across projects.

## Key Benefits

- **Enhanced searchability of work items:** Enables Microsoft Search to retrieve Monday.com boards, groups, and items efficiently.
- **AI-assisted project management:** Uses Copilot to summarize, track, and generate updates for tasks.
- **Seamless content indexing:** Captures metadata, task descriptions, and key attributes from Monday.com.
- **Maintains permissions and compliance:** Respects the Monday.com built-in ACLs to ensure access control.

## Capabilities

The Monday.com connector enables:

- **Project & task indexing:** Makes Monday.com boards, groups, and items searchable across Microsoft 365.
- **AI-powered insights:** Enhances workflows with intelligent recommendations based on indexed task data.
- **Summarization & tracking:** Generates summaries of pending tasks, overdue work, and key updates.
- **User-permission enforcement:** Maintains the Monday.com permission settings to restrict access to authorized users.
- **Metadata indexing:** Captures task priority, status, due dates, assignees, and related attributes.
- **Custom filtering:** Allows indexing by workspace.

## Limitations

- Indexes only active boards, groups, and tasks.
- Does not index attachments or comments.

## Prerequisites  

### Configure OAuth APP in Monday.com  

1. Log in your Monday.com account and go to the **Monday.com Developer Center**
   
![Screenshot that shows the navigation path to the Monday.com Developer Center](media/monday-developer-center.png)

2. Click **Create app**.

![Screenshot that shows the button of "Create APP".](media/monday-create-app.png)  
 
3. In the **General Settings** section, locate and note down your **Client ID** and **Client secret**.

![Screenshot that shows how to find the Client id and Client secret for the Monday.com OAuth App.](media/monday-general-settings.png)  

4. In the **Build** section, open the **OAuth & permission** tab, click the **Scopes** subtab and **enable all read permissions**.

![Screenshot that shows how to configure essential permission for the Monday.com OAuth App.](media/monday-oauth-scopes-read-permission.png)  
 
5. Go to the **Redirect URLs** subtab and enter the following redirect URLs and click **Save Scopes**. 

   - **For Microsoft 365 Enterprise**, copy and paste: `https://gcs.office.com/v1.0/admin/oauth/callback`.  
   - **For Microsoft 365 Government**, copy and paste:  `https://gcsgcc.office.com/v1.0/admin/oauth/callback`.

![Screenshot that shows how to configure Redirect URL for the Monday.com OAuth App.](media/monday-redirect-URL.png)

6. Click **Promote to Live** to activate the app.

![Screenshot that shows how to activate Monday.com OAuth App.](media/monday-promote-to-live.png)

## Get started

### 1. Choose display name
Choose a display name that helps users recognize merge requests, issues, or documentation in a Copilot response.

### 2. Monday.com Instance URL
Enter the instance URL of your Monday.com instance (for example, `https://test-instance.monday.com`). 

### 3. Authenticate

- Enter your **Client ID** and **Client secret** from Monday.com.
- Choose **Authorize** to sign in and grant access.
- Grant the required API scopes.

### 4. Roll out to limited audience

Deploy this connection to a limited user base if you want to validate it in Copilot and other Search surfaces before expanding the rollout to a broader audience.

To create the connection for your Monday.com instance, click **Create* to publish your connection and index items from your Monday.com instance.  

For other settings, like Access Permissions, Data inclusion rules, Schema, Crawl frequency, etc., we set defaults based on what works best with Monday.com items. The default values settings are as follows.

|Page|Settings|Default values|
|--- | ---- | ---|
Users | Access permissions | Only people with access to this data source.
Users | Map Identities |Data source identities mapped using Microsoft Entra IDs.
Content | Index content | All cards, except the cards in personal space. 
Content | Manage properties | To check default properties and their schema, [click here](#content).
Sync | Incremental crawl | Frequency: Every 4 hours
Sync | Full crawl | Frequency: Every day

If you want to edit any of these values, you need to choose the **Custom setup** option. 

## Custom setup
Custom setup is for admins who want to edit the default values for any settings. When you choose **Custom setup**, you see three other tabs: **Users**, **Content**, and **Sync**. 

### Users
**Access permissions**

The Monday.com Microsoft Graph connector supports data visible to Only people with access to this data source (recommended) or Everyone. If you choose Everyone, indexed data appears in the search results for all users. 

#### Identity mapping
To ensure correct permission enforcement, map Monday.com user identities to Microsoft Entra ID. The following are the options:

To identify which option is suitable for your organization: 

1. Choose the **Microsoft Entra ID** option if the email ID of Monday.com users is same as the UserPrincipalName (UPN) of users in Microsoft Entra ID. 

2. Choose the **non-AAD** option if the email ID of Monday.com users is **different** from the UserPrincipalName (UPN) of users in Microsoft Entra ID.

>[!Important]
>- If you choose Microsoft Entra ID as the type of identity source, the connector maps the email IDs of users obtained from Monday.com directly to UPN property from Microsoft Entra ID.
>- If you chose "non-AAD" for the identity type see Map your non-Azure AD Identities for instructions on mapping the identities. You can use this option to provide the mapping regular expression from email ID to UPN.
>- Updates to users or groups governing access permissions are synced in full crawls only. Incremental crawls do not currently support the processing of updates to permissions.


### Content
On the **Content** tab, you can verify property mappings in the sample data for metadata such as **content**, **labels**, **description**, and **timestamps**.

**Content ingestion filters**   

You can choose what data you want to index. Use the regex expression of WorkSpaces to select your data before it is indexed, allowing you to control what data is searchable. Following are some examples to illustrate how to use regex expressions to select specific workspace(s).

| Scenario                            | Example Workspace Name(s)                                      | Regex Expression                            | Notes                                                        |
|-------------------------------------------|------------------------------------------------------|---------------------------------------------|-----------------------------------------------------------------------|
| Exact match for a single workspace        | `/workspace1/`                                                  | <code>^/workspace1/</code>                  | Exact matches `workspace1`                                           |
| Fuzzy match for a single workspace        | `/team-marketing-q1`                                  | <code>^/.*marketing.*/</code>               | Matches any workspace that contains "marketing" in the name          |
| Exact match for multiple workspaces       | `/workspace1`, `/workspace2`                           | <code>^/(workspace1&#124;workspace2)/</code> | Exact matches `workspace1` and `workspace2`                          |
| Fuzzy match for multiple workspaces       | `/workspace-marketing/`, `/workspace-sales/`          | <code>^/workspace-[a-z]+/</code>            | Matches any workspace starting with `workspace-` followed by letters |
| Fuzzy match for multiple keywords in name | `/workspace-engineering/`, `/workspace-sales-q4/`         | <code>^/.*(eng&#124;sales).*/</code>        | Matches any workspace with `eng` or `sales` in the name              |

Use the preview results button to verify the sample values of the selected properties and filters. 

**Manage properties**

Here, you can check available properties from your Guru. Assign a schema to the property (define whether a property is searchable, queryable, retrievable, or refinable), change the semantic label, and add an alias to the property. Properties that are selected by default are listed below. 

| Properties       | Semantic Label          | Schema                      |
|-----------------|------------------------|-----------------------------|
| CollectionLink  |                        | Retrieve                    |
| CollectionName  |                        | Query, Retrieve, Search     |
| Content        | `CONTENT`               | Search                      |
| CreatedTime    | Created date time       | Query, Retrieve             |
| LastModifiedBy | Last modified by        | Query, Retrieve, Search     |
| Link          | url                      | Retrieve                    |
| ModifiedTime   | Last modified date time | Query, Refine, Retrieve     |
| Owner         | Created by               | Query, Retrieve, Search     |
| Title         | Title                    | Query, Retrieve, Search     |


#### Filter  
You can configure filtering by **workspace** to refine the indexed content.  

### Sync
You can configure **incremental** and **full** crawls. The following are the default values:

  - Incremental crawl runs **every 2 hours** by default.
  - Full crawl runs **daily** to ensure up-to-date indexing.

## Next steps

- Review the connection status in the Microsoft 365 admin center. 
- If you have issues or need support, see [Microsoft Graph support](https://developer.microsoft.com/en-us/graph/support).
