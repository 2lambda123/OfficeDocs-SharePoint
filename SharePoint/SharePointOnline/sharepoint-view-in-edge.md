---
title: "View SharePoint files with File Explorer in Microsoft Edge"
ms.reviewer: 
ms.author: adjoseph
author: adeejoseph
manager: serdars
recommendations: true
audience: Admin
f1.keywords:
- NOCSH
ms.topic: article
ms.service: sharepoint-online
ms.localizationpriority: medium
ms.collection:  
- Strat_SP_admin
- M365-collaboration
ms.custom:
- seo-marvel-apr2020
search.appverid:
- SPO160
- BSA160
- GSP150
- MET150
description: "Learn about SharePoint view in File Explorer for Edge."
---

# View SharePoint files with File Explorer in Microsoft Edge

Last year, we announced that Microsoft 365 apps and services would no longer support Internet Explorer 11 (IE 11). As a result, we no longer recommend View in File Explorer and encourage using the OneDrive sync client. The OneDrive sync client provides [Files On-Demand](https://support.office.com/article/0e6860d3-d9f3-4971-b321-7092438fb38e), which allows you to access all your files in SharePoint without using up local storage space. For info about using OneDrive to sync SharePoint files, visit [SharePoint file sync](sharepoint-sync.md).

The View in File Explorer menu option will not be visible to you or users in the SharePoint modern document library by default. In certain cases, organizations may still need to use View in File Explorer to access modern document libraries. Starting in Microsoft Edge Stable version 93, you can enable the View in File Explorer capability on SharePoint for modern document libraries.

</br>

## Configure View in File Explorer with Edge

Follow the steps below to use View in File Explorer in Edge:

1. Verify that devices are on Edge build 93 or later using [Find out which version of Microsoft Edge you have](https://support.microsoft.com/en-us/microsoft-edge/find-out-which-version-of-microsoft-edge-you-have-c726bee8-c42e-e472-e954-4cf5123497eb).

2. Enable the [ConfigureViewInFileExplorer](/deployedge/microsoft-edge-policies#configureviewinfileexplorer) Edge policy that  allows URLs with the viewinfileexplorer: scheme to open WebDAV URLs in Windows File Explorer.

3. Use the options below to enable View in File Explorer using group policy or Intune:

</br>

<details>
<summary><b>To enable using group policy</b></summary>

1. First, configure Microsoft Edge policy settings  by following the steps at [Configure Microsoft Edge policy settings on Windows](/deployedge/configure-microsoft-edge)
2. Ensure you have downloaded the Microsoft. administrative template at [Download and deploy Microsoft Edge for business](https://www.microsoft.com/en-us/edge/business/download) or you may not see the policy listed.
3. Once the template is downloaded, open the Group Policy Object Editor. Right-click **Administrative Templates** in the Computer Configuration or User Configuration node and select **Add/Remove Templates** and browse to the downloaded template.
4. When applying the policy, ensure you update the domain to your tenant domain or use **sharepoint.com** if you plan on visiting multiple SharePoint tenants. 
5. Enabling the group policy may require a refresh of client group policy settings. After changing the group policy settings, refresh the settings. From a Command Prompt, enter **GPUpdate.exe /force**.

    Example below with the Group Policy value: 
`[{"cookies": ["rtFa", "FedAuth"], "domain": "sharepoint.com"}]`
    :::image type="content" source="media/edgepolicy-adeejoseph.png" alt-text="Enable Configure the View in File Explorer feature for SharePoint pages in Microsoft Edge":::

</details>

<details>
<summary><b>To enable using Intune</b></summary>

1. First, configure Microsoft Edge policy settings  by following the steps at  [Configure Microsoft Edge policy settings with Microsoft Intune](/deployedge/configure-edge-with-intune).
2. Verify the policy has been enabled by opening Edge and navigating to Edge://policy/.

    :::image type="content" source="media/microsoft-edge-policy.png" alt-text="Snapshot of Microsoft Edge Policies page ":::

    > [!TIP] 
    > You may need to close and re-open Edge for the policy to appear.

3. As a tenant administrator, update your SharePoint Online tenant configuration via SharePoint Online Management Shell to allow the “View in File Explorer” option to show in the UX for Microsoft Edge Browser using the following steps:

    1. Connect to SharePoint Online Management Shell by running: `Connect-SPOService -Url https://contoso-admin.sharepoint.com`

    1. Run the following cmdlet to show the “View in File Explorer” menu option: 
    `Set-SPOTenant -ViewInFileExplorerEnabled $True`

    > [!NOTE]
    > Ensure the management shell version is 16.0.21610.12000 or higher or the ViewInFileExplorerEnabled option will not be available.

4. Next, update your SharePoint Online tenant configuration via SharePoint Online Management Shell to allow persisted cookies for View with Explorer.

	1. Run the following cmdlet to enable persistent cookies.
	      
    `Set-SPOTenant -UsePersistentCookiesForExplorerView $true`

    > [!NOTE]
    > This capability will only work for users in the tenant where the group policy is active.


</details>
</br>

## Frequently asked questions (FAQs)

**Where is the View In File Explorer button?**
</br>
You can find the View in Explorer button after the tenant setting has been enabled by navigating to the **Library** >  Select the **Library View Menu** on the right-hand side > Select **View In File Explorer**.

:::image type="content" source="media/view-in-file-explorer.png" alt-text="Menu for View in File Explorer":::
</br></br>

**How long do I need to wait before the tenant setting takes effect?**
</br>
The tenant setting should be effective immediately but may take 15 minutes to start showing the button in the UX.
</br></br>

**What happens if I have the tenant setting applied, but not the Edge policy?**
</br>
If you have enabled ViewInFileExplorerEnabled, you may notice the option appear in your SharePoint library to View In File Explorer. Without the Edge policy enabled, clicking the button will result in a blank screen.
</br> </br>