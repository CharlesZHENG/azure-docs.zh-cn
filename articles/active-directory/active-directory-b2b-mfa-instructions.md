---
title: "Azure Active Directory B2B 协作用户的条件性访问 | Microsoft Docs"
description: "Azure Active Directory B2B 协作支持多重身份验证 (MFA)，以便对公司应用程序进行选择性访问"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/11/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: fac4657e9b78e900c052384e212aa9f3915f970d
ms.lasthandoff: 04/13/2017


---

# <a name="conditional-access-for-b2b-collaboration-users"></a>B2B 协作用户的条件性访问

## <a name="multi-factor-authentication-for-b2b-users"></a>针对 B2B 用户的多重身份验证
通过 Azure AD B2B 协作，企业可以针对 B2B 用户强制实施多重身份验证策略，这是一项企业独有的功能。 可以在租户级别、应用级别或个人用户级别强制实施这些策略，也可以通过相同的方式针对全职员工和组织成员启用这些策略。 资源组织中强制实施 MFA 策略。

这意味着：
1. 公司 A 中的管理员或信息工作者邀请公司 B 中的用户加入公司 A 中的应用程序 Foo。
2. 公司 A 中的应用程序 *Foo* 配置为在访问时需要进行 MFA。
3. 当公司 B 中的用户尝试访问公司 A 租户中的应用 Foo 时，他们会被要求完成公司 A 的 MFA 策略要求的 MFA 质询。
4. 用户可以使用公司 A 设置其 MFA，选择其 MFA 选项
5. 这将适用于任何标识（Azure AD 或 MSA，例如，如果公司 B 中的用户使用社交 ID 进行身份验证）
6. 公司 A 将需要具有足够的高级 Azure AD SKU 以支持 MFA。 公司 B 中的用户将使用公司 A 提供的此许可证。

总之，邀请方租户*始终*负责对合作伙伴组织中的 B2B 协作用户进行 MFA，而不是由合作伙伴组织本身进行 MFA（即使它具有 MFA 功能，也是如此）。

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>为 B2B 协作用户设置 MFA
若要了解为 B2B 协作用户设置 MFA 有多轻松，请参阅以下视频：

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>用于产品/服务兑换的 B2B 用户 MFA 体验
请查看下面的动画了解兑换体验，如以下视频中所示：

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>为 B2B 协作用户重置 MFA
目前，管理员可以要求 B2B 协作用户只使用以下 PowerShell cmdlet 再次身份验证。 因此，如果要重置 B2B 协作用户的身份验证方法，应使用以下 PowerShell cmdlet。

1. 连接到 Azure AD

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. 使用身份验证方法获取所有用户

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  下面是一个示例：

  ```
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. 重置特定用户的 MFA 方法。 然后可以使用该 UserPrincipalName 运行 reset 命令，以要求 B2B 协作用户重新设置身份验证方法。 示例：

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a>为什么在资源租户中执行 MFA？

在当前版本中，资源租户始终需要执行 MFA。 其原因可以预测。

例如，假设 Contoso 用户 (Sally) 被邀请到 Fabrikam，并且 Fabrikam 针对 B2B 用户启用了 MFA。

现在，如果 Contoso 对 App1 启用了 MFA 策略，但对 App2 未启用该策略 - 那么如果通过查看令牌中的 Contoso MFA 声明来确定 Sally 是否应该在 Fabrikam 中进行 MFA，则可能会出现以下问题：

* 第 1 天：Sally 访问 App1 时在 Contoso 中进行了 MFA，所以在 Fabrikam 中不会看到 MFA 提示。

* 第 2 天：Sally 在 Contoso 中访问了 App2，现在当她访问 Fabrikam 时，就必须在 Fabrikam 中注册 MFA。

这可能会令 Sally 感到困惑，并且很可能会导致成功登录的次数降低。

此外，即使 Contoso 启用了 MFA 功能，Fabrikam 也不可能一直信任 Contoso MFA 策略。

最后，资源租户 MFA 还适用于 MSA 和社交 ID，以及尚未设置 MFA 的合作伙伴组织。

因此，在针对 B2B 用户进行 MFA 方面，我们建议始终需要进行资源 MFA。 这可能会导致有些时候需要进行两次 MFA，但是无论在什么时候访问资源租户，最终用户体验都是可以预测的：Sally 必须注册资源租户的 MFA。

### <a name="device-location-and-risk-based-conditional-access-for-b2b-users"></a>B2B 用户基于设备、位置和风险的条件性访问

当 Contoso 组织为其公司数据启用基于设备的条件性访问策略时，会阻止从非托管设备（即，不受 Contoso 组织管理并且不符合 Contoso 设备策略的设备）进行访问。

如果 B2B 用户的设备不受 Contoso 管理，则在强制实施这些策略的任何环境中，合作伙伴组织中的 B2B 用户进行的访问都将被阻止。

希望其他组织中的用户让发出邀请的组管织来理其设备是非常高的要求。 因此在以后的更新中，我们会使 Contoso 信任特定合作伙伴的设备符合性状态。 这样当 Fabrikam 中的用户使用 Fabrikam 管理的设备访问 Contoso 资源时，Contoso 也可以强制实施策略。

同时，Contoso 可以创建包含特定合作伙伴用户的排除列表，将其排除在基于设备的条件性访问策略之外。

#### <a name="location-based-conditional-access-for-b2b"></a>针对 B2B 的基于位置的条件性访问

如果发出邀请的组织（例如 Contoso）能够创建定义其合作伙伴组织（例如 Fabrikam）的受信任网络边界（即 IP 地址范围），则可以针对 B2B 用户强制实施基于位置的条件性访问策略。

#### <a name="risk-based-conditional-access-for-b2b"></a>针对 B2B 的基于风险的条件性访问

目前是在 B2B 用户的本组织（即 B2B 用户的标识租户）中进行风险评估，因此不能对 B2B 用户应用基于登录风险的策略。

在以后的更新中，我们正在考虑将各合作伙伴的风险分数联合起来（经合作伙伴同意），这样 Contoso 不仅可以保护其外部的共享应用和数据远离已知风险，而且还能保护其远离可能在其他地方发生的未知风险。

## <a name="next-steps"></a>后续步骤

在 Azure AD B2B 协作网站上浏览我们的其他文章：

* [什么是 Azure AD B2B 协作？](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Azure Active Directory 管理员如何添加 B2B 协作用户？](active-directory-b2b-admin-add-users.md)
* [信息工作者如何添加 B2B 协作用户？](active-directory-b2b-iw-add-users.md)
* [B2B 协作邀请电子邮件的元素](active-directory-b2b-invitation-email.md)
* [B2B 协作邀请兑换](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B 协作授权](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B 协作疑难解答](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B 协作常见问题 (FAQ)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B 协作 API 和自定义](active-directory-b2b-api.md)
* [在没有邀请的情况下添加 B2B 协作用户](active-directory-b2b-add-user-without-invite.md)
* [有关 Azure Active Directory 中应用程序管理的文章索引](active-directory-apps-index.md)

