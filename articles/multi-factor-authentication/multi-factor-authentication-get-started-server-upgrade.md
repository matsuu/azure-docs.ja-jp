---
title: "PhoneFactor エージェントから Azure Multi-Factor Authentication Server へのアップグレード"
description: "このドキュメントでは、Azure MFA Server を開始する方法と、古い phonefactor エージェントからアップグレードする方法について説明します。"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtland
ms.assetid: 42838ff7-bdf2-4d06-bacc-b3839a00cd76
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/04/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1cd92121b150461698674b8acd4369d09c9b9920


---
# <a name="upgrading-the-phonefactor-agent-to-azure-multifactor-authentication-server"></a>PhoneFactor エージェントから Azure Multi-Factor Authentication Server へのアップグレード
PhoneFactor エージェント v5.x 以前から Azure Multi-factor Authentication Server にアップグレードするには、PhoneFactor エージェントおよび関連するコンポーネントをアンインストールし、その後 Multi-Factor Authentication Server および関連するコンポーネントをインストールする必要があります。

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multifactor-authentication-server"></a>PhoneFactor エージェントから Azure Multi-Factor Authentication Server にアップグレードするには
<ol>
<li>まず、PhoneFactor データ ファイルをバックアップします。 既定のインストール先は C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata です。


<li>ユーザー ポータルがインストールされている場合は、</li>
<ol>
<li>インストール フォルダーに移動し、web.config ファイルをバックアップします。 既定のインストール先は C:\inetpub\wwwroot\PhoneFactor です。</li>


<li>カスタム テーマをポータルに追加した場合は、C:\inetpub\wwwroot\PhoneFactor\App_Themes ディレクトリ以下にカスタム フォルダーをバックアップします。</li>


<li>ユーザー ポータルを PhoneFactor エージェント (PhoneFactor エージェントと同じサーバーにインストールされている場合にのみ使用可能) または Windows の [プログラムと機能] よりアンインストールします。</li></ol>




<li>モバイル アプリ Web サービスがインストールされている場合は、次の手順を実行します。

<ol>
<li>インストール フォルダーに移動し、web.config ファイルをバックアップします。 既定のインストール先は C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService です。</li>
<li>Windows の [プログラムと機能] よりモバイル アプリ Web サービスをアンインストールします。</li></ol>

<li>Web サービス SDK がインストールされている場合、PhoneFactor エージェントまたは Windows の [プログラムと機能] よりアンインストールします。

<li>Windows の [プログラムと機能] より PhoneFactor エージェントをアンインストールします。

<li>Multi-Factor Authentication Server をインストールします。 インストール パスは前回の PhoneFactor エージェントのインストールのレジストリから取得されるため、同じ場所 (C:\Program Files\PhoneFactor) にインストールされます。 新しくインストールする場合は、異なる既定のインストール パス (C:\Program Files\Multi-Factor Authentication Server など) が作成されます。 以前の PhoneFactor エージェントにより残されたデータ ファイルはインストール時にアップグレードされるため、ユーザーおよび設定は新しい Multi-Factor Authentication Server のインストール後もそこに残す必要があります。

<li>プロンプトが表示される場合は、Multi-Factor Authentication Server をアクティブ化し、適切なレプリケーション グループに割り当てられるようにします。

<li>Web サービス SDK が既にインストールされている場合は、Multi-Factor Authentication Server のユーザー インターフェイスより新しい Web サービス SDK をインストールします。 既定の仮想ディレクトリ名は、"PhoneFactorWebServiceSdk" ではなく "MultiFactorAuthWebServiceSdk" であることに注意してください。 以前の名前を使用する場合は、インストール時に仮想ディレクトリ名を変更する必要があります。 そのようにしない場合、インストールで新しい既定の名前の使用を許可する場合は、ユーザー ポータルやモバイル アプリ Web サービスなどの Web サービス SDK を参照する任意のアプリケーションの URL を正しい場所を示すように変更する必要があります。

<li>ユーザー ポータルが既に PhoneFactor エージェント サーバーにインストールされている場合は、Multi-Factor Authentication Server のユーザー インターフェイスより、新しい Multi-Factor Authentication ユーザー ポータルをインストールします。 既定の仮想ディレクトリ名は、"PhoneFactor" ではなく "MultiFactorAuth" であることに注意してください。 以前の名前を使用する場合は、インストール時に仮想ディレクトリ名を変更する必要があります。 そのようにしない場合、インストールで新しい既定の名前の使用を許可する場合は、Multi-Factor Authentication Server の [ユーザー ポータル] アイコンをクリックし、[設定] タブのユーザー ポータル URL を更新する必要があります。

<li>過去にユーザー ポータルまたはモバイル アプリ Web サービスが、PhoneFactor エージェントとは別のサーバーにインストールされていた場合は、次の手順を実行します。

<ol>
<li>インストール場所 (例: C:\Program Files\PhoneFactor) に移動して適切なインストーラーをもう一方のサーバーにコピーします。 ユーザー ポータルとモバイル アプリ Web サービスには、32 ビットおよび 64 ビットのインストーラーがあります。 それぞれ、MultiFactorAuthenticationUserPortalSetupXX.msi と MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi という名前です。</li>
<li>Web サーバー上にユーザー ポータルをインストールするには、コマンド プロンプトを管理者として開き、MultiFactorAuthenticationUserPortalSetupXX.msi を実行します。 既定の仮想ディレクトリ名は、"PhoneFactor" ではなく "MultiFactorAuth" であることに注意してください。 以前の名前を使用する場合は、インストール時に仮想ディレクトリ名を変更する必要があります。 そのようにしない場合、インストールで新しい既定の名前の使用を許可する場合は、Multi-Factor Authentication Server の [ユーザー ポータル] アイコンをクリックし、[設定] タブのユーザー ポータル URL を更新する必要があります。 既存のユーザーに、新しい URL を通知する必要があります。</li>
<li>ユーザー ポータルのインストール先 (C:\inetpub\wwwroot\MultiFactorAuth など) に移動し、web.config ファイルを編集します。 新しい web.config ファイルへのアップグレード前にバックアップされた元の web.config ファイルから、appSettings および applicationSettings セクションの値をコピーします。 Web サービス SDK をインストールするときに新しい既定の仮想ディレクトリ名が保持された場合は、正しい場所を示すように applicationSettings セクションの URL を変更します。 他の任意の既定値が以前の web.config ファイルで変更された場合、その同様の変更を新しい web.config ファイルに適用します。</li>
<li>モバイル アプリ Web サービスを Web サーバーにインストールするには、コマンド プロンプトを管理者として開き、MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi を実行します。 既定の仮想ディレクトリ名は、"PhoneFactorPhoneAppWebService" ではなく "MultiFactorAuthMobileAppWebService" であることに注意してください。 以前の名前を使用する場合は、インストール時に仮想ディレクトリ名を変更する必要があります。 エンドユーザーがモバイル デバイスに簡単に入力できるように、短い名前を選択できます。 そのようにしない場合、インストールで新しい既定の名前の使用を許可する場合は、Multi-Factor Authentication Server の [モバイル アプリ] アイコンをクリックし、モバイル アプリ Web サービス URL を更新する必要があります。</li>
<li>モバイル アプリ Web サービスのインストール先 (C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService など) に移動し、web.config ファイルを編集します。 新しい web.config ファイルへのアップグレード前にバックアップされた元の web.config ファイルから、appSettings および applicationSettings セクションの値をコピーします。 Web サービス SDK をインストールするときに新しい既定の仮想ディレクトリ名が保持された場合は、正しい場所を示すように applicationSettings セクションの URL を変更します。 他の任意の既定値が以前の web.config ファイルで変更された場合、その同様の変更を新しい web.config ファイルに適用します。</li></ol>



<!--HONumber=Nov16_HO2-->


