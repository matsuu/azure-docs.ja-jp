---
title: IIS の Blob Storage とイベントの Table Storage の使用 | Microsoft Docs
description: Log Analytics は、Table Storage に診断情報を出力する Azure サービスのログと Blob Storage に出力された IIS のログを読み取ることができます。
services: log-analytics
documentationcenter: ''
author: bandersmsft
manager: jwhit
editor: ''

ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: banders

---
# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>IIS の Blob Storage とイベントの Table Storage の使用
Log Analytics は、Table Storage に診断情報を出力する以下のサービスのログと Blob Storage に出力された IIS のログを読み取ることができます。

* Service Fabric クラスター (プレビュー)
* Virtual Machines
* Web/worker ロール

Log Analytics でこれらのリソースのデータを収集する前に、Azure 診断を有効にする必要があります。

診断が有効になっていれば、Log Analytics でログを収集するための構成を Azure Portal または PowerShell で行うことができます。

Azure 診断は、Azure で実行している worker ロール、Web ロール、または仮想マシンから診断データを収集できる Azure の拡張機能です。 そのデータは Azure Storage アカウントに保存され、Log Analytics から収集することができます。

Log Analytics でこれらの Azure 診断ログを収集するためには、ログが次の場所に存在している必要があります。

| ログの種類 | リソースの種類 | 場所 |
| --- | --- | --- |
| IIS ログ |Virtual Machines <br> Web ロールの比較 <br> Worker ロール |wad-iis-logfiles (Blob Storage) |
| syslog |Virtual Machines |LinuxsyslogVer2v0 (Table Storage) |
| Service Fabric の操作イベント |Service Fabric ノード |WADServiceFabricSystemEventTable |
| Service Fabric Reliable Actor のイベント |Service Fabric ノード |WADServiceFabricReliableActorEventTable |
| Service Fabric Reliable Service のイベント |Service Fabric ノード |WADServiceFabricReliableServiceEventTable |
| Windows イベント ログ |Service Fabric ノード <br> Virtual Machines <br> Web ロールの比較 <br> Worker ロール |WADWindowsEventLogsTable (Table Storage) |
| Windows ETW のログ |Service Fabric ノード <br> Virtual Machines <br> Web ロールの比較 <br> Worker ロール |WADETWEventTable (Table Storage) |

> [!NOTE]
> 現在、Azure Websites からの IIS ログはサポートされていません。
> 
> 

仮想マシンについては、 [Microsoft Monitoring Agent](http://go.microsoft.com/fwlink/?LinkId=517269) をインストールして追加のインサイトを有効にすることもできます。 これにより、IIS ログとイベント ログを分析できるほか、構成の変更の追跡、SQL の評価、更新の評価などの追加の分析を実行することもできます。

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>イベント ログと IIS ログの収集のために仮想マシンで Azure 診断を有効にする
イベント ログと IIS ログを収集するために、Microsoft Azure の管理ポータルを使用して、仮想マシンで Azure 診断を有効にするには、以下の手順を実行します。

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-management-portal"></a>Azure 管理ポータルを使用して仮想マシンで Azure 診断を有効にするには
1. 仮想マシンを作成する際に、VM エージェントをインストールします。 仮想マシンが既に存在する場合は、VM エージェントが既にインストールされていることを確認します。
   
   * Azure 管理ポータルで仮想マシンに移動し、**[オプションの構成]**、**[診断]** の順に選択して、**[状態]** を **[オン]** に設定します。
     
     完了すると、診断データを収集する Azure 診断の拡張機能が VM に自動的にインストールされ、実行されます。
2. 既存の VM で監視を有効にし、イベント ログを構成します。 診断は VM レベルで有効にすることができます。 診断を有効にして、イベント ログを構成するには、次の手順を実行します。
   
   1. VM を選択します。
   2. **[監視]**をクリックします。
   3. **[診断]**をクリックします。
   4. **[状態]** を **[オン]** に設定します。
   5. 収集する各診断ログを選択します。
   6. **[OK]**をクリックします。

Azure PowerShell を使用すると、Azure Storage に書き込むイベントをより細かく指定できます。 [テーブル ストレージに書き込まれた Azure 診断または BLOB に書き込まれた IIS ログを使用してデータを収集する](log-analytics-azure-storage-json.md)方法についてのページを参照してください。

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>IIS ログとイベントの収集のために Web ロールで Azure 診断を有効にする
[クラウド サービスの診断を有効にする方法](../cloud-services/cloud-services-dotnet-diagnostics.md)のページを参照してください。 ここでは、そのトピックから基本的な情報を流用し、Log Analytics を使用するために手順をカスタマイズします。

診断を有効にした場合

* 既定では、IIS ログは scheduledTransferPeriod の転送間隔で転送されたログ データと共に格納されます。
* 既定では、Windows イベント ログは転送されません。

### <a name="to-enable-diagnostics"></a>診断を有効にするには
Windows イベント ログを有効にするか、または scheduledTransferPeriod を変更するには、XML 構成ファイル (diagnostics.wadcfg) を使用して Azure 診断を構成します。手順については、「[手順 4. 診断構成ファイルを作成して拡張機能をインストールする](../cloud-services/cloud-services-dotnet-diagnostics.md)」を参照してください。

次の構成ファイルの例では、IIS ログと、アプリケーション ログとシステム ログからすべてのイベントを収集します。

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

次の例のように、ConfigurationSettings に必ずストレージ アカウントを指定してください。

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

**AccountName** および **AccountKey** の値は、Azure 管理ポータルのストレージ アカウント ダッシュボードの [アクセス キーの管理] に表示されます。 接続文字列のプロトコルは **https**である必要があります。

更新された診断の構成がクラウド サービスに適用され、Azure Storage に診断データが書き込まれていれば、Log Analytics を構成する準備が整っています。

## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Azure Portal を使用して Azure Storage からログを収集する
以下の Azure サービスのログを Log Analytics で収集するための構成は Azure Portal で行うことができます。

* Service Fabric クラスター
* Virtual Machines
* Web/worker ロール

Azure Portal で Log Analytics ワークスペースに移動し、次の作業を行います。

1. *[Storage accounts logs (ストレージ アカウントのログ)]* をクリックします。
2. *[追加]* タスクをクリックします。
3. 診断ログが格納されているストレージ アカウントを選択します。
   * これには、クラシック ストレージ アカウントと Azure Resource Manager ストレージ アカウントとがあります。
4. ログの収集対象となるデータの種類を選択します。
   * IIS ログ、イベント、Syslog (Linux)、ETW ログ、Service Fabric イベントのいずれかになります。
5. ソースの値はデータの種類に応じて自動的に設定され、変更することはできません。
6. [OK] をクリックして構成を保存します。

Log Analytics で収集する他のストレージ アカウントとデータの種類についても手順 2. ～ 6. を繰り返します。

約 30 分経過すれば、ストレージ アカウントのデータを Log Analytics で確認できるようになります。 表示されるのは、構成の適用後にストレージに書き込まれたデータだけです。 Log Analytics は、過去のデータをさかのぼってストレージ アカウントから読み取ることはしません。

> [!NOTE]
> ストレージ アカウントにソースが存在するかどうかや、新しいデータが書き込まれているかどうかの確認は、ポータルでは実行されません。
> 
> 

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>イベント ログと IIS ログの収集のために PowerShell を使用して仮想マシンで Azure 診断を有効にする
Azure PowerShell を使用すると、Azure Storage に書き込むイベントをより細かく指定できます。
詳細については、「 [Azure Virtual Machines での診断の有効化](../virtual-machines-dotnet-diagnostics.md) 」を参照してください。

次の PowerShell スクリプトを使用して、エージェントを有効にして更新することができます。
このスクリプトでカスタム ログ構成を使用することもできます。
その場合、スクリプトを変更して、ストレージ アカウント、サービス名、仮想マシンの名前を設定します。
従来の仮想マシン用のコマンドレットを使用するスクリプトです。

次のサンプル スクリプトを確認、コピーして、必要に応じて変更し、PowerShell スクリプト ファイルとして保存します。その後、スクリプトを実行します。

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```

## <a name="next-steps"></a>次のステップ
* JSON 形式の診断情報を Blob Storage に出力する Azure サービスからのログを [Blob Storage 内の JSON ファイルを使用](log-analytics-azure-storage-json.md)して読み取ります。
* [ソリューションを有効](log-analytics-add-solutions.md) にして、データに対する洞察を得ます。
* [検索クエリを使用](log-analytics-log-searches.md) して、データを分析します。

<!--HONumber=Oct16_HO2-->


