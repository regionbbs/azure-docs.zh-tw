---
title: "教學課程：Azure Active Directory 與 Cherwell 整合 | Microsoft Docs"
description: "了解如何使用 Cherwell 搭配 Azure Active Directory 來啟用單一登入、自動化佈建和更多功能！"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ad891f99-179e-4487-834d-35f3bc01c1ec
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/14/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 70a46193b66142e6be55f4e5fdbe25888c7adfd6


---
# <a name="tutorial-azure-active-directory-integration-with-cherwell"></a>教學課程：Azure Active Directory 與 Cherwell 整合
本教學課程的目的是要示範 Azure 與 Cherwell 的整合。 本教學課程中說明的案例假設您已經具有下列項目：

* 有效的 Azure 訂閱
* 啟用 Cherwell 單一登入的訂用帳戶

完成本教學課程之後，您指派給 Cherwell 的 Azure AD 使用者就能夠單一登入您 Cherwell 公司網站 (服務提供者起始登入) 的應用程式，或是使用 [存取面板簡介](active-directory-saas-access-panel-introduction.md)進行單一登入。

本教學課程中說明的案例由下列建置組塊組成：

1. 啟用 Cherwell 的應用程式整合
2. 設定單一登入
3. 設定使用者佈建
4. 指派使用者

![案例](./media/active-directory-saas-cherwell-tutorial/IC798988.png "Scenario")

## <a name="enabling-the-application-integration-for-cherwell"></a>啟用 Cherwell 的應用程式整合
本節的目的是要說明如何啟用 Cherwell 的應用程式整合。

### <a name="to-enable-the-application-integration-for-cherwell-perform-the-following-steps"></a>若要啟用 Cherwell 的應用程式整合，請執行下列步驟：
1. 在 Azure 傳統入口網站中，按一下左方瀏覽窗格的 [Active Directory] 。
   
   ![Active Directory](./media/active-directory-saas-cherwell-tutorial/IC700993.png "Active Directory")
2. 從 [目錄]  清單中，選取要啟用目錄整合的目錄。
3. 若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]  。
   
   ![應用程式](./media/active-directory-saas-cherwell-tutorial/IC700994.png "Applications")
4. 按一下頁面底部的 [新增]  。
   
   ![新增應用程式](./media/active-directory-saas-cherwell-tutorial/IC749321.png "Add application")
5. 在 [欲執行動作] 對話方塊上，按一下 [從資源庫中新增應用程式]。
   
   ![從組件庫新增應用程式](./media/active-directory-saas-cherwell-tutorial/IC749322.png "Add an application from gallerry")
6. 在**搜尋方塊**中，輸入 **Cherwell**。
   
   ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798989.png "Cherwell")
7. 在結果窗格中，選取 [Cherwell]，然後按一下 [完成] 以新增應用程式。
   
   ## <a name="configuring-single-sign-on"></a>設定單一登入
   ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798996.png "Cherwell")

本節的目的是要說明如何依據 SAML 通訊協定來使用同盟，讓使用者能夠用自己的 Azure AD 帳戶驗證至 Cherwell。

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>若要設定單一登入，請執行下列步驟：
1. 在 Azure 傳統入口網站的 [Cherwell] 應用程式整合頁面上，按一下 [設定單一登入] 來開啟 [設定單一登入] 對話方塊。
   
   ![設定單一登入](./media/active-directory-saas-cherwell-tutorial/IC798990.png "Configure Single Sign-On")
2. 在 [要如何讓使用者登入 Cherwell] 頁面上，選取 [Microsoft Azure AD 單一登入]，然後按 [下一步]。
   
   ![設定單一登入](./media/active-directory-saas-cherwell-tutorial/IC798991.png "Configure Single Sign-On")
3. 在 [設定應用程式 URL]  頁面上，執行下列步驟：
   
   ![設定應用程式 URL](./media/active-directory-saas-cherwell-tutorial/IC798992.png "Configure App URL")
   
   a.  在 [登入 URL] 文字方塊中，輸入您的使用者用來登入 **Cherwell** 的 URL (例如：https://\<公司名稱\>.cherwellondemand.com/cherwellclient)。
   
   b.  依序按一下 [ **下一步**
4. 於 [在 Cherwell 設定單一登入]  頁面上，執行下列步驟：
   
   ![設定單一登入](./media/active-directory-saas-cherwell-tutorial/IC798993.png "Configure Single Sign-On")
   
   a.  按 [下載憑證]，然後將憑證儲存在您的本機電腦中。
   
   b.  複製 [識別提供者 URL]。
   
   c.  複製 [單一登入服務 URL]。
   
   d.  按 [下一步] 。
5. 將下載的憑證、**識別提供者 URL** 和**單一登入服務 URL** 提交給 Cherwell 支援小組。
   
   > [!NOTE]
   > Cherwell 支援小組必須執行實際的 SSO 組態。
   > 當您的訂用帳戶啟用 SSO 之後，您會收到通知。
   > 
   > 
6. 在 Azure 傳統入口網站上，選取單一登入設定確認，然後按一下 [完成] 來關閉 [設定單一登入] 對話方塊。
   
   ![設定單一登入](./media/active-directory-saas-cherwell-tutorial/IC798994.png "Configure Single Sign-On")

## <a name="configuring-user-provisioning"></a>設定使用者佈建
若要讓 Azure AD 使用者可以登入 Cherwell，必須將他們佈建到 Cherwell。  
Cherwell 的使用者帳戶必須由 Cherwell 支援小組建立。

> [!NOTE]
> 您可以使用任何其他的 Cherwell 使用者帳戶建立工具或 Cherwell 提供的 API 來佈建 Azure Active Directory 使用者帳戶。
> 
> 

## <a name="assigning-users"></a>指派使用者
若要測試您的組態，則需指派您所允許使用您應用程式的 Azure AD 使用者，藉此授予其存取組態的權限。

### <a name="to-assign-users-to-cherwell-perform-the-following-steps"></a>若要指派使用者給 Cherwell，請執行下列步驟：
1. 在 Azure 傳統入口網站中建立測試帳戶。
2. 在 [Cherwell] 應用程式整合頁面上，按一下 [指派使用者]。
   
   ![指派使用者](./media/active-directory-saas-cherwell-tutorial/IC798995.png "Assign Users")
3. 選取測試使用者，按一下 [指派]，然後按一下 [是] 以確認指派。
   
   ![是](./media/active-directory-saas-cherwell-tutorial/IC767830.png "Yes")

如果要測試您的單一登入設定，請開啟存取面板。 如需 [存取面板] 的詳細資訊，請參閱 [存取面板簡介](active-directory-saas-access-panel-introduction.md)。




<!--HONumber=Nov16_HO3-->


