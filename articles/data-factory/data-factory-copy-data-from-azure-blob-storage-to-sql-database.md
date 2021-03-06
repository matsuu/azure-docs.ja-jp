---
title: Blob Storage から SQL Database へのデータのコピー | Microsoft Docs
description: このチュートリアルでは、Azure Data Factory パイプラインでコピー アクティビティを使用して、Blob Storage から SQL Database にデータをコピーする方法を示します。
keywords: BLOB SQL, Blob Storage, データのコピー
services: data-factory
documentationcenter: ''
author: spelluru
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: spelluru

---
# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Data Factory を使用した Blob Storage から SQL Database へのデータのコピー
> [!div class="op_single_selector"]
> * [概要と前提条件](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [コピー ウィザード](data-factory-copy-data-wizard-tutorial.md)
> * [Azure ポータル](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager テンプレート](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

このチュートリアルでは、BLOB ストレージから SQL Database にデータをコピーするパイプラインを備えたデータ ファクトリを作成します。

コピー アクティビティにより、Azure Data Factory でデータ移動が実行されます。 このアクティビティは、安全で信頼性の高いスケーラブルな方法によってさまざまなデータ ストア間でデータをコピーできる、グローバルに利用可能なサービスによって動作します。 コピー アクティビティの詳細については、「 [データ移動アクティビティ](data-factory-data-movement-activities.md) 」をご覧ください。  

> [!NOTE]
> Data Factory サービスの詳細については、 [Azure Data Factory サービスの概要](data-factory-introduction.md) に関する記事をご覧ください。
> 
> 

## <a name="prerequisites-for-the-tutorial"></a>このチュートリアルの前提条件
このチュートリアルを読み始める前に、次の項目を用意する必要があります。

* **Azure サブスクリプション**。  サブスクリプションがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、 [無料試用版](http://azure.microsoft.com/pricing/free-trial/) のページを参照してください。
* **Azure ストレージ アカウント**。 このチュートリアルでは、BLOB ストレージを **ソース** データ ストアとして使用します。 Azure ストレージ アカウントがない場合、ストレージ アカウントの作成手順については、「 [ストレージ アカウントの作成](../storage/storage-create-storage-account.md#create-a-storage-account) 」をご覧ください。
* **Azure SQL データベース**。 このチュートリアルでは、Azure SQL Database を **コピー先** データ ストアとして使用します。 このチュートリアルで使用できる Azure SQL Database がない場合の作成方法については、「 [Azure SQL Database を作成して構成する方法](../sql-database/sql-database-get-started.md) 」を参照してください。
* **SQL Server 2012/2014 または Visual Studio 2013**。 サンプル データベースを作成し、結果データをデータベースに表示するには、SQL Server Management Studio または Visual Studio を使用します。  

## <a name="collect-blob-storage-account-name-and-key"></a>BLOB ストレージ アカウントの名前とキーを収集する
このチュートリアルを実行するには、Azure ストレージ アカウントのアカウント名とアカウント キーが必要です。 Azure ストレージ アカウントの**アカウント名**と**アカウント キー**をメモしておきます。

1. [Azure ポータル](https://portal.azure.com/)にログインします。
2. 左側のメニューで **[More services (その他のサービス)]** を選択し、**[ストレージ アカウント]** を選択します。
   
    ![参照 - ストレージ アカウント](media\\data-factory-copy-data-from-azure-blob-storage-to-sql-database\\browse-storage-accounts.png)
3. **[ストレージ アカウント]** ブレードで、このチュートリアルで使用する **Azure ストレージ アカウント**を選択します。
4. **[設定]** にある **[アクセス キー]** リンクを選択します。
5. **[ストレージ アカウント名]** テキスト ボックスの隣にある (イメージの) **[コピー]** ボタンをクリックし、任意の場所 (たとえばテキスト ファイル) に貼り付けて保存します。
6. **key1**についても、前のコピー手順を繰り返すか、メモしておきます。
   
    ![スストレージ アクセス キー](media\\data-factory-copy-data-from-azure-blob-storage-to-sql-database\\storage-access-key.png)
7. **[X]**をクリックしてすべてのブレードを閉じます。

## <a name="collect-sql-server,-database,-user-names"></a>SQL サーバー、データベース、ユーザーの名前を収集する
このチュートリアルを実行するには、Azure SQL サーバー名、データベース名、ユーザー名が必要です。 Azure SQL Database の**サーバー**、**データベース**、**ユーザー**の名前をメモしておきます。

1. **Azure Portal** で、左側にある **[More services (その他のサービス)]** をクリックし、**[SQL データベース]** を選択します。
2. **[SQL データベース]** ブレードで、このチュートリアルで使用する**データベース**を選択します。 **データベース名**をメモしておきます。  
3. **[SQL データベース]** ブレードで、**[設定]** の **[プロパティ]** をクリックします。
4. **[サーバー名]** と **[サーバー管理ログイン]** の値をメモしておきます。
5. **[X]**をクリックしてすべてのブレードを閉じます。

## <a name="allow-azure-services-to-access-sql-server"></a>Azure サービスに SQL サーバーへのアクセスを許可する
Data Factory サービスから Azure SQL サーバーにアクセスできるように、Azure SQL サーバーで **[Azure サービスへのアクセスを許可する]** の設定が**オン**になっていることを確認します。 この設定を確認して有効にするには、次の手順を実行します。

1. 左側にある **[More services (その他のサービス)]** ハブをクリックし、**[SQL サーバー]** をクリックします。
2. サーバーを選択し、**[設定]** の **[ファイアウォール]** をクリックします。 
3. **[ファイアウォールの設定]** ブレードの **[Azure サービスへのアクセスを許可する]** で **[オン]** をクリックします。
4. **[X]**をクリックしてすべてのブレードを閉じます。

## <a name="prepare-blob-storage-and-sql-database"></a>Blob Storage と SQL Database を準備する
ここからは、次の手順を実行して、チュートリアルで使用する Azure Blob Storage と Azure SQL Database を準備します。  

1. メモ帳を起動し、次のテキストを貼り付け、**emp.txt** という名前でハード ドライブの **C:\ADFGetStarted** フォルダーに保存します。
   
        John, Doe
        Jane, Doe
2. [Azure Storage エクスプローラー](https://azurestorageexplorer.codeplex.com/)などのツールを使用して **adftutorial** コンテナーを作成し、このコンテナーに **emp.txt** ファイルをアップロードします。
   
    ![Azure Storage Explorer. Copy data from Blob storage to SQL database](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. 次の SQL スクリプトを使用して、 **emp** テーブルを Azure SQL Database に作成します。  

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **SQL Server 2012/2014 がコンピューターにインストールされている場合** : 記事「 [SQL Server Management Studio を使用した Azure SQL Database の管理」の「手順 2. SQL Database への接続](../sql-database/sql-database-manage-azure-ssms.md#Step2) 」に従い、Azure SQL サーバーに接続して SQL スクリプトを実行します。 この記事では、[新しい Azure Portal](https://portal.azure.com) ではなく、[Azure クラシック ポータル](http://manage.windowsazure.com)を使用して、Azure SQL サーバーのファイアウォールを構成します。

    クライアントから Azure SQL サーバーへのアクセスが許可されていない場合は、コンピューター (IP アドレス) からのアクセスを許可するように、Azure SQL サーバーのファイアウォールを構成する必要があります。 Azure SQL サーバーのファイアウォールを構成する手順については、 [こちらの記事](../sql-database/sql-database-configure-firewall-settings.md) を参照してください。

これで前提条件を完了しました。 データ ファクトリを作成するには、次のいずれかの方法を使用します。 上部にあるいずれかのタブまたは次のリンクをクリックして、チュートリアルを実行します。     

* [Azure ポータル](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
* [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
* [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
* [コピー ウィザード](data-factory-copy-data-wizard-tutorial.md)

<!--HONumber=Oct16_HO2-->


