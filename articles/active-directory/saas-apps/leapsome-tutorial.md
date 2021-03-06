---
title: 教程：Azure Active Directory 与 Leapsome 集成 | Microsoft Docs
description: 了解如何配置 Azure Active Directory 和 Leapsome 之间的单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cb523e97-add8-4289-b106-927bf1a02188
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.openlocfilehash: 4b2c23745a5e624bcf668dfbfe5d085392d7a583
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052445"
---
# <a name="tutorial-azure-active-directory-integration-with-leapsome"></a>教程：Azure Active Directory 与 Leapsome 集成

本教程介绍了如何将 Leapsome 与 Azure Active Directory (Azure AD) 集成。

将 Leapsome 与 Azure AD 集成具有以下优势：

- 可以在 Azure AD 中控制谁有权访问 Leapsome。
- 可便于用户使用自己的 Azure AD 帐户自动登录 Leapsome（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置将 Azure AD 与 Leapsome 集成，需要有以下各项：

- Azure AD 订阅
- 启用了 Leapsome 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Leapsome
2. 配置和测试 Azure AD 单一登录

## <a name="adding-leapsome-from-the-gallery"></a>从库中添加 Leapsome
若要配置将 Leapsome 集成到 Azure AD，需要将 Leapsome 从库中添加到托管 SaaS 应用程序列表。

**若要从库中添加 Leapsome，请按照以下步骤操作：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

4. 在搜索框中，键入“Leapsome”，从结果面板中选择“Leapsome”，再单击“添加”按钮，以添加应用程序。

    ![结果列表中的“Leapsome”](./media/leapsome-tutorial/tutorial_leapsome_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

本部分以测试用户“Britta Simon”为例，介绍了如何配置和测试 Azure AD 与 Leapsome 单一登录。

为了让单一登录生效，Azure AD 需要知道与 Azure AD 用户对等的 Leapsome 用户。 也就是说，需要在 Azure AD 用户和 Leapsome 相关用户之间建立关联关系。

若要配置和测试 Azure AD 与 Leapsome 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Leapsome 测试用户](#create-a-leapsome-test-user)** - 在 Leapsome 中创建与 Britta Simon 对等的用户，并将它与 Azure AD 用户相关联。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

本部分介绍了如何在 Azure 门户中启用 Azure AD 单一登录，并在 Leapsome 应用程序中配置单一登录。

**若要配置 Azure AD 与 Leapsome 单一登录，请按照以下步骤操作：**

1. 在 Azure 门户中的 Leapsome 应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

2. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![“单一登录”对话框](./media/leapsome-tutorial/tutorial_leapsome_samlbase.png)

3. 在“Leapsome 域和 URL”部分中，若要在 IDP 发起模式下配置应用程序，请按照以下步骤操作：

    ![“Leapsome 域和 URL”单一登录信息](./media/leapsome-tutorial/tutorial_leapsome_url.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“标识符”文本框中，键入一个 URL：`https://www.leapsome.com`

    b. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/assert`

4. 如果要在 SP 发起的模式下配置应用程序，请选中“显示高级 URL 设置”，并执行以下步骤：

    ![“Leapsome 域和 URL”单一登录信息](./media/leapsome-tutorial/tutorial_leapsome_url1.png)

    在“登录 URL”文本框中，使用以下模式键入 URL： `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/login`
     
    > [!NOTE] 
    > 上面的回复 URL 和登录 URL 不是实际值。 将使用实际值更新这些值（本教程稍后将会介绍）。

5. Leapsome 应用程序需要使用特定格式的 SAML 断言。 请为此应用程序配置以下声明。 可以在应用程序集成页的“用户属性”部分管理这些属性的值。 以下屏幕截图显示了示例。
    
    ![配置单一登录](./media/leapsome-tutorial/tutorial_Leapsome_attribute.png)

6. 在“单一登录”对话框的“用户属性”部分中，按上图所示配置 SAML 令牌属性，并执行以下步骤：
    
    | 属性名称 | 属性值 | 命名空间 |
    | ---------------| --------------- | --------- |   
    | 名 | user.givenname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | 姓 | user.surname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | title | user.jobtitle | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | picture | 员工图片的 URL | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

    > [!Note]
    > 图片属性值不是实际值。 请使用实际图片 URL 更新此值。 若要获取此值，请联系 [Leapsome 客户端支持团队](mailto:support@leapsome.com)。
    
    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 单击“添加属性”，打开“添加属性”对话框。

    ![配置单一登录](./media/leapsome-tutorial/tutorial_attribute_04.png)

    ![配置单一登录](./media/leapsome-tutorial/tutorial_attribute_05.png)
    
    b. 在“名称”文本框中，键入为该行显示的属性名称。
    
    c. 在“值”列表中，选择为该行显示的属性值。

    d. 在“命名空间”文本框中，键入相应行的命名空间 URI。
    
    e. 单击“确定”

7. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![证书下载链接](./media/leapsome-tutorial/tutorial_leapsome_certificate.png) 

8. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/leapsome-tutorial/tutorial_general_400.png)
    
9. 在“Leapsome 配置”部分中，单击“配置 Leapsome”，以打开“配置登录”窗口。 从“快速参考”部分中复制“SAML 单一登录服务 URL”

    ![Leapsome 配置](./media/leapsome-tutorial/tutorial_leapsome_configure.png)

10. 在另一个 Web 浏览器窗口中，以安全管理员身份登录 Leapsome。

11. 依次单击右上角的“设置”徽标和“管理员设置”。 

    ![Leapsome 设置](./media/leapsome-tutorial/tutorial_leapsome_admin.png)

12. 单击左侧菜单栏上的“单一登录(SSO)”，并在“基于 SAML 的单一登录(SSO)”页上按照以下步骤操作：
    
    ![Leapsome saml](./media/leapsome-tutorial/tutorial_leapsome_samlsettings.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 选中“启用基于 SAML 的单一登录”。

    b. 将“登录 URL (指引用户开始登录)”值复制并粘贴到 Azure 门户上“Leapsome 域和 URL”部分中的“登录 URL”文本框。

    c. 将“回复 URL (接收标识提供程序的响应)”值复制并粘贴到 Azure 门户上“Leapsome 域和 URL”部分中的“回复 URL”文本框。

    d. 在“SSO 登录 URL (由标识提供程序提供)”文本框中，粘贴从 Azure 门户复制的“SAML 单一登录服务 URL”值。

    e. 将从 Azure 门户下载的证书（不含 --BEGIN CERTIFICATE 和 END CERTIFICATE-- 注释）复制并粘贴到“证书(由标识提供程序提供)”文本框。

    f. 单击“更新 SSO 设置”。
    
### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/leapsome-tutorial/create_aaduser_01.png)

2. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/leapsome-tutorial/create_aaduser_02.png)

3. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/leapsome-tutorial/create_aaduser_03.png)

4. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/leapsome-tutorial/create_aaduser_04.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，并单击“添加引用”。 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。
 
### <a name="create-a-leapsome-test-user"></a>创建 Leapsome 测试用户

本部分介绍了如何在 Leapsome 中创建用户“Britta Simon”。 与 [Leapsome 客户端支持团队](mailto:support@leapsome.com)协作，共同添加需要在 Leapsome 平台中列入白名单的用户或域。 如果域是由团队添加，用户会自动预配到 Leapsome 平台。 使用单一登录前，必须先创建并激活用户。 

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

本部分介绍了如何向 Britta Simon 授予对 Leapsome 访问权限，让其能够使用 Azure 单一登录。

![分配用户角色][200] 

**若要将 Britta Simon 分配到 Leapsome，请按照以下步骤操作：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

2. 在应用程序列表中，选择“Leapsome”。

    ![应用程序列表中的“Leapsome”链接](./media/leapsome-tutorial/tutorial_leapsome_app.png)  

3. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

5. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

6. 在“用户和组”对话框中单击“选择”按钮。

7. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

如果单击访问面板中的“Leapsome”磁贴，应该会自动登录 Leapsome 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/leapsome-tutorial/tutorial_general_01.png
[2]: ./media/leapsome-tutorial/tutorial_general_02.png
[3]: ./media/leapsome-tutorial/tutorial_general_03.png
[4]: ./media/leapsome-tutorial/tutorial_general_04.png

[100]: ./media/leapsome-tutorial/tutorial_general_100.png

[200]: ./media/leapsome-tutorial/tutorial_general_200.png
[201]: ./media/leapsome-tutorial/tutorial_general_201.png
[202]: ./media/leapsome-tutorial/tutorial_general_202.png
[203]: ./media/leapsome-tutorial/tutorial_general_203.png

