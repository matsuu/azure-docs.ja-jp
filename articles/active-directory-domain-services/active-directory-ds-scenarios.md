---
title: 'Azure Active Directory Domain Services: デプロイ シナリオ | Microsoft Docs'
description: Azure AD ドメイン サービスのデプロイ シナリオ
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand

ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2016
ms.author: maheshu

---
# <a name="deployment-scenarios-and-use-cases"></a>デプロイ シナリオおよびユース ケース
このセクションでは、Azure Active Directory (AD) ドメイン サービスから恩恵を受けられるいくつかのシナリオとユース ケースについて説明します。

## <a name="secure,-easy-administration-of-azure-virtual-machines"></a>Azure 仮想マシンの安全で簡単な管理
Azure Active Directory Domain Services を使用して、Azure 仮想マシンを効率的に管理できます。 Azure 仮想マシンは管理対象ドメインに参加できるため、企業の AD の資格情報を使用してログインできるようになります。 この方法により、各 Azure 仮想マシンのローカル管理者アカウントの管理などの、資格情報の管理に関する面倒を回避できます。

管理対象ドメインに参加しているサーバー仮想マシンは、グループ ポリシーを使用してセキュリティ保護することもできます。 Azure 仮想マシンに必要なセキュリティ基準を適用し、企業のセキュリティ ガイドラインに従ってロックダウンできます。 たとえば、グループ ポリシー管理機能を使用して、これらの仮想マシンで起動できるアプリケーションの種類を制限できます。

![Azure 仮想マシンの効率的な管理](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

サーバーおよびその他のインフラストラクチャが寿命に達するので、Contoso では現在オンプレミスでホストされている多くのアプリケーションをクラウドに移行しています。 Contoso における現在の IT 標準では、企業アプリケーションをホストするサーバーはドメインに参加し、グループ ポリシーを使用して管理される必要があると定められています。 Contoso の IT 管理者は、管理を容易にするために、Azure にデプロイされた仮想マシンをドメインに参加させることを望んでいます。 その結果、管理者とユーザーは、企業の資格情報を使用してログインできます。 同時に、グループ ポリシーを使用して、必要なセキュリティ基準に準拠するようにマシンを構成できます。 Contoso では、Azure の仮想マシンを保護するために Azure でドメイン コントローラーのデプロイ、監視、および管理を行う必要はないと考えています。 したがって、Azure AD ドメイン サービスは、このユースケースに非常に適しています。

**デプロイに関する注意事項**

このデプロイ シナリオについて、次の重要事項を考慮してください。

* Azure AD ドメイン サービスによって提供される管理対象ドメインでは、デフォルトで単一のフラットな OU (組織単位) 構造が提供されます。 すべてのドメインに参加しているコンピューターは、単一のフラットな OU に存在します。 ただし、カスタムの OU を作成することもできます。
* Azure AD ドメイン サービスは、ユーザーおよびコンピューターに対してそれぞれ組み込み GPO の形式で単純なグループ ポリシーをサポートします。 OU/部門ごとに GP をターゲットとすることも、WMI フィルター処理を実行することも、カスタムの GPO を作成することもできません。
* Azure AD ドメイン サービスでは、基本の AD コンピューター オブジェクト スキーマをサポートします。 コンピューター オブジェクトのスキーマを拡張することはできません。

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>LDAP バインド認証を使用するオンプレミスのアプリケーションをリフト アンド シフト方式で Azure インフラストラクチャ サービスに移行する
![LDAP バインド](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso には何年も前に ISV から購入したオンプレミスのアプリケーションがあります。 アプリケーションは現在、ISV によってメンテナンス モードにされています。アプリケーションに対する変更を依頼することは Contoso にとって非常にコスト高です。 このアプリケーションは Web ベースのフロントエンドを備えています。このフロントエンドでは Web 形式を使用してユーザーの資格情報を収集し、次に企業の Active Directory への LDAP バインドを実行してユーザーを認証します。 Contoso では、このアプリケーションを Azure インフラストラクチャ サービスに移行したいと考えています。 アプリケーションは、変更を必要とせずに、そのまま動作するのが望ましいです。 さらに、ユーザーが既存の企業資格情報を使用して認証を受けることができるようにし、ユーザーに異なる操作方法を再トレーニングしなくても済むようにします。 すなわち、エンドユーザーはアプリケーションが実行されている場所を気にする必要はなく、移行はエンドユーザーに対して透過的である必要があります。

**デプロイに関する注意事項**

このデプロイ シナリオについて、次の重要事項を考慮してください。

* アプリケーションがディレクトリへの変更/書き込みを行う必要がないことを確認します。 Azure AD ドメイン サービスによって提供される管理対象ドメインへの LDAP 書き込みアクセスはサポートされていません。
* 管理対象ドメインに対してパスワード変更を直接行うことはできません。 エンド ユーザーがパスワードを変更するには、Azure AD のセルフサービス パスワード変更メカニズムを使用するか、オンプレミスのディレクトリに対してパスワード変更を行います。 これらの変更は自動的に同期され、管理対象ドメイン内で使用できるようになります。

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>LDAP 読み取りを使用してディレクトリにアクセスするオンプレミスのアプリケーションをリフト アンド シフト方式で Azure インフラストラクチャ サービスに移行する
Contoso には、約 10 年前に開発されたオンプレミスの基幹業務 (LOB) アプリケーションがあります。 このアプリケーションは、ディレクトリ対応のアプリケーションであり、Windows Server AD と連携して動作するように設計されています。 アプリケーションでは、LDAP (ライトウェイト ディレクトリ アクセス プロトコル) を使用して、Active Directory からユーザーに関する情報と属性を読み取ります。 アプリケーションは属性の変更もディレクトリへの書き込みも行いません。 Contoso では、このアプリケーションを Azure インフラストラクチャ サービスに移行し、現在このアプリケーションをホストしている古くなったオンプレミスのハードウェアを廃止したいと考えています。 REST ベースの Azure AD Graph API など、最新のディレクトリ API を使用するためにアプリケーションを書き直すことはできません。 したがって、リフト アンド シフト オプションが必要となります。このオプションを使用すれば、コードの変更またはアプリケーションの書き直しを行うことなく、クラウドで実行されるようにアプリケーションを移行することができます。

**デプロイに関する注意事項**

このデプロイ シナリオについて、次の重要事項を考慮してください。

* アプリケーションがディレクトリへの変更/書き込みを行う必要がないことを確認します。 Azure AD ドメイン サービスによって提供される管理対象ドメインへの LDAP 書き込みアクセスはサポートされていません。
* アプリケーションがカスタム/拡張型の Active Directory スキーマを必要としないことを確認します。 Azure AD ドメイン サービスでは、スキーマの拡張機能はサポートされていません。

## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>オンプレミスのサービスまたはデーモン アプリケーションを Azure インフラストラクチャ サービスに移行する
一部のアプリケーションは複数の層で構成されますがそのうち 1 つの層では、データベース層などのバックエンド層に対して認証された呼び出しを実行する必要があります。 Active Directory のサービス アカウントはこれらのユース ケースでよく使用されます。 これらのアプリケーションを、リフト アンド シフト方式で Azure インフラストラクチャ サービスに移行し、これらのアプリケーションの ID のニーズに対して Azure の AD ドメイン サービスを使用することができます。 オンプレミスのディレクトリから Azure AD に同期されているサービス アカウントと同一のサービス アカウントを使用できます。 代わりに、カスタムの OU をまず作成し、その OU 内でその OU 内の個別のサービス アカウントを作成して、そのようなアプリケーションをデプロイすることもできます。

![WIA を使用したサービス アカウント](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso には、Web フロントエンド、SQL Server、およびバックエンド FTP サーバーを含むカスタム作成のソフトウェア資格情報コンテナー アプリケーションがあります。 サービス アカウントの Windows 統合認証は、FTP サーバーに対する Web フロントエンドの認証に使用されます。 Web フロント エンドは、サービス アカウントとして実行されるように設定されています。 バックエンド サーバーは Web フロント エンドのサービス アカウントからのアクセスを承認するように構成されています。 Contoso は、このアプリケーションを Azure インフラストラクチャ サービスに移動するためにクラウドでドメイン コント ローラー仮想マシンをデプロイする必要はないとしています。 Contoso の IT 管理者は、Web フロントエンドをホストするサーバー、SQL Server、および FTP サーバーを Azure 仮想マシンにデプロイできます。 その後、これらの仮想マシンを Azure AD ドメイン サービスで管理されているドメインに 参加させることができます。 次に、オンプレミスのディレクトリにある同じサービス アカウントを、アプリの認証に使用できます。 このサービス アカウントは Azure AD ドメイン サービスで管理されていドメインに同期されており、使用可能です。

**デプロイに関する注意事項**

このデプロイ シナリオについて、次の重要事項を考慮してください。

* アプリケーションが認証でユーザー名とパスワードを使用していることを確認します。 証明書/スマート カード ベースの認証は、Azure AD ドメイン サービスでサポートされていません。
* 管理対象ドメインに対してパスワード変更を直接行うことはできません。 エンド ユーザーがパスワードを変更するには、Azure AD のセルフサービス パスワード変更メカニズムを使用するか、オンプレミスのディレクトリに対してパスワード変更を行います。 これらの変更は自動的に同期され、管理対象ドメイン内で使用できるようになります。

## <a name="azure-remoteapp"></a>Azure RemoteApp
Azure RemoteApp は、ドメイン参加コレクションを作成する Contoso の管理者を有効にします。 この機能により、Azure RemoteApp で処理されるリモート アプリケーションがドメイン参加コンピューターで実行されたり、Windows 統合認証を使用して他のリソースにアクセスしたりできるようになります。 Contoso は Azure AD ドメイン サービスを使用して、Azure RemoteApp ドメイン参加コレクションによって使用される管理対象ドメインを提供することができます。

![Azure RemoteApp](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

このデプロイ シナリオの詳細については、「 [Lift-and-shift your workloads with Azure RemoteApp and Azure AD Domain Services (Azure RemoteApp と Azure AD ドメイン サービスを使用したワークロードのリフト アンド シフト)](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx)」というタイトルのリモート デスクトップ サービスのブログを参照してください。

<!--HONumber=Oct16_HO2-->


