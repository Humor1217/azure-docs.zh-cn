---
title: 教程：Azure Active Directory 与 Lifesize Cloud 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Lifesize Cloud 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: b699714a2ab90fd0ad1c2f290681ccdae7aeb1ba
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052187"
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>教程：Azure Active Directory 与 Lifesize Cloud 的集成

在本教程中，了解如何将 Lifesize Cloud 与 Azure Active Directory (Azure AD) 集成。

将 Lifesize Cloud 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Lifesize Cloud
- 可以让用户使用其 Azure AD 帐户自动登录到 Lifesize Cloud（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Lifesize Cloud 的集成，需要以下项：

- Azure AD 订阅
- 已启用 Lifesize Cloud 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Lifesize Cloud
2. 配置和测试 Azure AD 单一登录

## <a name="adding-lifesize-cloud-from-the-gallery"></a>从库中添加 Lifesize Cloud
要配置 Lifesize Cloud 与 Azure AD 的集成，需要从库中将 Lifesize Cloud 添加到托管 SaaS 应用列表。

**若要从库中添加 Lifesize Cloud，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

4. 在搜索框中，键入“Lifesize Cloud”。

    ![创建 Azure AD 测试用户](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. 在结果窗格中，选择“Lifesize Cloud”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 Lifesize Cloud 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Lifesize Cloud 用户。 换句话说，需要建立 Azure AD 用户与 Lifesize Cloud 中相关用户之间的链接关系。

可通过将 Azure AD 中“用户名”的值指定为 Lifesize Cloud 中“用户名”的值来建立此链接关系。

若要配置和测试 Lifesize Cloud 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Lifesize Cloud 测试用户](#creating-a-lifesize-cloud-test-user)** - 在 Lifesize Cloud 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Lifesize Cloud 应用程序中配置单一登录。

**若要配置 Lifesize Cloud 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 Lifesize Cloud 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

2. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. 在“Lifesize Cloud 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“登录 URL”文本框中，使用以下模式键入 URL： `https://login.lifesizecloud.com/ls/?acs`

    b. 在“标识符”文本框中，使用以下模式键入 URL：`https://login.lifesizecloud.com/<companyname>`

     
4. 选中“显示高级 URL 设置”，执行以下步骤：    
   
    ![配置单一登录](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    在“中继状态”文本框中，使用以下模式键入 URL：`https://webapp.lifesizecloud.com/?ent=<identifier>`
   
   > [!NOTE] 
   >请注意，这些不是实际值。 必须使用实际登录 URL、中继状态和标识符更新这些值。 请联系 [Lifesize Cloud 客户端支持团队](https://www.lifesize.com/support)获取登录 URL 值和标识符值，可从本教程之后将介绍的 SSO 配置中获取中继状态值。

4. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. 单击“保存”按钮。

    ![配置单一登录](./media/lifesize-cloud-tutorial/tutorial_general_400.png)

6. 在“Lifesize Cloud 配置”部分，单击“配置 Lifesize Cloud”打开“配置登录”窗口。 从“快速参考”部分中复制“SAML 实体 ID 和 SAML 单一登录服务 URL”。

    ![配置单一登录](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. 若要为应用程序配置 SSO，请使用管理员权限登录到 Lifesize Cloud 应用程序。

8. 在右上角单击你的名字，并单击“高级设置”。
   
    ![配置单一登录](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. 现在，在“高级设置”中，单击“SSO 配置”链接。 这将打开实例的 SSO 配置页。
   
    ![配置单一登录](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. 现在，在 SSO 配置 UI 中配置以下值。    
   
    ![配置单一登录](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“标识提供程序颁发者”文本框中，粘贴从 Azure 门户复制的“SAML 实体 ID”值。

    b.  在“登录 URL”文本框中，粘贴从 Azure 门户复制的“SAML 单一登录服务 URL”值。

    c. 在记事本中打开从 Azure 门户下载的 base-64 编码证书，将其内容复制到剪贴板，然后再粘贴到“X.509 证书”文本框。
  
    d. 在“名字的 SAML 属性映射”文本框中以 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname** 形式输入值
    
    e. 在“姓氏的 SAML 属性映射”文本框中以 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** 形式输入值
    
    f. 在“电子邮件的 SAML 属性映射”文本框中以 **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** 形式输入值

11. 若要检查配置，可以单击“测试”按钮。
   
    >[!NOTE]
    >要想成功测试，需要在 Azure AD 中完成配置向导，还需要向可以执行测试的用户或组提供访问权限。

12. 通过单击“启用 SSO”按钮启用 SSO。

13. 现在，单击“更新”按钮以便保存所有设置。 这会生成 RelayState 值。 复制文本框中生成的 RelayState 值，将其粘贴在“Lifesize Cloud 域和 URL”部分下的“中继状态”文本框中。 

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[ Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/lifesize-cloud-tutorial/create_aaduser_01.png) 

2. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/lifesize-cloud-tutorial/create_aaduser_02.png) 

3. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/lifesize-cloud-tutorial/create_aaduser_03.png) 

4. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/lifesize-cloud-tutorial/create_aaduser_04.png) 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-a-lifesize-cloud-test-user"></a>创建 Lifesize Cloud 测试用户

在本部分中，将在 Lifesize Cloud 中创建一个名为“Britta Simon”的用户。 Lifesize cloud 支持自动化用户预配。 在 Azure AD 中成功进行身份验证后，会自动在应用程序中预配用户。 

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Lifesize Cloud 的权限，允许其使用 Azure 单一登录。

![分配用户][200] 

**要将 Britta Simon 分配到 Lifesize Cloud，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

2. 在应用程序列表中，选择“Lifesize Cloud”。

    ![配置单一登录](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

5. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

6. 在“用户和组”对话框中单击“选择”按钮。

7. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 Lifesize Cloud 磁贴时，应当会登录到 Lifesize Cloud 应用程序页。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/lifesize-cloud-tutorial/tutorial_general_203.png

