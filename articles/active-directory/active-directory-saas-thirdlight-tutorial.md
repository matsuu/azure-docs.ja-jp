---
title: 'チュートリアル: Azure Active Directory と Thirdlight の統合 | Microsoft Docs'
description: Azure Active Directory で Thirdlight を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/11/2016
ms.author: jeedes

---
# チュートリアル: Azure Active Directory と Thirdlight の統合
このチュートリアルでは、Azure と Thirdlight の統合について説明します。このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

* 有効な Azure サブスクリプション
* Thirdlight でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、Thirdlight に割り当てた Azure AD ユーザーは、Thirdlight 企業サイト (サービス プロバイダーが開始したサインオン) で、または「[アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)」の説明に従って、アプリケーションにシングル サインオンできるようになります。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1. Thirdlight のアプリケーション統合の有効化
2. シングル サインオンの構成
3. ユーザー プロビジョニングの構成
4. ユーザーの割り当て

![シナリオ](./media/active-directory-saas-thirdlight-tutorial/IC805836.png "シナリオ")

## Thirdlight のアプリケーション統合の有効化
このセクションでは、Thirdlight のアプリケーション統合を有効にする方法を説明します。

### Thirdlight のアプリケーション統合を有効にするには、次の手順を実行します。
1. Azure クラシック ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。
   
   ![Active Directory](./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Active Directory")
2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。
3. アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。
   
   ![アプリケーション](./media/active-directory-saas-thirdlight-tutorial/IC700994.png "アプリケーション")
4. ページの下部にある **[追加]** をクリックします。
   
   ![アプリケーションの追加](./media/active-directory-saas-thirdlight-tutorial/IC749321.png "アプリケーションの追加")
5. **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。
   
   ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-thirdlight-tutorial/IC749322.png "ギャラリーからのアプリケーションの追加")
6. **検索ボックス**に、「**Thirdlight**」と入力します。
   
   ![アプリケーション ギャラリー](./media/active-directory-saas-thirdlight-tutorial/IC805837.png "アプリケーション ギャラリー")
7. 結果ウィンドウで **[Thirdlight]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。
   
   ![ThirdLight](./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

## シングル サインオンの構成
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで Thirdlight に対する認証を行うことができるようにする方法を説明します。Thirdlight のシングル サインオンを構成するには、証明書からサムプリント値を取得する必要があります。この手順に慣れていない場合は、「[How to retrieve a certificate's thumbprint value (証明書の拇印の値を取得する方法)](http://youtu.be/YKQF266SAxI)」をご覧ください。

### シングル サインオンを構成するには、次の手順に従います。
1. Azure クラシック ポータルの **[Thirdlight]** アプリケーション統合ページで **[シングル サインオンの構成]** をクリックし、**[シングル サインオンの構成]** ダイアログを開きます。
   
   ![Configure Single Sign-On](./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Configure Single Sign-On")
2. **[ユーザーの Thirdlight へのアクセスを設定してください]** ページで、**[Microsoft Azure AD のシングル サインオン]** を選択し、**[次へ]** をクリックします。
   
   ![Configure Single Sign-On](./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Configure Single Sign-On")
3. **[アプリケーション URL の構成]** ページの **[Thirdlight のサインイン URL]** ボックスに、ユーザーが Thirdlight アプリケーションにサインオンする際に使用する URL (例: "http://azuresso2.thirdlight.com/*") を入力し、[次へ] をクリックします。
   
   ![Configure App URL](./media/active-directory-saas-thirdlight-tutorial/IC805841.png "アプリケーション URL の構成")
4. **[Thirdlight でのシングル サインオンの構成]** ページで、メタデータをダウンロードするために、**[メタデータのダウンロード]** をクリックし、コンピューターでメタデータ ファイルをローカルに保存します。
   
   ![Configure Single Sign-On](./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Configure Single Sign-On")
5. 別の Web ブラウザーのウィンドウで、Thirdlight 企業サイトに管理者としてログインします。
6. **[Configuration]、[System Administration]** の順に移動し、**[SAML2]** をクリックします。
   
   ![システム管理](./media/active-directory-saas-thirdlight-tutorial/IC805843.png "システム管理")
7. SAML2 構成セクションで、次の手順を実行します。
   
   ![SAML シングル サインオン](./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML シングル サインオン")
   
   1. **[Enable SAML2 Single Sign-On]** を選択します。
   2. **[Source for IdP Metadata]** として、**[Load IdP Metadata from XML]** を選択します。
   3. ダウンロードしたメタデータ ファイルを開き、内容をコピーしてから、**[IdP Metadata XML]** テキストボックスに貼り付けます。
   4. **[Save SAML2 settings]** をクリックします。
8. Azure クラシック ポータルで、[シングル サインオンの構成の確認] を選択し、**[完了]** をクリックして **[シングル サインオンの構成]** ダイアログを閉じます。
   
   ![Configure Single Sign-On](./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Configure Single Sign-On")

## ユーザー プロビジョニングの構成
Azure AD ユーザーが Thirdlight にログインできるようにするには、そのユーザーを Thirdlight にプロビジョニングする必要があります。Thirdlight の場合、プロビジョニングは手動で行います。

### ユーザー プロビジョニングを構成するには、次の手順に従います。
1. **Thirdlight** 企業サイトに管理者としてログインします。
2. **[Users]** タブに移動します。
3. **[Users and Groups]** を選択します。
4. **[Add new User]** ボタンをクリックします。
5. プロビジョニングする有効な AAD アカウントの **[Username]、[Name or Description]、[Email]、[Choose a Preset or Group of New Members]** を入力します。
6. **[作成]** をクリックします。

> [!NOTE]
> Thirdlight から提供されている他の Thirdlight ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。
> 
> 

## ユーザーの割り当て
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Thirdlight に割り当てるには、次の手順を実行します。
1. Azure クラシック ポータルで、テスト アカウントを作成します。
2. **Thirdlight** アプリケーション統合ページで、**[ユーザーの割り当て]** をクリックします。
   
   ![ユーザーの割り当て](./media/active-directory-saas-thirdlight-tutorial/IC805846.png "ユーザーの割り当て")
3. テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。
   
   ![Yes](./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。アクセス パネルの詳細については、[アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)を参照してください。

<!---HONumber=AcomDC_0914_2016-->