---
title: How To - Require managed devices for cloud app access with Azure Active Directory conditional access | Microsoft Docs
description: Learn how to configure Azure Active Directory (Azure AD) device-based conditional access policies that require managed devices for cloud app access.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''

ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2018
ms.author: markvi
ms.reviewer: jairoc
#Customer intent: As a It admin, I wan to know how to require managed devices for the access to certain resources to ensure that they are accessed only from devices that meet my standards for security and compliance

---

# How To: Require managed devices for cloud app access with conditional access

In a mobile-first, cloud-first world, Azure Active Directory (Azure AD) enables single sign-on to apps, and services from anywhere. Authorized users can access your cloud apps from a broad range of devices including mobile and also personal devices. However, many environments have at least a few apps that should only be accessed by devices that meet your standards for security and compliance. These devices are also known as managed devices. 

This article explains how you can configure conditional access policies that require managed devices to access certain cloud apps in your environment. 


## Prerequisites

Requiring managed devices for cloud app access ties **Azure AD conditional access** and **Azure AD device management** together. If you are not familiar with one of these areas yet, you should read the following topics, first:

- **[Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)** - This article provides you with a conceptual overview of conditional access and the related terminology.

- **[Introduction to device management in Azure Active Directory](device-management-introduction.md)** - This article gives you an overview of the various options you have to get devices under organizational control. 


## Scenario description

Mastering the balance between security and productivity is a challenge. The proliferation of supported devices to access your cloud resources helps to improve the productivity of your users. On the flip side, you probably don't want certain resources in your environment to be accessed by devices with an unknown protection level. For the affected resources, you should require that users can only access them using a managed device. 

With Azure AD conditional access, you can address this requirement with a single policy that grants access:

- To selected cloud apps

- For selected users and groups

- Requiring a managed device


## Managed devices  

In simple terms, managed devices are devices that are are under *some sort* of organizational control. In Azure AD, the prerequisite for a managed device is that it has been registered with Azure AD. Registering a device creates an identity for the device in form of a device object. This object is used by Azure to track status information about a device. As an Azure AD administrator, you can already use this object to toggle (enable/disable) the state of a device.
  
![Device-based conditions](./media/active-directory-conditional-access-policy-connected-applications/32.png)

To get a device registered with Azure AD, you have three options:

- **[Azure AD registered devices](device-management-introduction.md#azure-ad-registered-devices)** - to get a personal device registered with Azure AD

- **[Azure AD joined devices](device-management-introduction.md#azure-ad-joined-devices)** - to get an organizational Windows 10 device that is not joined to an on-premises AD registered with Azure AD. 

- **[Hybrid Azure AD joined devices](device-management-introduction.md#hybrid-azure-ad-joined-devices)** - to get a Windows 10 device that is joined to an on-premises AD registered with Azure AD.

To become a managed device, a registered device must be either a **Hybrid Azure AD joined device** or a **device that has been marked as compliant**.  

![Device-based conditions](./media/active-directory-conditional-access-policy-connected-applications/47.png)

 
## Require Hybrid Azure AD joined devices

In your conditional access policy, you can select **Require Hybrid Azure AD joined device** to state that the selected cloud apps can only be accessed using a managed device. 

![Device-based conditions](./media/active-directory-conditional-access-policy-connected-applications/10.png)

This setting only applies to Windows 10 devices that are joined to an on-premises AD. You can only register these devices with Azure AD using a Hybrid Azure AD join, which is an [automated process](device-management-hybrid-azuread-joined-devices-setup.md) to get a Windows 10 device registered. 

![Device-based conditions](./media/active-directory-conditional-access-policy-connected-applications/45.png)

What makes a Hybrid Azure AD joined device a managed device?  For devices that are joined to an on-premises AD, it is assumed that the control over these devices is enforced using management solutions such as **System Center Configuration Manager (SCCM)** or **group policy (GP)** to manage them. Because there is no method for Azure AD to determine whether any of these methods has been applied to a device, requiring a hybrid Azure AD joined device is a relatively weak mechanism to require a managed device. It is up to you as an administrator to judge whether the methods that are applied to your on-premises domain-joined devices are strong enough to constitute a managed device if such a device is also a Hybrid Azure AD joined device.


## Require device to be marked as compliant

The option to *require a device to be marked as compliant* is the strongest form to request a managed device.

![Device-based conditions](./media/active-directory-conditional-access-policy-connected-applications/11.png)

This option requires a device to be registered with Azure AD, and also to be marked as compliant by:
         
- Intune.
- A third-party mobile device management (MDM) system that manages Windows 10 devices via Azure AD integration. Third-party MDM systems for device OS types other than Windows 10 are not supported.
 
![Device-based conditions](./media/active-directory-conditional-access-policy-connected-applications/46.png)



For a device that is marked as compliant, you can assume that: 

- The mobile devices your workforce uses to access company data are managed
- Mobile apps your workforce uses are managed
- Your company information is protected by helping to control the way your workforce accesses and shares it
- The device and its apps are compliant with company security requirements




## Next steps

Before configuring a device-based conditional access policy in your environment, you should take a look at the [best practices for conditional access in Azure Active Directory](active-directory-conditional-access-best-practices.md).

