---
title: "コピー アクティビティを使用したデータの移動 | Microsoft Docs"
description: "Data Factory パイプラインでのデータの移動 (クラウド ストア間、およびオンプレミスのストアとクラウド ストアの間でのデータ移行) について説明します。 コピー アクティビティの使用。"
keywords: "データのコピー, データの移動, データの移行, データ転送"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 67543a20-b7d5-4d19-8b5e-af4c1fd7bc75
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: ef5c1f296a0a4ee6476db663e85c49c351f826b9
ms.openlocfilehash: 53a2012a1d928c961cbfbdcea485ae18d776360f


---
# <a name="move-data-by-using-copy-activity"></a>コピー アクティビティを使用したデータの移動
## <a name="overview"></a>Overview
Azure Data Factory では、コピー アクティビティを使用して、さまざまな形式のデータを、オンプレミスやクラウドの各種データ ソースから Azure にコピーできます。 コピーしたデータは、高度な変換や分析を実行できます。 また、コピー アクティビティを使用して、変換や分析の結果を発行し、ビジネス インテリジェンス (BI) やアプリケーションで使用することもできます。

![コピー アクティビティの役割](media/data-factory-data-movement-activities/copy-activity.png)

コピー アクティビティは、安全性、信頼性、拡張性に優れた [グローバルに利用可能なサービス](#global)によって動作します。 この記事では、Data Factory でのコピー アクティビティによるデータの移動について詳しく説明します。

まず、2 つのクラウド データ ストアの間、およびオンプレミスのデータ ストアとクラウド データ ストアの間でデータの移行がどのように行われるかについて説明します。

> [!NOTE]
> アクティビティ全般については、 [パイプラインとアクティビティの概要](data-factory-create-pipelines.md)に関するページをご覧ください。
>
>

### <a name="copy-data-between-two-cloud-data-stores"></a>2 つのクラウド データ ストア間でのデータのコピー
ソース データ ストアとシンク データ ストアの両方がクラウドにある場合、コピー アクティビティは、次の段階を経てデータをソースからシンクにコピーします。 コピー アクティビティを実行するサービスが行う処理を次に示します。

1. ソース データ ストアからデータを読み取る。
2. シリアル化/逆シリアル化、圧縮/圧縮解除、列マッピング、型変換を実行する。 この操作は、入力データセット、出力データセット、およびコピー アクティビティの構成に基づいて実行されます。
3. コピー先データ ストアにデータを書き込む。

このサービスでは、データ移動の実行に適したリージョンが自動的に選択されます。 選択されるのは、通常、シンク データ ストアに最も近いリージョンです。

![クラウド間のコピー](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>オンプレミス データ ストアとクラウド データ ストア間でのデータのコピー
オンプレミス データ ストアと、クラウド データ ストアの間でデータを安全に移動するには、Data Management Gateway を、オンプレミスのコンピューターにインストールする必要があります。 Data Management Gateway は、ハイブリッド データの移動と処理を可能にするエージェントです。 この Data Management Gateway は、データ ストア自体と同じコンピューター、またはデータ ストアにアクセスできる別のコンピューターにインストールできます。

このシナリオでは、Data Management Gateway によって、シリアル化/逆シリアル化、圧縮/圧縮解除、列マッピング、型変換が実行されます。 データは Azure Data Factory サービスを経由せず、 Data Management Gateway によって、直接目的のストアに書き込まれます。

![オンプレミスとクラウドの間のコピー](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

概要とチュートリアルについては、 [オンプレミスのデータ ストアとクラウド データ ストアの間でのデータ移動](data-factory-move-data-between-onprem-and-cloud.md) に関する記事をご覧ください。 このエージェントの詳細については、「 [Data Management Gateway](data-factory-data-management-gateway.md) 」をご覧ください。

Data Management Gateway を使用すると、Azure IaaS 仮想マシン (VM) でホストされている、サポートされるデータ ストア間でデータを移動することもできます。 この場合、Data Management Gateway は、データ ストア自体と同じ VM、またはデータ ストアにアクセスできる別の VM にインストールできます。

## <a name="supported-data-stores-and-formats"></a>サポートされるデータ ストアと形式
[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

コピー アクティビティでサポートされていないデータ ストアとの間でデータを移動する必要がある場合は、データのコピーと移動に独自のロジックを使用した、Data Factory の **カスタム アクティビティ** を使用します。 カスタム アクティビティの作成と使用の詳細については、「 [Azure Data Factory パイプラインでカスタム アクティビティを使用する](data-factory-use-custom-activities.md)」をご覧ください。

### <a name="supported-file-formats"></a>サポートされるファイル形式
コピー アクティビティを使用すると、Azure BLOB、Azure Data Lake Store、Amazon S3、FTP、ファイル システム、HDFS などを含む&2; つのファイル ベース データ ストアの間で**ファイルをそのままコピー**できます。 これを行うには、入力と出力の両方のデータセット定義で [format セクション](data-factory-create-datasets.md) をスキップします。 これにより、シリアル化/逆シリアル化を実行することなく、データが効率的にコピーされます。

また、コピー アクティビティは、指定された形式 (**テキスト、Avro、ORC、Parquet、JSON**) でのファイルの読み取りと書き込みも行います。 コピー アクティビティの例をいくつか示します。

* Azure BLOB からテキスト (CSV) 形式でデータをコピーし、Azure SQL Database に書き込む。
* オンプレミスのファイル システムからテキスト (CSV) 形式でファイルをコピーし、Azure BLOB に Avro 形式で書き込む。
* Azure SQL Database のデータをコピーし、オンプレミスの HDFS に ORC 形式で書き込む。

## <a name="a-nameglobalaglobally-available-data-movement"></a><a name="global"></a>グローバルに使用できるデータの移動
Azure Data Factory は、米国西部、米国東部、北ヨーロッパ リージョンでのみ使用できます。 ただし、コピー アクティビティを実行するサービスは、以下のリージョンと場所でグローバルに使用できます。 グローバルに使用できるトポロジでは効率的なデータ移動が保証されます。このデータ移動では、通常、リージョンをまたがるホップが回避されます。 特定のリージョンにおける Data Factory とデータ移動の提供状況については、[リージョン別のサービス](https://azure.microsoft.com/regions/#services)に関するページをご覧ください。

### <a name="copy-data-between-cloud-data-stores"></a>クラウド データ ストア間でのデータのコピー
ソース データとシンク データが両方ともクラウドにある場合、Data Factory は同じ地域内で対象シンクに最も近いリージョンにあるサービスのデプロイメントを使用して、データを移動します。 リージョンのマッピングについては、以下の表をご覧ください。

| コピー先データ ストアの地理的な場所 | コピー先データ ストアのリージョン | データ移動に使用するリージョン |
|:--- |:--- |:--- |
| 米国 | 米国東部 | 米国東部 |
| に関するページを参照してください。 | 米国東部 2 | 米国東部 2 |
| に関するページを参照してください。 | 米国中央部 | 米国中央部 |
| に関するページを参照してください。 | 米国中北部 | 米国中北部 |
| に関するページを参照してください。 | 米国中南部 | 米国中南部 |
| に関するページを参照してください。 | 米国中西部 | 米国中西部 |
| 」の説明どおりに仮想デバイスをプロビジョニングして接続していること。 | 米国西部 | 米国西部 |
| に関するページを参照してください。 | 米国西部 2 | 米国西部 |
| カナダ | カナダ東部 | カナダ中部 |
| に関するページを参照してください。 | カナダ中部 | カナダ中部 |
| ブラジル | ブラジル南部 | ブラジル南部 |
| ヨーロッパ | 北ヨーロッパ | 北ヨーロッパ |
| に関するページを参照してください。 | 西ヨーロッパ | 西ヨーロッパ |
| アジア太平洋 | 東南アジア | 東南アジア |
| に関するページを参照してください。 | 東アジア | 東南アジア |
| オーストラリア | オーストラリア東部 | オーストラリア東部 |
| に関するページを参照してください。 | オーストラリア南東部 | オーストラリア南東部 |
| 日本 | 東日本 | 東日本 |
| に関するページを参照してください。 | 西日本 | 東日本 |
| インド | インド中部 | インド中部 |
| に関するページを参照してください。 | インド西部 | インド中部 |
| に関するページを参照してください。 | インド南部 | インド中部 |


> [!NOTE]
> コピー先データ ストアのリージョンが前のリストにない場合、代わりのリージョンには移動せず、コピー アクティビティは失敗します。
>
>

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>オンプレミス データ ストアとクラウド データ ストア間でのデータのコピー
データがオンプレミス (または Azure 仮想マシン/IaaS) のストアとクラウド ストアの間でコピーされる場合は、 [Data Management Gateway](data-factory-data-management-gateway.md) が、オンプレミスのコンピューターまたは仮想マシンでデータ移動を実行します。 データは、 [ステージング コピー](data-factory-copy-activity-performance.md#staged-copy) 機能を使用しない限り、クラウド上のサービスを経由しません。 この場合、データはシンク データ ストアに書き込まれる前に、ステージング Azure BLOB ストレージを経由します。

## <a name="create-a-pipeline-with-copy-activity"></a>コピー アクティビティのあるパイプラインの作成
コピー アクティビティのあるパイプラインを作成する方法はいくつかあります。

### <a name="by-using-the-copy-wizard"></a>コピー ウィザードを使用
Data Factory コピー ウィザードは、コピー アクティビティのあるパイプラインを作成するのに役立ちます。 このパイプラインを使用すると、リンクされたサービス、データセット、およびパイプラインの " *JSON 定義を作成しなくても* "、サポートされているソースからデータをコピーできます。 このウィザードの詳細については、「 [Data Factory コピー ウィザード](data-factory-copy-wizard.md) 」をご覧ください。  

### <a name="by-using-json-scripts"></a>JSON スクリプトを使用
Azure ポータル、Visual Studio、または Azure PowerShell で Data Factory エディターを使用すると、パイプラインの JSON 定義を作成できます (コピー アクティビティを使用)。 その後、その定義をデプロイして、Data Factory にパイプラインを作成することができます。 詳しい手順については、 [Azure Data Factory パイプラインでコピー アクティビティを使用する方法](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) に関するチュートリアルをご覧ください。    

JSON プロパティ (名前、説明、入力テーブル、出力テーブル、ポリシーなど) は、あらゆる種類のアクティビティで使用できます。 アクティビティの `typeProperties` セクションで使用できるプロパティは、各アクティビティの種類によって異なります。

コピー アクティビティの場合、 `typeProperties` セクションはソースとシンクの種類によって異なります。 そのデータ ストアについて、コピー アクティビティでサポートされている type プロパティについては、 [サポートされているソースとシンク](#supported-data-stores) に関するセクションでソース/シンクをクリックしてください。   

JSON 定義のサンプルを次に示します。

```json
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from Azure blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputBlobTable"
          }
        ],
        "outputs": [
          {
            "name": "OutputSQLTable"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
}
```
出力データセットで定義されているスケジュールに従って、アクティビティが実行されるタイミングが決まります (たとえば、frequency を **day**、interval を **1** に設定すると、**日単位**で実行されます)。 コピー アクティビティでは、入力データセット (**ソース**) から出力データセット (**シンク**) にデータがコピーされます。

コピー アクティビティには複数の入力データセットを指定できます。 この複数の入力データセットを使用して、アクティビティが実行される前に依存関係が検証されます。 ただし、コピーされるのは、最初のデータセットのデータだけです。 詳細については、「 [スケジュールと実行](data-factory-scheduling-and-execution.md)」を参照してください。  

## <a name="performance-and-tuning"></a>パフォーマンスとチューニング
Azure Data Factory でのデータ移動 (コピー アクティビティ) のパフォーマンスに影響する主な要因については、「 [コピー アクティビティのパフォーマンスとチューニングに関するガイド](data-factory-copy-activity-performance.md)」をご覧ください。 このガイドでは、内部テスト実行時の実際のパフォーマンスを一覧表示すると共に、コピー アクティビティのパフォーマンスを最適化するさまざまな方法についても説明します。

## <a name="scheduling-and-sequential-copy"></a>スケジュール設定と順次コピー
Data Factory でのスケジュール設定と実行のしくみに関する詳細については、 [スケジュール設定と実行のしくみ](data-factory-scheduling-and-execution.md) に関するページをご覧ください。 複数のコピー操作を、順番にまたは順序を指定して&1; つずつ実行できます。 [順次コピー](data-factory-scheduling-and-execution.md#run-activities-in-a-sequence)に関するセクションを参照してください。

## <a name="type-conversions"></a>型の変換
データ ストアが異なると、ネイティブな型システムも異なります。 コピー アクティビティは次の&2; 段階のアプローチで型を source から sink に自動的に変換します。

1. ネイティブの source 型から .NET 型に変換する。
2. .NET 型からネイティブの sink 型に変換する。

データ ストアのネイティブ型システムから .NET 型へのマッピングは、該当するデータ ストアの記事を参照してください  ([サポートされるデータ ストア](#supported-data-stores)の表に示されているリンクをクリックしてください)。 このマッピングを使用して、テーブル作成時に適切な型を決定でき、コピー アクティビティによって適切な変換が実行されます。

## <a name="next-steps"></a>次のステップ
* コピー アクティビティの詳細については、「 [Azure Blob Storage から Azure SQL Database にデータをコピーする](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)」を参照してください。
* オンプレミスのデータ ストアからクラウド データ ストアへのデータの移動については、 [オンプレミスのデータ ストアからクラウド データ ストアへのデータの移動](data-factory-move-data-between-onprem-and-cloud.md)に関するページをご覧ください。



<!--HONumber=Dec16_HO2-->


