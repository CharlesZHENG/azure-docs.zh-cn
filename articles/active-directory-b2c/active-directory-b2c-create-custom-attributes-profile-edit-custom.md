---
title: "Azure Active Directory B2C：将自己的属性添加到自定义策略并在配置文件编辑中使用 | Microsoft Docs"
description: "有关使用扩展属性、自定义属性以及将其包含在用户界面中的演练"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/29/2017
ms.author: joroja
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 83748140c7b92b95a648ae3ecf78f22e2393780b
ms.contentlocale: zh-cn
ms.lasthandoff: 05/08/2017

---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C：在自定义配置文件编辑策略中创建和使用自定义属性。

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

在本文中，你将在 Azure AD B2C 目录中创建一个自定义属性，然后将此新属性用作配置文件编辑用户旅程中的自定义声明。

## <a name="prerequisites"></a>先决条件

完成[自定义策略入门](active-directory-b2c-get-started-custom.md)一文中的步骤。

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a>使用自定义属性来收集有关使用自定义策略的 Azure Active Directory B2C 中的使用者的信息
Azure Active Directory (Azure AD) B2C 目录附带了一组内置属性：名字、姓氏、城市、邮政编码、userPrincipalName，等等。我们经常需要创建自己的属性。  例如：
* 面向客户的应用程序需要保留“LoyaltyNumber”等属性。
* 标识提供者具有必须保存的唯一用户标识符，例如“uniqueUserGUID”。
* 自定义用户旅程需要保留用户的状态，例如“migrationStatus”。

在 Azure AD B2C 中，可以扩展存储在每个用户帐户中的属性集。 你还可以使用 [Azure AD 图形 API](active-directory-b2c-devquickstarts-graph-dotnet.md) 读取和写入这些属性。

[!NOTE]
我们将自定义属性或扩展属性称作 Azure AD B2C 目录的特征。  扩展属性扩展目录中用户对象的架构。  若要将自定义属性用作策略中的自定义声明，请在策略中的 `ClaimsSchema` 内定义该属性。

[!NOTE]
尽管扩展属性可以包含用户的数据，但它们只能在应用程序对象中注册。 属性将附加到应用程序。 必须向应用程序对象授予写入访问权限，使其能够注册扩展属性。 可将 100 个扩展属性（针对所有类型和所有应用程序）写入任何一个对象。 扩展属性将添加到目标目录类型，然后在 Azure AD B2C 目录租户中立即可供访问。
如果删除了该应用程序，则也会删除这些扩展属性及其为所有用户包含的所有数据。 如果应用程序删除了某个扩展属性，该属性将从目标目录对象中删除，其包含的所有数据也会一并删除。

[!NOTE]
扩展属性只在租户中已注册的应用程序的上下文中存在。 必须在使用该应用程序的 TechnicalProfile 中包含该应用程序的对象 ID

[!NOTE]
Azure AD B2C 目录通常包含名为 `b2c-extensions-app` 的 Web API 应用。  此应用程序主要由通过 Azure 门户创建的自定义声明的 b2c 内置策略使用。  仅建议高级用户使用此应用程序来注册 b2c 自定义策略的扩展。


## <a name="creating-a-new-application-to-store-the-extension-properties"></a>创建用于存储扩展属性的新应用程序

1. 打开浏览会话并导航到 [Azure 门户](https://portal.azure.com)，然后使用想要配置的 B2C 目录的管理凭据登录。
1. 在左侧导航菜单中单击 `Azure Active Directory`。 可能需要选择“更多服务>”才能找到该选项。
1. 选择 `App registrations`，然后单击 `New application registration`。
1. 提供以下建议条目：
  * 指定 Web 应用程序的名称：`WebApp-GraphAPI-DirectoryExtensions`
  * 应用程序类型：Web 应用/API
  * 登录 URL：https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions
1. 选择 `Create`。 `notifications` 中会出现成功完成消息。
1. 选择新建的 Web 应用程序：`WebApp-GraphAPI-DirectoryExtensions`
1. 选择“设置”：`Required permissions`
1. 选择 API `Windows Active Directory`
1. 勾选应用程序权限：`Read and write directory data` 和 `Save`
1. 选择 `Grant permissions` 并确认 `Yes`。
1. 复制到剪贴板，并保存“Web 应用”>“图形 API”>“目录扩展”>“设置”>“属性”中的以下标识符
*  `Application ID`。 示例：`103ee0e6-f92d-4183-b576-8c3739027780`
* `Object ID`。 示例：`80d8296a-da0a-49ee-b6ab-fd232aa45201`

## <a name="modifying-your-custom-policy-to-add-the-applicationobjectid"></a>修改自定义策略以添加 `ApplicationObjectId`

对于要读取或写入属性扩展的每个技术配置文件，都必须添加 `<Metadata>` 元素，其中包含在前面步骤中获取的以下两个项：ApplicationObjectId 和 ClientId。

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item>
                <Item Key="ClientId">insert appId here</Item>
              </Metadata>
            <!-- End of changes -->
              <CryptographicKeys>
                <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
              </CryptographicKeys>
              <IncludeInSso>false</IncludeInSso>
              <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
            </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
```
[!NOTE]
<TechnicalProfile Id="AAD-Common"> 是“通用”的，因为其元素将在使用以下元素的所有 Azure Active Directory 技术配置文件中重复使用：
```
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
```
[!NOTE]
当技术配置文件在新建的扩展属性中首次写入时，你可能会遇到一次性错误，因为找不到该属性时会创建它。  .*  

## <a name="using-the-new-extension-property--custom-attribute-in-a-user-journey"></a>在用户旅程中使用新的扩展属性/自定义属性


1. 打开描述策略编辑用户旅程的信赖方 (RP) 文件。  如果你要开始演练，我们建议直接从 Azure 门户中的“Azure B2C 自定义策略”部分下载已配置的 RP-PolicyEdit 文件版本。  或者，从存储文件夹打开 XML 文件。
2. 添加自定义声明 `loyaltyId`。  在 `<RelyingParty>` 元素中包含自定义声明后，它将作为参数传递给 UserJourney 技术配置文件，并包含在应用程序的令牌中。
```xml
<RelyingParty>
   <DefaultUserJourney ReferenceId="ProfileEdit" />
   <TechnicalProfile Id="PolicyProfile">
     <DisplayName>PolicyProfile</DisplayName>
     <Protocol Name="OpenIdConnect" />
     <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
       <OutputClaim ClaimTypeReferenceId="city" />

       <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />

     </OutputClaims>
     <SubjectNamingInfo ClaimType="sub" />
   </TechnicalProfile>
 </RelyingParty>
 ```
3. 如下所示，将声明定义添加到扩展策略文件 `TrustFrameworkExtensions.xml` 中的 ``<ClaimsSchema>`` 元素内。
```xml
<ClaimsSchema>
        <ClaimType Id="extension_loyaltyId">
            <DisplayName>Loyalty Identification Tag</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your loyalty number from your membership card</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
</ClaimsSchema>
```
4. 将相同的声明定义添加到基本策略文件 `TrustFrameworkBase.xml`。  
注意：通常不必要同时在基本文件和扩展文件中添加 `ClaimType` 定义，但由于后续步骤要将 extension_loyaltyId 添加到基本文件中的技术配置文件，如果未提供该定义，策略验证程序将拒绝上传基本文件。

注意：跟踪 TrustFrameworkBase.xml 文件中名为“ProfileEdit”的用户旅程的执行可能会有所帮助。  在编辑器中搜索同名的用户旅程，然后观察业务流程步骤 5 是否调用 TechnicalProfileReferenceID ="SelfAsserted ProfileUpdate"。  请搜索并检查此技术配置文件，以熟悉相关的流。

5. 在技术配置文件“SelfAsserted-ProfileUpdate”中添加 loyaltyId 作为输入和输出声明

```xml
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
          <DisplayName>User ID signup</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>

            <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
            <InputClaim ClaimTypeReferenceId="userPrincipalName" />

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updateed by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updateed by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. 在技术配置文件“AAD-UserWriteProfileUsingObjectId”中添加声明，以保留目录中当前用户的扩展属性中的声明值。

```xml
<TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
          <Metadata>
            <Item Key="Operation">Write</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <PersistedClaims>
            <!-- Required claims -->
            <PersistedClaim ClaimTypeReferenceId="objectId" />

            <!-- Optional claims -->
            <PersistedClaim ClaimTypeReferenceId="givenName" />
            <PersistedClaim ClaimTypeReferenceId="surname" />
            <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />

          </PersistedClaims>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
```
7. 在技术配置文件“AAD-UserReadUsingObjectId”中添加声明，以便每次用户登录时都会读取扩展属性的值。
注意：到目前为止，技术配置文件只是在本地帐户流中发生更改。  如果社交/联合帐户流中需要新属性，则需要更改一组不同的技术配置文件。 请参阅后续步骤。

```xml
<!-- The following technical profile is used to read data after user authenticates. -->
     <TechnicalProfile Id="AAD-UserReadUsingObjectId">
       <Metadata>
         <Item Key="Operation">Read</Item>
         <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
       </Metadata>
       <IncludeInSso>false</IncludeInSso>
       <InputClaims>
         <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
       </InputClaims>
       <OutputClaims>
         <!-- Optional claims -->
         <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
         <OutputClaim ClaimTypeReferenceId="displayName" />
         <OutputClaim ClaimTypeReferenceId="otherMails" />
         <OutputClaim ClaimTypeReferenceId="givenName" />
         <OutputClaim ClaimTypeReferenceId="surname" />
         <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
       </OutputClaims>
       <IncludeTechnicalProfile ReferenceId="AAD-Common" />
     </TechnicalProfile>
```

[!IMPORTANT]
上述 IncludeTechnicalProfile 元素将 AAD-Common 的所有元素添加到此技术配置文件。

## <a name="test-the-custom-policy-using-run-now"></a>使用“立即运行”测试自定义策略

     1. Open the **Azure AD B2C Blade** and navigate to **Identity Experience Framework > Custom policies**.
     2. Select the custom policy that you uploaded, and click the **Run now** button.
     3. You should be able to sign up using an email address.

发回到应用程序的 ID 令牌会将新扩展属性包含为前面带有 extension_loyaltyId 的自定义声明。 请参阅示例。

```
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a>后续步骤

通过更改下面列出的技术配置文件，将新的声明添加到社交帐户登录流。 社交/联合帐户登录使用这两个技术配置文件来写入和读取将 alternativeSecurityId 用作用户对象定位符的数据。
```
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```


## <a name="reference"></a>引用

* **技术配置文件 (TP)** 是一个可被视为*函数*的元素类型，定义终结点的名称及其元数据和协议，以及标识体验框架应执行的声明交换的详细信息。  在业务流程步骤中或者通过另一技术配置文件调用此*函数*时，调用方将以参数形式提供 InputClaims 和 OutputClaims。


* 有关扩展属性的完整处理方法，请参阅[目录架构扩展 | 图形 API 的概念](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)
[!NOTE]一文
图形 API 中的扩展属性是使用约定 `extension_ApplicationObjectID_attributename` 命名的。自定义策略将扩展属性称为 extension_attributename，因此将在 XML 中省略 ApplicationObjectId

