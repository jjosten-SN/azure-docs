---
title: 'Tutorial: Azure AD SSO integration with Reprints Desk - Article Galaxy'
description: Learn how to configure single sign-on between Azure Active Directory and Reprints Desk - Article Galaxy.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/16/2022
ms.author: jeedes
---

# Tutorial: Azure AD SSO integration with Reprints Desk - Article Galaxy

In this tutorial, you'll learn how to integrate Reprints Desk - Article Galaxy with Azure Active Directory (Azure AD). When you integrate Reprints Desk - Article Galaxy with Azure AD, you can:

* Control in Azure AD who has access to Reprints Desk - Article Galaxy.
* Enable your users to be automatically signed-in to Reprints Desk - Article Galaxy with their Azure AD accounts.
* Manage your accounts in one central location - the Azure portal.

## Prerequisites

To get started, you need the following items:

* An Azure AD subscription. If you don't have a subscription, you can get a [free account](https://azure.microsoft.com/free/).
* Reprints Desk - Article Galaxy single sign-on (SSO) enabled subscription.
* Along with Cloud Application Administrator, Application Administrator can also add or manage applications in Azure AD.
For more information, see [Azure built-in roles](../roles/permissions-reference.md).

## Scenario description

In this tutorial, you configure and test Azure AD SSO in a test environment.

* Reprints Desk - Article Galaxy supports **IDP** initiated SSO.

* Reprints Desk - Article Galaxy supports **Just In Time** user provisioning.

## Add Reprints Desk - Article Galaxy from the gallery

To configure the integration of Reprints Desk - Article Galaxy into Azure AD, you need to add Reprints Desk - Article Galaxy from the gallery to your list of managed SaaS apps.

1. Sign in to the Azure portal using either a work or school account, or a personal Microsoft account.
1. On the left navigation pane, select the **Azure Active Directory** service.
1. Navigate to **Enterprise Applications** and then select **All Applications**.
1. To add new application, select **New application**.
1. In the **Add from the gallery** section, type **Reprints Desk - Article Galaxy** in the search box.
1. Select **Reprints Desk - Article Galaxy** from results panel and then add the app. Wait a few seconds while the app is added to your tenant.

## Configure and test Azure AD SSO for Reprints Desk - Article Galaxy

Configure and test Azure AD SSO with Reprints Desk - Article Galaxy using a test user called **B.Simon**. For SSO to work, you need to establish a link relationship between an Azure AD user and the related user in Reprints Desk - Article Galaxy.

To configure and test Azure AD SSO with Reprints Desk - Article Galaxy, perform the following steps:

1. **[Configure Azure AD SSO](#configure-azure-ad-sso)** - to enable your users to use this feature.
   1. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with B.Simon.
   1. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable B.Simon to use Azure AD single sign-on.
1. **[Configure Reprints Desk - Article Galaxy SSO](#configure-reprints-desk---article-galaxy-sso)** - to configure the single sign-on settings on application side.
   1. **[Create Reprints Desk - Article Galaxy test user](#create-reprints-desk---article-galaxy-test-user)** - to have a counterpart of B.Simon in Reprints Desk - Article Galaxy that is linked to the Azure AD representation of user.
1. **[Test SSO](#test-sso)** - to verify whether the configuration works.

## Configure Azure AD SSO

Follow these steps to enable Azure AD SSO in the Azure portal.

1. In the Azure portal, on the **Reprints Desk - Article Galaxy** application integration page, find the **Manage** section and select **single sign-on**.
1. On the **Select a single sign-on method** page, select **SAML**.
1. On the **Set up single sign-on with SAML** page, click the pencil icon for **Basic SAML Configuration** to edit the settings.

   ![Screenshot shows to edit Basic S A M L Configuration.](common/edit-urls.png "Basic Configuration")

1. On the **Basic SAML Configuration** section, the application is pre-configured and the necessary URLs are already pre-populated with Azure. The user needs to save the configuration by clicking the **Save** button.

1. Reprints Desk - Article Galaxy application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration. The following screenshot shows the list of default attributes.

	![Screenshot shows the image of Reprints Desk application.](common/default-attributes.png "Image")

1. In addition to above, Reprints Desk - Article Galaxy application expects few more attributes to be passed back in SAML response which are shown below. These attributes are also pre populated but you can review them as per your requirements.

	| Name | Source Attribute|
	| ------------ | --------- |
	| firstname | user.givenname |
	| lastname | user.surname |

1. On the **Set up single sign-on with SAML** page, in the **SAML Signing Certificate** section,  find **Federation Metadata XML** and select **Download** to download the certificate and save it on your computer.

	![Screenshot shows the Certificate download link.](common/metadataxml.png "Certificate")

1. On the **Set up Reprints Desk - Article Galaxy** section, copy the appropriate URL(s) based on your requirement.

	![Screenshot shows to copy configuration appropriate U R L.](common/copy-configuration-urls.png "Metadata")  

### Create an Azure AD test user

In this section, you'll create a test user in the Azure portal called B.Simon.

1. From the left pane in the Azure portal, select **Azure Active Directory**, select **Users**, and then select **All users**.
1. Select **New user** at the top of the screen.
1. In the **User** properties, follow these steps:
   1. In the **Name** field, enter `B.Simon`.  
   1. In the **User name** field, enter the username@companydomain.extension. For example, `B.Simon@contoso.com`.
   1. Select the **Show password** check box, and then write down the value that's displayed in the **Password** box.
   1. Click **Create**.

### Assign the Azure AD test user

In this section, you'll enable B.Simon to use Azure single sign-on by granting access to Reprints Desk - Article Galaxy.

1. In the Azure portal, select **Enterprise Applications**, and then select **All applications**.
1. In the applications list, select **Reprints Desk - Article Galaxy**.
1. In the app's overview page, find the **Manage** section and select **Users and groups**.
1. Select **Add user**, then select **Users and groups** in the **Add Assignment** dialog.
1. In the **Users and groups** dialog, select **B.Simon** from the Users list, then click the **Select** button at the bottom of the screen.
1. If you're expecting any role value in the SAML assertion, in the **Select Role** dialog, select the appropriate role for the user from the list and then click the **Select** button at the bottom of the screen.
1. In the **Add Assignment** dialog, click the **Assign** button.

## Configure Reprints Desk - Article Galaxy SSO

To configure single sign-on on **Reprints Desk - Article Galaxy** side, you need to send the downloaded **Federation Metadata XML** and appropriate copied URLs from Azure portal to [Reprints Desk - Article Galaxy support team](mailto:customersupport@reprintsdesk.com). They set this setting to have the SAML SSO connection set properly on both sides.

### Create Reprints Desk - Article Galaxy test user

In this section, a user called B.Simon is created in Reprints Desk - Article Galaxy. Reprints Desk - Article Galaxy supports just-in-time user provisioning, which is enabled by default. There is no action item for you in this section. If a user doesn't already exist in Reprints Desk - Article Galaxy, a new one is created after authentication.

## Test SSO 

In this section, you test your Azure AD single sign-on configuration with following options.

* Click on Test this application in Azure portal and you should be automatically signed in to the Reprints Desk - Article Galaxy for which you set up the SSO.

* You can use Microsoft My Apps. When you click the Reprints Desk - Article Galaxy tile in the My Apps, you should be automatically signed in to the Reprints Desk - Article Galaxy for which you set up the SSO. For more information about the My Apps, see [Introduction to the My Apps](../user-help/my-apps-portal-end-user-access.md).

## Next steps

Once you configure Reprints Desk - Article Galaxy you can enforce session control, which protects exfiltration and infiltration of your organization’s sensitive data in real time. Session control extends from Conditional Access. [Learn how to enforce session control with Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).