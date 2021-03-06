---
title: "クラウド サービスのリモート デスクトップの有効化 (Node.js)"
description: "Azure Node.js アプリケーションをホストする仮想マシンについてリモート デスクトップ アクセスを有効にする方法を説明します。"
services: cloud-services
documentationcenter: nodejs
author: rmcmurray
manager: erikre
editor: 
ms.assetid: a0141904-c9bc-478d-82af-5bceaca5cf6a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/01/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6dedf3a7a7b4092784291334b0586b8f37e86354


---
# <a name="enabling-remote-desktop-in-azure"></a>Azure でのリモート デスクトップの有効化
リモート デスクトップを使用して、Azure で実行されているロール インスタンスのデスクトップにアクセスできます。 リモート デスクトップ接続を使用して仮想マシンの構成やアプリケーションの問題のトラブルシューティングを行うことができます。

> [!NOTE]
> この記事は、Azure クラウド サービスとしてホストされる Node.js アプリケーションに適用されます。
> 
> 

## <a name="prerequisites"></a>前提条件
* [Azure Powershell](../powershell-install-configure.md)のインストールおよび構成。
* Azure クラウド サービスへの Node.js アプリのデプロイ。 詳細については、「 [Node.js アプリケーションの構築と Azure クラウド サービスへのデプロイ](cloud-services-nodejs-develop-deploy-app.md)」を参照してください。

## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>手順 1. Azure PowerShell を使用して、リモート デスクトップ アクセス用にサービスを構成する
リモート デスクトップを使用するには、ユーザー名、パスワード、および証明書を使用して、Azure サービス定義および構成を更新する必要があります。 

アプリのソース ファイルが含まれているコンピューターから次の手順を実行します。

1. **Windows PowerShell** を管理者として実行します。 (**[スタート]** メニューまたは**スタート画面**で、**Windows PowerShell** を検索します)。
2. サービス定義 (.csdef) ファイルおよびサービス構成 (.cscfg) ファイルを含むディレクトリに移動します。
3. 次の PowerShell コマンドレットに入ります。
   
        Enable-AzureServiceProjectRemoteDesktop
4. プロンプトに、ユーザー名とパスワードを入力します。
   
    ![Enable-AzureServiceProjectRemoteDesktop][enable-rdp]
5. 次の PowerShell コマンドレットに入り、変更を発行します。
   
       Publish-AzureServiceProject
   
   ![Publish-AzureServiceProject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>手順 2. ロール インスタンスに接続する
サービス定義の更新を発行後は、ロール インスタンスに接続することができます。

1. [Azure クラシック ポータル]で **[クラウド サービス]** を選択し、サービスを選択します。
   
   ![Azure クラシック ポータル][cloud-services]
2. **[インスタンス]** をクリックし、**[運用]** または **[ステージング]** をクリックしてサービスのインスタンスを表示します。 インスタンスを選択し、ページの下部にある **[接続]** をクリックします。
   
   ![[インスタンス] ページ][3]
3. **[接続]** をクリックすると、.rdp ファイルを保存するように促すメッセージが Web ブラウザーに表示されます。 このファイルを開きます。 (たとえば、Internet Explorer を使用している場合は、**[開く]** をクリックします。)
   
   ![.rdp ファイルを開くまたは保存することを促すメッセージ][4]
4. ファイルが開くと、セキュリティに関する次のメッセージが表示されます。
   
   ![Windows のセキュリティに関するメッセージ][5]
5. **[接続]**をクリックすると、インスタンスにアクセスするための資格情報を入力するように求めるセキュリティ メッセージが表示されます。 [手順 1.][手順 1:  Azure PowerShell を使用して、リモート デスクトップ アクセス用にサービスを構成する] で作成したパスワードを入力し、 **[OK]**をクリックします。
   
   ![ユーザー名/パスワードの入力を求めるメッセージ][6]

接続すると、リモート デスクトップ接続に、Azure 内のインスタンスのデスクトップが表示されます。 

![リモート デスクトップ セッション][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>手順 3. リモート デスクトップ アクセスが無効になるようにサービスを構成する
クラウドのロール インスタンスに対するリモート デスクトップ接続が不要になった場合は、 [Azure PowerShell]を使ってリモート デスクトップ アクセスを無効にします。

1. 次の PowerShell コマンドレットに入ります。
   
       Disable-AzureServiceProjectRemoteDesktop
2. 次の PowerShell コマンドレットに入り、変更を発行します。
   
       Publish-AzureServiceProject

## <a name="additional-resources"></a>その他のリソース
* [Azure のロール インスタンスへのリモート アクセス] 
* [Azure ロールでのリモート デスクトップの使用]
* [Node.js デベロッパー センター](/develop/nodejs/)

[Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure クラシック ポータル]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png

[Azure のロール インスタンスへのリモート アクセス]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Azure ロールでのリモート デスクトップの使用]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx



<!--HONumber=Nov16_HO3-->


