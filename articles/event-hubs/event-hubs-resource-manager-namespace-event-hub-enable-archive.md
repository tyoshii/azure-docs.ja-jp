---
title: "Azure Resource Manager テンプレートでイベント ハブを含んだ Event Hubs 名前空間を作成してアーカイブを有効にする | Microsoft Docs"
description: "Azure Resource Manager テンプレートでイベント ハブを含んだ Event Hubs 名前空間を作成してアーカイブを有効にします。"
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 8bdda6a2-5ff1-45e3-b696-c553768f1090
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/21/2016
ms.author: shvija;sethm
translationtype: Human Translation
ms.sourcegitcommit: 188e3638393262a8406f322a5720e7e3eadf3e49
ms.openlocfilehash: 6fb396063f4944a3043314cfbc58121f45a5c0c6


---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Azure Resource Manager テンプレートでイベント ハブを含んだ Event Hubs 名前空間を作成してアーカイブを有効にする
この記事では、Azure Resource Manager テンプレートを使用し、イベント ハブを含んだ Event Hubs 名前空間を作成して、イベント ハブのアーカイブを有効にする方法について説明します。 デプロイ対象のリソースを定義する方法と、デプロイの実行時に指定されるパラメーターを定義する方法について説明します。 このテンプレートは、独自のデプロイに使用することも、要件に合わせてカスタマイズすることもできます。

テンプレートの作成の詳細については、「[Azure Resource Manager のテンプレートの作成][Azure Resource Manager のテンプレートの作成]」を参照してください。

Azure リソースの名前付け規則の慣習とパターンの詳細については、[Azure リソースの名前付け規則][Azure リソースの名前付け規則]に関するページを参照してください。

完全なテンプレートについては、GitHub で[イベント ハブと、アーカイブ テンプレートの有効化][イベント ハブと、アーカイブ テンプレートの有効化]に関する記事を参照してください。

> [!NOTE]
> 最新のテンプレートを確認する場合は、「[Azure クイック スタート テンプレート][Azure クイックスタート テンプレート]」ギャラリーで Event Hubs を検索してください。
> 
> 

## <a name="what-will-you-deploy"></a>デプロイの対象
このテンプレートを使用して、イベント ハブを含んだ Event Hubs 名前空間をデプロイし、Event Hubs アーカイブも有効にします。

[Event Hubs](event-hubs-what-is-event-hubs.md) は、Azure への大規模なイベントとテレメトリ受信をわずかな待機時間と高い信頼性で提供するために使用される、イベント処理サービスです。 Event Hubs Archive を使用すると、指定した時間またはサイズの間隔で、Event Hubs のストリーミング データを任意の Azure BLOB ストレージに自動的に配信できます。

デプロイメントを自動的に実行するには、次のボタンをクリックします。

[![Azure へのデプロイ](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>パラメーター
Azure リソース マネージャーを使用して、テンプレートのデプロイ時に値を指定するパラメーターを定義します。 テンプレートには、すべてのパラメーター値を含む `Parameters` という名前のセクションがあります。 これらの値のパラメーターを定義する必要があります。これらの値は、デプロイするプロジェクトやデプロイ先の環境に応じて異なります。 常に同じ値に対してはパラメーターを定義しないでください。 テンプレート内のそれぞれのパラメーターの値は、デプロイされるリソースを定義するために使用されます。

このテンプレートでは、次のパラメーターを定義します。

### <a name="eventhubnamespacename"></a>eventHubNamespaceName
作成する Event Hubs 名前空間の名前。

```json
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName
Event Hubs 名前空間に作成するイベント ハブの名前。

```json
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays
イベント ハブでメッセージを保持する日数。 

```json
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount
イベント ハブに作成するパーティションの数。

```json
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled
イベント ハブのアーカイブを有効にします。

```json
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat
イベント データのシリアル化に使用するエンコード形式。

```json
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime
Azure BLOB ストレージへのデータのアーカイブが開始される時間間隔。

```json
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize
Azure BLOB ストレージへのデータのアーカイブが開始されるサイズ間隔。

```json
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId
目的のストレージ アカウントへのアーカイブを有効にするには、Azure ストレージ アカウントのリソース ID が必要になります。

```json
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName
イベント データのアーカイブ先の BLOB コンテナー。

```json
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion
テンプレートの API バージョン。

```json
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>デプロイ対象のリソース
イベント ハブを含む **EventHubs** タイプの名前空間を作成し、アーカイブも有効にします。

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }

               }

            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>デプロイを実行するコマンド
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI
```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Azure Resource Manager のテンプレートの作成]: ../resource-group-authoring-templates.md
[Azure クイックスタート テンプレート]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Azure リソース マネージャーでの Azure PowerShell の使用]: ../powershell-azure-resource-manager.md
[Azure リソース管理での、Mac、Linux、および Windows 用 Azure CLI の使用]: ../xplat-cli-azure-resource-manager.md
[イベント ハブとコンシューマー グループを作成するためのテンプレート]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Azure リソースの名前付け規則]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[イベント ハブと、アーカイブ テンプレートの有効化]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive



<!--HONumber=Nov16_HO4-->


