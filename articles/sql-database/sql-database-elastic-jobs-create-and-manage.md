---
title: "エラスティック ジョブを使用したスケールアウト済み Azure SQL Database の作成と管理 (プレビュー) | Microsoft Docs"
description: "エラスティック データベースのジョブの作成と管理について説明します。"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: f858344d-085b-4022-935e-1b5fa20adbac
ms.service: sql-database
ms.custom: jobs
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
translationtype: Human Translation
ms.sourcegitcommit: 7b3c6d29ad7ded2ea65f378a1f807235c688d738
ms.openlocfilehash: 2e652979d56e8f14179102a946761874553219df


---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>エラスティック ジョブを使用したスケールアウト済み Azure SQL Database の作成と管理 (プレビュー)
> [!div class="op_single_selector"]
> * [Azure ポータル](sql-database-elastic-jobs-create-and-manage.md)
> * [PowerShell](sql-database-elastic-jobs-powershell.md)
> 
> 

**Elastic Database ジョブ** では、スキーマの変更、資格情報の管理、参照データの更新、パフォーマンス データの収集、テナント (顧客) 製品利用統計情報の収集などの管理操作を実行して、データベース グループの管理を簡略化できます。 現在、Elastic Database ジョブは、Azure ポータルまたは PowerShell コマンドレットから使用できます。 ただし、Azure ポータルでは、 [エラスティック データベース プール (プレビュー)](sql-database-elastic-pool.md)のすべてのデータベースに対して実行される機能に限定されています。 その他の機能や、カスタム定義のコレクションや ([Elastic Database クライアント ライブラリ](sql-database-elastic-scale-introduction.md)を使用して作成された) シャード セットなどのデータベース グループ全体に対する実行にアクセスするには、「[PowerShell を使用したジョブの作成と管理](sql-database-elastic-jobs-powershell.md)」をご覧ください。 ジョブの詳細については、「 [エラスティック データベース ジョブの概要](sql-database-elastic-jobs-overview.md)」を参照してください。 

## <a name="prerequisites"></a>前提条件
* Azure サブスクリプション。 無料評価版については、「 [1 か月間の無料評価版](https://azure.microsoft.com/pricing/free-trial/)」をご覧ください。
* エラスティック データベース プール。 「 [弾力性データベース プール](sql-database-elastic-pool.md)
* エラスティック データベース ジョブ サービス コンポーネントのインストール。 「 [弾力性データベース ジョブ サービスのインストール](sql-database-elastic-jobs-service-installation.md)」をご覧ください。

## <a name="creating-jobs"></a>ジョブの作成
1. [Azure ポータル](https://portal.azure.com)を使用して、既存のエラスティック データベース ジョブ プールで [ **ジョブの作成**] をクリックします。
2. ジョブ管理データベース (ジョブのメタデータ ストレージ) のデータベース管理者のユーザー名とパスワード (ジョブのインストール時に作成) を入力します。
   
    ![ジョブに名前を付け、コードに入力するか、貼り付け、[実行] をクリックします。][1]
3. **[ジョブの作成]** ブレードで、ジョブの名前を入力します。
4. ターゲット データベースに接続するために、スクリプトを正常に実行できるアクセス許可を持つユーザー名とパスワードを入力します。
5. T-SQL スクリプトを貼り付けるか、入力します。
6. **[保存]** をクリックし、次に **[実行]** をクリックします。
   
    ![ジョブを作成して実行します。][5]

## <a name="run-idempotent-jobs"></a>べき等ジョブを実行します
一連のデータベースに対してスクリプトを実行する場合、スクリプトがべき等であることを確認する必要があります。 つまり、スクリプトは、前に不完全な状態で失敗した場合でも、複数回実行される必要があります。 たとえば、スクリプトが失敗すると、ジョブは成功するまで自動的に再試行されます (制限内、再試行ロジックが最終的に再試行を停止するまで)。 これを行うには、"IF EXISTS" 句を使用して、新しいオブジェクトを作成する前に、検出されたすべてのインスタンスを削除します。 例を次に示します。

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

代わりに、新しいインスタンスを作成する前に、"IF NOT EXISTS" 句を使用します。

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

このスクリプトは、以前に作成されたテーブルを更新します。

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>ジョブの状態を確認する
ジョブが開始されると、進行状況を確認できます。

1. 弾力性データベース ページ プールで、 **[ジョブの管理]**をクリックします。
   
    ![[ジョブの管理] をクリックします。][2]
2. ジョブの名前 (a) をクリックします。 **[状態]** は、「完了」か「失敗」です。 ジョブの詳細は、作成と実行の日時とともに (b) に表示されます。 下記のリスト (c) は、プール内の各データベースに対するスクリプトの進行状況を日時の詳細とともに表示します。
   
    ![完成したジョブの確認][3]

## <a name="checking-failed-jobs"></a>失敗したジョブの確認
ジョブが失敗した場合、その実行ログを確認できます。 失敗したジョブの名前をクリックして詳細を表示します。

![失敗したジョブを確認する][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png





<!--HONumber=Dec16_HO1-->


