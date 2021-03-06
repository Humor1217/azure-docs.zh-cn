---
title: 教程：Azure Active Directory 与 Pantheon 集成 | Microsoft 文档
description: 了解如何在 Azure Active Directory 和 Pantheon 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 2f77fedddd84303ae489379a62a3b1845127b16b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051813"
---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a>教程：Azure Active Directory 与 Pantheon 集成

在本教程中，了解如何将 Pantheon 与 Azure Active Directory (Azure AD) 集成。

将 Pantheon 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Pantheon
- 可以让用户通过其 Azure AD 帐户自动登录到 Pantheon（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Pantheon 的集成，需要以下项：

- Azure AD 订阅
- 已启用 Pantheon 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Pantheon
2. 配置和测试 Azure AD 单一登录

## <a name="adding-pantheon-from-the-gallery"></a>从库中添加 Pantheon
要配置 Pantheon 与 Azure AD 的集成，需要从库中将 Pantheon 添加到托管 SaaS 应用列表。

**若要从库中添加 Pantheon，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

4. 在搜索框中，键入“Pantheon”。

    ![创建 Azure AD 测试用户](./media/pantheon-tutorial/tutorial_pantheon_search.png)

5. 在结果面板中，选择“Pantheon”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 Pantheon 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Pantheon 用户。 换句话说，需要在 Azure AD 用户与 Pantheon 中相关用户之间建立链接关系。

可通过将 Azure AD 中“用户名”的值指定为 Pantheon 中“用户名”的值来建立此关联关系。

若要配置并测试 Pantheon 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. [创建 Pantheon 测试用户](#creating-a-pantheon-test-user) - 在 Pantheon 中有一个与 Azure AD 中的 Britta Simon 相对应的关联用户。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Pantheon 应用程序中配置单一登录。

**若要配置 Pantheon 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 Pantheon 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

2. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/pantheon-tutorial/tutorial_pantheon_samlbase.png)

3. 在“Pantheon 域和 URL”部分，执行以下步骤：

    ![配置单一登录](./media/pantheon-tutorial/tutorial_pantheon_url.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“标识符”文本框中，使用以下模式键入 URL：`urn:auth0:pantheon:<orgname>-SSO`

    b. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`

    > [!NOTE] 
    > 这些不是实际值。 请使用实际标识符和回复 URL 更新这些值。 请联系 [Pantheon 支持团队](https://pantheon.io/docs/getting-support/)获取这些值。

4. Pantheon 应用程序需要特定格式的 SAML 断言，这就需要使用用户的电子邮件地址设置 UserIdentifier 属性值。 默认情况下，Azure AD 将 UserPrincipalName 用于 UserIdentifier 属性。 但为了成功集成，需要将此值调整为与用户的电子邮件地址匹配。 只有在完成相应的映射后，集成才有效。

    ![配置单一登录](./media/pantheon-tutorial/tutorial_attribute.png)    


5. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/pantheon-tutorial/tutorial_pantheon_certificate.png)

6. 单击“保存”按钮。

    ![配置单一登录](./media/pantheon-tutorial/tutorial_general_400.png)

7. 在“Pantheon 配置”部分中，单击“配置 Pantheon”打开“配置登录”窗口。 从“快速参考”部分中复制“SAML 单一登录服务 URL”

    ![配置单一登录](./media/pantheon-tutorial/tutorial_pantheon_configure.png) 

8. 若要在 Pantheon 端配置单一登录，需要将下载的证书和 SAML 单一登录服务 URL 发送给 [Pantheon 支持团队](https://pantheon.io/docs/getting-support/)。

     > [!Note]
     > 还需要提供电子邮件域信息和要启用此连接的日期时间。 可以从[此处](https://pantheon.io/docs/sso-organizations/)找到更多详细信息

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[ Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/pantheon-tutorial/create_aaduser_01.png) 

2. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/pantheon-tutorial/create_aaduser_02.png) 

3. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/pantheon-tutorial/create_aaduser_03.png) 

4. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/pantheon-tutorial/create_aaduser_04.png) 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-pantheon-test-user"></a>创建 Pantheon 测试用户

在本部分中，会在 Pantheon 中创建一个名为“Britta Simon”的用户。 请按照以下步骤将用户添加到 Pantheon 中。 

>[!NOTE] 
>要使 SSO 正常工作，需要先在 Pantheon 中创建用户。

1. 使用管理员凭据登录到 Pantheon。

2. 导航到“组织”仪表板页。
 
3. 单击“人员”。

4. 单击“添加用户”。

5. 输入用户的电子邮件地址。

6. 选择用户的角色。

7. 单击“添加用户”。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Pantheon 的权限，允许她使用 Azure 单一登录。

![分配用户][200] 

**要将 Britta Simon 分配到 Pantheon，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

2. 在应用程序列表中，选择“Pantheon”。

    ![配置单一登录](./media/pantheon-tutorial/tutorial_pantheon_app.png) 

3. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

5. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

6. 在“用户和组”对话框中单击“选择”按钮。

7. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的“Pantheon”磁贴时，用户应自动登录到 Pantheon 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/pantheon-tutorial/tutorial_general_01.png
[2]: ./media/pantheon-tutorial/tutorial_general_02.png
[3]: ./media/pantheon-tutorial/tutorial_general_03.png
[4]: ./media/pantheon-tutorial/tutorial_general_04.png

[100]: ./media/pantheon-tutorial/tutorial_general_100.png

[200]: ./media/pantheon-tutorial/tutorial_general_200.png
[201]: ./media/pantheon-tutorial/tutorial_general_201.png
[202]: ./media/pantheon-tutorial/tutorial_general_202.png
[203]: ./media/pantheon-tutorial/tutorial_general_203.png

