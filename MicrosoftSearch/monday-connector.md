--- 
title: "Monday.com Microsoft Graph connector (preview)" 
ms.author: dannyyao
author: dannyyaou
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
ms.date: 02/26/2025
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

1. Go to the **Monday.com Developer Center** and create a **new app**.  
2. In the **Basic Information** section, locate and note down your **Client ID** and **Client secret**.  
3. Open the **OAuth** tab and **enable all read permissions**.  
4. Go to the **Redirect URLs** tab and enter the following URLs:  

   - **For Microsoft 365 Enterprise**, copy and paste: `https://gcs.office.com/v1.0/admin/oauth/callback`.  
   - **For Microsoft 365 Government**, copy and paste:  `https://gcsgcc.office.com/v1.0/admin/oauth/callback`.

5. Choose **Promote to Live** to activate the app.  

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
Before you deploy the connector, test the connection with a limited user base in Copilot and Microsoft Search.

## Custom setup
Custom setup is for admins who want to edit the default values for any settings. When you choose **Custom setup**, you see three other tabs: **Users**, **Content**, and **Sync**. 

### Users
#### Identity mapping
To ensure correct permission enforcement, map Monday.com user identities to Microsoft Entra ID. The following are the options:
  - **Email:** Matches Monday.com email to Microsoft Entra ID user properties.

### Content
On the **Content** tab, you can verify property mappings in the sample data for metadata such as **content**, **labels**, **description**, and **timestamps**.

#### Filter  
You can configure filtering by **workspace** to refine the indexed content.  

### Sync
You can configure **incremental** and **full** crawls. The following are the default values:

  - Incremental crawl runs **every 2 hours** by default.
  - Full crawl runs **daily** to ensure up-to-date indexing.

## Next steps

- Review the connection status in the Microsoft 365 admin center. 
- If you have issues or need support, see [Microsoft Graph support](https://developer.microsoft.com/en-us/graph/support).
