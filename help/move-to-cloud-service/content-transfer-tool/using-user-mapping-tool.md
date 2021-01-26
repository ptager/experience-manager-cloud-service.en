---
title: Using User Mapping Tool
description: Using User Mapping Tool
---

# Using User Mapping Tool {#user-mapping-tool}

## Overview {#overview}

As part of the transition journey to AEM as a Cloud Service, you need to move users and groups from your existing AEM system to AEM as a Cloud Service. This is done by the Content Transfer Tool. 

A major change to AEM as a Cloud Service is the fully integrated use of Adobe IDs for accessing the author tier.  This requires use of the Adobe Admin Console for managing users and user groups. The user-profile information is centralized in the Adobe Identity Management System (IMS) that provides single-sign-on across all Adobe cloud applications. For more details, refer to Identity Management. Because of this change, existing users and groups need to be mapped to their IMS IDs to avoid duplicate users and groups on the Cloud Service author instance.

## Important Considerations {#important-considerations} 

There are a few exceptional cases to be considered. The following specific cases will be logged and the user or group in question will not be mapped:

1. If a user has no email address in the `profile/email` field of their jcr node.

1. If a given email is not found on the IMS system for the Organization ID used (or if the IMS ID cannot be retrieved for another reason).

1. If the user is currently disabled it is treated the same as if it were not disabled.  It will be mapped and migrated as normal, and will remain disabled on the cloud instance.

## Using the User Mapping Tool {#using-user-mapping-tool}

The User Mapping Tool uses an API that allows it to lookup IMS users by email and return their IMS IDs. This API requires the user to create a Client ID for their organization, a Client Secret, and an Access Token/Bearer Token.  

Follow these steps to set this up:

1. Navigate to [Adobe Developer Console](https://console.adobe.io) using your Adobe ID.
1. Create a new project or open an existing project
1. Add an API
1. Choose User Management API
1. Create a JWT credential
1. Generate a key pair, or Upload a public key (rsa is no good)
1. Generate an access token (or JWT token or bearer token).
1. Save all this information (Client ID, Client Secret, Technical Account ID, Technical Account Email, Organization ID, Access Token) in a safe place.

## User Interface {#user-interface}

The User Mapping Tool is integrated into the Content Transfer Tool. You can download the Content Transfer Tool from Software Distribution Portal. For more details on the latest version, refer to Release Notes.

1. Select Select the Adobe Experience Manager and navigate to tools -> **Operations** -> **Content Transfer**.
1. Click on **Create User Mapping Config**.

   >[!NOTE]
   >If you skip this step, users and groups mapping will be skipped during Extraction phase.

   Populate the fields in User Management API Configuration as described below:

   * **Org ID**:  Enter the IMS Org ID for the organization the users are being migrated.  

      >[!NOTE]
      >To get the Org ID, log into the [Admin Console](https://adminconsole.adobe.com/) and choose your organization (in the top right area) if you belong to more than one. The Org ID will be in the URL of that page, in the format like `xx@AdobeOrg`, where xx is the IMS Org ID.  Alternately, you can find the Org ID in the [Adobe Developer Console](https://console.adobe.io) page where you generate the Access Token.

   * **Client ID**: Enter the Client ID that you saved from the Setup step 

   * **Access Token**: Enter the Access Token that you saved from the Setup step

      >[!NOTE]
      >The Access Token expires every 24 hours and a new one needs to be created. To create a new token, go back into [Adobe Developer Console](https://console.adobe.io), choose your project, click on User Management API and paste the same private key into the box.

1. After entering the above information, click on Save.

1. Create a Migration Set by clicking on Create Migration Set and populating the fields and then clicking on Save. For more details, refer to Running the Content Transfer Tool.

   >[!NOTE]
   >The toggle switch to include Mapping Users from IMS Users and Groups is ON by default. With this setting, when Extraction is performed on this migration set, the User Mapping Tool will run as part of the Extraction phase. This is the recommended way to run the Extraction phase of the Content Transfer Tool. If this toggle is turned OFF and/or User Mapping Config is not created, users and groups mapping will be skipped during the Extraction phase.

1. To run Extraction phase, refer to [Running the Content Transfer Tool](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/using-content-transfer-tool.html?lang=en#running-tool).


