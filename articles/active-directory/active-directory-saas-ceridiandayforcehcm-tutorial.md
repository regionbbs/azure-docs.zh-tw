---
title: "教學課程：Azure Active Directory 與 Ceridian Dayforce HCM 整合 | Microsoft Docs"
description: "了解如何設定 Azure Active Directory 與 Ceridian Dayforce HCM 之間的單一登入。"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6e0056af13bfa064d740205746a93afeef9b33ab


---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>教學課程：Azure Active Directory 與 Ceridian Dayforce HCM 整合
本教學課程旨在說明如何整合 Ceridian Dayforce HCM 與 Azure Active Directory (Azure AD)。  
Ceridian Dayforce HCM 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 Ceridian Dayforce HCM 的人員
* 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 Ceridian Dayforce HCM (單一登入)
* 您可以在 Azure 傳統入口網站中集中管理您的帳戶

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>必要條件
若要設定 Azure AD 與 Ceridian Dayforce HCM 整合，您需要下列項目：

* Azure AD 訂用帳戶
* 啟用 Ceridian Dayforce HCM 單一登入的訂用帳戶

> [!NOTE]
> 若要測試本教學課程中的步驟，我們不建議使用生產環境。
> 
> 

若要測試本教學課程中的步驟，您應該遵循這些建議：

* 除非必要，否則您不應使用生產環境，。
* 如果您沒有 Azure AD 試用環境，您可以在 [這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月試用。

## <a name="scenario-description"></a>案例描述
此教學課程的目標是讓您在測試環境中測試 Azure AD 單一登入。  
本教學課程中說明的案例由二個主要建置組塊組成：

1. 從資源庫新增 Ceridian Dayforce HCM
2. 設定並測試 Azure AD 單一登入

## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>從資源庫新增 Ceridian Dayforce HCM
若要設定將 Ceridian Dayforce HCM 整合到 Azure AD 中，您需要從資源庫將 Ceridian Dayforce HCM 加入受管理的 SaaS 應用程式清單中。

**若要從資源庫新增 Ceridian Dayforce HCM，請執行下列步驟：**

1. 在 **Azure 傳統入口網站**中，按一下左方瀏覽窗格的 [Active Directory]。 
   
    ![Active Directory][1]
2. 從 [目錄]  清單中，選取要啟用目錄整合的目錄。
3. 若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]  。
   
    ![應用程式][2]
4. 按一下頁面底部的 [新增]  。
   
    ![應用程式][3]
5. 在 [欲執行動作] 對話方塊上，按一下 [從資源庫中新增應用程式]。
   
    ![應用程式][4]
6. 在搜尋方塊中，輸入 **Ceridian Dayforce HCM**。
   
    ![應用程式](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)
7. 在結果窗格中，選取 [Ceridian Dayforce HCM]，然後按一下 [完成] 來新增應用程式。
   
    ![應用程式](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>設定並測試 Azure AD 單一登入
本節的目標是要說明如何以名為 "Britta Simon" 的測試使用者為基礎，設定及測試與 Ceridian Dayforce HCM 搭配運作的 Azure AD 單一登入。

若要讓單一登入運作，Azure AD 必須知道 Ceridian Dayforce HCM 與 Azure AD 中互相對應的使用者。 換句話說，必須在 Azure AD 使用者與 Ceridian Dayforce HCM 中的相關使用者之間建立連結關聯性。

若要設定及測試與 Ceridian Dayforce HCM 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[建立 Azure AD 測試使用者](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
3. **[建立 Ceridian Dayforce HCM 測試使用者](#creating-a-ceridian-dayforce-hcm-test-user)** - 在 Ceridian Dayforce HCM 中建立 Britta Simon 的對應項目，且該項目與 Azure AD 中代表 Britta Simon 的項目連結。
4. **[指派 Azure AD 測試使用者](#assigning-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[Testing Single Sign-On](#testing-single-sign-on)** - 驗證組態是否能運作。

### <a name="configuring-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入
本節的目標是要在 Azure 傳統入口網站中啟用 Azure AD 單一登入，並在您的 Ceridian Dayforce HCM 應用程式中設定單一登入。

Ceridian Dayforce HCM 應用程式需要特定格式的 SAML 判斷提示。 請先與 Dayforce HCM 小組合作來識別正確的使用者識別碼。 Microsoft 建議使用 **"name"** 屬性做為使用者識別碼。 您可以從 [屬性]  索引標籤管理這個屬性的值。 以下螢幕擷取畫面顯示上述的範例。 

![設定單一登入](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**若要設定與 Ceridian Dayforce HCM 搭配運作的 Azure AD 單一登入，請執行下列步驟：**

1. 在 Azure 傳統入口網站中的 **Ceridian Dayforce HCM** 應用程式整合頁面上，按一下頂端功能表中的 [屬性]。
   
    ![設定單一登入](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 
2. 在屬性 **SAML Token 屬性**清單中，選取名稱屬性，然後按一下 [編輯]。
   
    ![設定單一登入](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 
3. 在 [編輯使用者屬性]  對話方塊上，執行下列步驟：
   
    a. 從 [屬性值] 清單中，選取您想要用於實作的使用者屬性。  
    例如，如果您想要使用 EmployeeID 為唯一的使用者識別碼，而且已在 ExtensionAttribute2 中儲存屬性值，則選取 [user.extensionattribute2]。 
   
    b. 按一下頁面底部的 [新增] 。    
4. 按一下頂端功能表中的 [快速啟動] 。
   
    ![設定單一登入](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  
5. 在 Azure 傳統入口網站的 [Ceridian Dayforce HCM] 應用程式整合頁面上，按一下 [設定單一登入]。
   
    ![設定單一登入][6] 
6. 在 [要如何讓使用者登入 Ceridian Dayforce HCM] 頁面上，選取 [Azure AD 單一登入]，然後按 [下一步]。
   
    ![設定單一登入](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 
7. 在 [設定應用程式設定]  對話方塊頁面上，執行下列步驟：
   
    ![設定單一登入](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 

    a. 在 [登入 URL] 文字方塊中，輸入使用者用來登入您 Ceridian Dayforce HCM 應用程式的 URL。 針對生產環境，使用下列 URL 格式︰`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    為了清楚起見，使用您環境的命名空間或貴公司的識別碼來取代 DayforcehcmNamespace，例如︰ `https://sso.dayforcehcm.com/contoso`

    針對測試環境，使用下列 URL 格式︰`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    b.這是另一個 C# 主控台應用程式。 在 [回覆 URL] 文字方塊中，輸入 Azure AD 用來公佈回應的 URL。  
    針對生產環境，請使用︰`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    針對測試環境，請使用︰`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  


1. 在 [設定在 Ceridian Dayforce HCM 單一登入]  頁面上，執行下列步驟：
   
    ![設定單一登入](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 
   
    a. 按一下 [下載憑證]，然後將檔案儲存在您的電腦上。
   
    b. 按 [下一步] 。
2. 若要為您的應用程式設定 SSO，請透過電子郵件連絡您的 Ceridian Dayforce HCM 支援小組，並提供下列資訊：
   
   * 下載的憑證檔案
   * **簽發者 URL**
   * **SAML SSO URL** 
   * **單一登出服務 URL** 
3. 在 Azure 傳統入口網站中，選取單一登入設定確認項目，然後按 [下一步] 。
   
    ![Azure AD 單一登入][10]
4. 在 [單一登入確認] 頁面上，按一下 [完成]。  
   
    ![Azure AD 單一登入][11]

### <a name="creating-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者
本節目標是在 Azure 傳統入口網站中建立名為 Britta Simon 的測試使用者。

![建立 Azure AD 使用者][20]

**若要在 Azure AD 中建立測試使用者，請執行下列步驟：**

1. 在 **Azure 傳統入口網站**中，按一下左方瀏覽窗格的 [Active Directory]。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 
2. 從 [目錄]  清單中，選取要啟用目錄整合的目錄。
3. 若要顯示使用者清單，請按一下頂端功能表的 [使用者] 。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 
4. 若要開啟 [新增使用者] 對話方塊，請按一下底部工具列上的 [新增使用者]。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 
5. 在 [告訴我們這位使用者]  對話方塊頁面上，執行下列步驟：
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 
   
    a. 針對 [使用者類型]，選取 [您組織中的新使用者]。
   
    b. 在 [使用者名稱] 文字方塊中，輸入 **BrittaSimon**。
   
    c. 按 [下一步] 。
6. 在 [使用者設定檔]  對話方塊頁面上，執行下列步驟：
   
   ![建立 Azure AD 測試使用者](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 
   
   a. 在 [名字] 文字方塊中，輸入 **Britta**。  
   
   b. 在 [姓氏] 文字方塊中，輸入 **Simon**。
   
   c. 在 [顯示名稱] 文字方塊中，輸入 **Britta Simon**。
   
   d. 在 [角色] 清單中選取 [使用者]。
   
   e. 按 [下一步] 。
7. 在 [取得暫時密碼] 對話方塊頁面上，按一下 [建立]。
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 
8. 在 [取得暫時密碼]  對話方塊頁面上，執行下列步驟：
   
    ![建立 Azure AD 測試使用者](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 
   
    a. 記下 [新密碼] 的值。
   
    b. 按一下頁面底部的 [新增] 。   

### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>建立 Ceridian Dayforce HCM 測試使用者
本節的目標是要在 Ceridian Dayforce HCM 中建立名為 Britta Simon 的使用者。 

請與 Ceridian Dayforce HCM 支援小組合作，將使用者新增至您的 Ceridian Dayforce HCM 應用程式。 

### <a name="assigning-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者
本節目標是授權 Britta Simon 存取 Ceridian Dayforce HCM，讓她能夠使用 Azure 單一登入。

![指派使用者][200] 

**若要將 Britta Simon 指派給 Ceridian Dayforce HCM，請執行下列步驟：**

1. 在 Azure 傳統入口網站中，若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]  。
   
    ![指派使用者][201] 
2. 在應用程式清單中，選取 [Ceridian Dayforce HCM] 。
   
    ![設定單一登入](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 
3. 在頂端的功能表中，按一下 [使用者] 。
   
    ![指派使用者][203] 
4. 在 [使用者] 清單中，選取 [Britta Simon] 。
5. 在底部的工具列中，按一下 [指派] 。
   
    ![指派使用者][205]

### <a name="testing-single-sign-on"></a>測試單一登入
本節的目標是要使用存取面板來測試您的 Azure AD 單一登入組態。  
當您在存取面板中按一下 [Ceridian Dayforce HCM] 圖格時，應該會自動登入您的 Ceridian Dayforce HCM 應用程式。

## <a name="additional-resources"></a>其他資源
* [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](active-directory-saas-tutorial-list.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


