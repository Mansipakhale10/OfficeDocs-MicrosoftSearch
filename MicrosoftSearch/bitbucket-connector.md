---
title: "Bitbucket Microsoft Graph connectors (preview)"
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
description: "Set up the Bitbucket Microsoft Graph connectors for Microsoft Search and Microsoft 365 Copilot."
ms.date: 02/14/2025
---

# Bitbucket Microsoft Graph connectors (preview)

The Bitbucket Microsoft Graph connectors (Bitbucket Cloud Pull Request and Bitbucket Cloud Knowledge) allow your organization to index pull requests and documentation (.txt and .md files) stored in BitBucket. After you configure the connector and index Bitbucket content, users can search and retrieve information via Microsoft Search and Microsoft 365 Copilot.

This article is intended for Microsoft 365 administrators or anyone who configures, runs, or monitors Bitbucket Microsoft Graph connectors.

## Capabilities

- Index Bitbucket repositories, pull requests, and documentation.
- Enable Microsoft Search and Microsoft 365 Copilot to retrieve Bitbucket data efficiently.
- Maintain Bitbucket ACLs and user permissions.
- Allow administrators to customize crawl frequency and indexing preferences.

## Limitations

- The connector does not support indexing Bitbucket CI/CD pipelines beyond status indexing.
- Only repositories, pull requests, .md, and .txt files are indexed.
- On-premises/self-hosted Bitbucket instances aren't currently supported.
- The connector may leave the LastModifiedBy field blank in cases where Git changes are not mapped to a Bitbucket account. This occurs when a manual configuration linking Git changes to Bitbucket user accounts is not completed before an incremental crawl.

## Prerequisites

Before you set up the connector, make sure that:

1. Your Bitbucket instance is accessible via API.
2. You generate a **Client ID** and **Client secret** from Bitbucket for authentication.
3. The user account used for authentication has access to the repositories, pull requests, and knowledge files to be indexed.
4. The client ID and client secret have the **repository:read**, **account:read,** and **pullrequest** permissions.
5. Users who access indexed Bitbucket data have corresponding **Microsoft Entra ID** identities for permission mapping.

We recommend using separate user accounts for OAuth authentication with each connection as Bitbucket's rate limit is calculated individually per user.

## Get started

### 1. Choose display name
Choose a display name that helps users recognize merge requests or documentation in a Copilot response.

### 2. Bitbucket instance URL
Enter the URL of your Bitbucket instance (for example, `https://bitbucket.org/testinstance`).

### 3. Authenticate

- Enter your **Client ID** and **Client secret** from Bitbucket.
- Choose **Authorize** to sign in and grant access.
- Grant the required API scopes.

### 4. Roll out to limited audience
Before you deploy the connector, test the connection with a limited user base in Copilot and Microsoft Search.

## Custom setup
Custom setup is for admins who want to edit the default values for any settings. When you choose **Custom setup**, you see three other tabs: **Users**, **Content**, and **Sync**. 

### Users
#### Identity mapping
By default, due to the limitation of Bitbucket API, the connector maps emails in Microsoft Entra ID using public names from Bitbucket.
If this mapping does not align with your configuration, customize the identity mapping.

To ensure correct permission enforcement, map Bitbucket user identities to Microsoft Entra ID. The following are the options:
  - **Full name:** Matches Bitbucket full names to Microsoft Entra ID user properties.
  - **Public name:** Maps Bitbucket public names with Microsoft Entra ID user properties.

If direct mapping fails, use **regular expressions (regex)** for transformation.

### Content
On the **Content** tab, you can verify property mappings in the sample data for metadata such as **content**, **labels**, **description**, and **timestamps**.

### Sync
You can configure **incremental** and **full** crawls. The following are the default values:

  - Incremental crawl runs **every 15 minutes** by default.
  - Full crawl runs **daily** to ensure up-to-date indexing.

## Next steps

- Review the connection status in the Microsoft 365 Admin Center. 
- If you have issues or need support, see [Microsoft Graph support](https://developer.microsoft.com/en-us/graph/support).
