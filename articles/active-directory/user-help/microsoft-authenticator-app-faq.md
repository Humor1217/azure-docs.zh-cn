---
title: 有关 Microsoft Authenticator 应用的帮助 - Azure AD | Microsoft Docs
description: 提供与 Microsoft Authenticator 应用和 Azure 多重身份验证相关的常见问题与解答列表。
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/08/2018
ms.author: lizross
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: d86bc84653e38a9b64a336b8ce9ed7e657129e8c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2018
ms.locfileid: "39058159"
---
# <a name="microsoft-authenticator-app-faq"></a>Microsoft Authenticator 应用常见问题解答

本文解答了有关 Microsoft Authenticator 应用的常见问题。 如果没有看到所提问题的答案，请转到 [Microsoft Authenticator 应用论坛](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp)。 此外，另一篇常见问题解答文章[使用手机登录时的常见问题解答](microsoft-authenticator-app-phone-signin-faq.md)也解答了有关具体应用功能的常见问题。

Microsoft Authenticator 应用替代了 Azure Authenticator 应用，建议使用 Azure 多重身份验证时使用该应用。 Microsoft Authenticator 应用适用于 [Android](https://go.microsoft.com/fwlink/?linkid=866594)、[iOS](https://go.microsoft.com/fwlink/?linkid=866594) 和 [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)。

## <a name="frequently-asked-questions"></a>常见问题

### <a name="what-data-does-the-authenticator-store-on-my-behalf-and-how-can-i-delete-it"></a>哪些数据是 Authenticator 以我的名义存储的，我如何删除它？

Microsoft Authenticator 存储添加帐户时创建的帐户信息。 使用 Authenticator 时，创建诊断日志以进行调试，并存储有用的数据，帮助 Microsoft 诊断任何不可预见的问题。 可以通过依次打开“帮助” > “发送日志” > “查看日志”来访问日志数据。

可以通过删除帐户磁贴来删除数据。 删除帐户磁贴也会删除应用程序正在使用的所有帐户信息，包括日志。 

有关 Microsoft 如何使用客户数据的详细信息，请访问：https://servicetrust.microsoft.com/ViewPage/PrivacyGettingStarted

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>此应用中的代码有哪些用途？ 为什么数字会保持倒计数？

打开 Microsoft 验证器应用时，将看到已添加的帐户以及按每个已添加的帐户列出的六位或八位数字。 你可能会看到倒计时的 30 秒计时器。

登录到帐户时，将使用这些代码。 输入用户名和密码后，可能需要输入验证码。 打开 Microsoft 验证器应用，并复制当前显示的代码。 在登录页中输入该代码，以完成登录。

代码每隔 30 秒更改一次的原因是，使你永远不会使用同一代码两次。 它与你应记住的密码不同。 原理在于只有有权访问你的手机的人才知道你的验证码。

这些验证码不需要 Internet 或数据，因此你不必担心使用电话服务进行登录。 关闭该应用后，该应用不会在后台继续运行，不会耗尽电量。 可以关闭该应用，并在下次登录之前忽略它。  

### <a name="i-only-get-notifications-when-i-have-the-app-open-if-the-app-isnt-open-i-dont-get-any-notifications"></a>仅当应用处于打开状态时才会收到通知。 如果应用未打开，不会收到任何通知。

如果会收到通知，但即使打开振铃，通知也不发出声音或不振动，请先检查应用设置。 让应用对通知使用声音或振动。

如果完全不会收到通知，请检查是否存在以下情况：

- 手机是否进入“免打扰”或“静音”模式？ 在这些模式下，应用可能不会发送通知。
- 是否能从其他应用接收通知？ 如果不能，可能是手机出现网络连接问题或出现 Android/Apple 通知频道问题。 第一种问题可通过手机设置解决，但对于第二个问题，可能需要联系服务提供商获取帮助。
- 是否应用上的某些帐户可以收到通知而其他帐户不能？ 如果是这样的话，从应用中删除有问题的帐户，然后重新添加该帐户以启用推送通知。

如果尝试过这些建议的故障排除方法，但仍有问题，可以向我们发送日志以供诊断。 请转到应用程序设置，然后选择“帮助和反馈”和“发送日志”。 然后，请转到 [ 应用论坛](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp)告诉我们你所遇到的问题以及目前为止你已尝试过步骤。

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>我已使用 Microsoft Authenticator 应用程序生成验证代码。 如何切换到一键式推送通知？
通过推送通知批准登录仅适用于个人 Microsoft 帐户或工作和学校 Microsoft 帐户，并不适用于 Google 或 Facebook 等第三方帐户。 如果拥有工作或学校 Microsoft 帐户，组织可以选择禁用此选项。

如果 Microsoft 帐户使用的是个人帐户，并且想要切换到推送通知，则需要再次添加帐户。 向帐户重新注册该设备，并设置推送通知。  

如果对工作或学校帐户使用 Microsoft Authenticator，则组织决定是否允许一键式通知。

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>一键式推送通知是否适用于非 Microsoft 帐户？
不适用，推送通知只适用于 Microsoft 帐户和 Azure Active Directory 帐户。 如果工作单位或学校使用的是 Azure AD 帐户，则可能会禁用此功能。  

### <a name="i-got-a-new-device-or-restored-my-device-from-a-backup-how-do-i-set-up-my-accounts-in-the-microsoft-authenticator-app-again"></a>我购买了新的设备，或者从备份还原了我的设备。 如何再次在 Microsoft Authenticator 应用中设置我的帐户？
如果运行 iOS 设备，已启用“自动备份”，并且在旧设备上创建了帐户的备份，则可以使用该备份在新设备上恢复帐户凭据。 有关详细信息，请参阅[使用 Microsoft Authenticator 应用备份和恢复帐户凭据](microsoft-authenticator-app-backup-and-recovery.md)一文。 

### <a name="i-lost-my-device-or-moved-on-to-a-new-device-how-do-i-make-sure-notifications-dont-continue-to-go-to-my-old-device"></a>我丢失了设备或者改用了新设备。 如何确保不会继续向旧设备发送通知？  
将 Microsoft Authenticator 应用添加到新 iOS 设备不会自动从旧设备上删除该应用。 从旧设备中删除该应用并不足够。 必须从旧设备中删除该应用，同时告知 Microsoft 或组织忘记旧设备，并从帐户中注销该设备。
- **使用个人 Microsoft 帐户从设备中删除应用。** 转到[帐户安全](https://account.microsoft.com/security) 页的双重验证区域，选择关闭旧设备的验证。  
- **使用工作或学校 Microsoft 帐户从设备中删除应用。** 转到[我的应用](https://myapps.microsoft.com/)页的双重验证区域或转到组织的自定义门户，选择关闭旧设备的验证。 



### <a name="how-do-i-remove-an-account-from-the-app"></a>如何从应用中删除帐户？
* iOS：从主屏幕中，向左滑动帐户磁贴。 选择“删除”。
* Windows Phone：在主屏幕中，依次选择菜单按钮、“**编辑帐户**”。 点击帐户名称旁边的 **X**。
* Android：在主屏幕中，依次选择菜单按钮、“**编辑帐户**”。 点击帐户名称旁边的 **X**。

如果拥有已注册到组织的设备，可能需要完成一个额外步骤才能删除帐户。 在这些设备上，Microsoft Authenticator 应用自动注册为设备管理员。 如果要完全卸载该应用，首先需要在应用设置中取消注册它。

### <a name="why-does-the-app-request-so-many-permissions"></a>应用为什么请求这么多的权限？
下面是可能需要的权限完整列表及它们在应用中的用法。 所见到的特定权限取决于所持有的电话类型。

* **照相机**：在添加工作、学校或非 Microsoft 帐户时用于扫描 QR 码。
* **联系人和电话**：在使用个人 Microsoft 帐户登录时，用于通过查找手机中的现有帐户来简化过程。
* **短信**：用于确保在首次使用个人 Microsoft 帐户登录时， 电话号码与记录中的号码匹配。  我们会将包含 6-8 位数验证码的短信发送到下载应用的手机。 我们不会要求在应用中查找并输入此代码，而是在短信中发送此代码。
* **在其他应用上绘制**：身份验证的通知也会显示在可能正运行的其他任何应用上。
* **从 Internet 接收数据**：此权限是发送通知所必需的。
* **防止手机休眠**：如果向组织注册设备，组织可以更改手机上的这项策略。
* **控制振动**：可以选择在收到验证身份的通知时是否希望手机振动。
* **使用指纹硬件**：每验证身份时，某些工作和学校帐户需要其他的 PIN。 为了使过程更加简单，我们允许使用指纹而不是输入 PIN。
* **查看网络连接**：添加 Microsoft 帐户时，应用需要网络/Internet 连接。
* **读取存储内容**：仅当通过应用设置报告技术问题时才使用此权限。 收集存储中的某些信息来诊断问题。
* **完全网络访问**：此权限是发送通知以验证身份必需的。
* **启动时运行**：如果重新打开手机，此权限可确保继续接收通知以验证身份。

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>为什么 Microsoft Authenticator 应用允许在不解锁设备的情况下批准请求？

同意验证请求时无需解锁设备，因为仅需证明的是你带了你的电话。 双重验证要求提供两项证明：一件你知道的事和一件你拥有的物品。 你只需知道密码。 拥有的物品是手机（已使用 Microsoft Authenticator 应用设置，并已注册为 MFA 证明）。因此，拥有手机和批准请求符合第二个身份验证因素的标准。

### <a name="what-does-the-lock-icon-in-the-account-list-mean"></a>帐户列表中的锁状图标是什么意思？

锁状图标指示设备已在 Azure AD 中注册并注册到帐户下。 在 Microsoft Intune 注册过程中执行适用于 iOS 的设备注册。

## <a name="next-steps"></a>后续步骤

### <a name="contact-us"></a>联系我们
如果此文档没有回答问题，请告知我们。 请转到 [Microsoft Authenticator 应用论坛](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp)发布问题，并从社区获取帮助，或在此页面留下评论。


### <a name="related-topics"></a>相关主题
* 关于 Microsoft 帐户的[双重验证](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification)
* 为工作或学校帐户[进行设置双重验证时遇到问题](multi-factor-authentication-end-user-troubleshoot.md)？
* [使用 Microsoft Authenticator 通过手机登录](microsoft-authenticator-app-phone-signin-faq.md)
