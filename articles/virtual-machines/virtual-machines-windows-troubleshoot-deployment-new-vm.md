---
title: "Windows VM の RM デプロイメントのトラブルシューティング | Microsoft Docs"
description: "Azure で新しい Windows 仮想マシンを作成するときに発生する Resource Manager デプロイメントの問題のトラブルシューティング"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cjiang
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: bec8c8347b3e29e2d87d7565a29187f22bd10652


---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Azure での新しい Windows 仮想マシンの作成に関する Resource Manager デプロイメントの問題のトラブルシューティング
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>監査ログの収集
トラブルシューティングを開始するには、監査ログを収集して問題に関連するエラーを特定します。 このプロセスの詳細については、次のリンクをご覧ください。

[Azure ポータルでのリソース グループのデプロイのトラブルシューティング](../resource-manager-troubleshoot-deployments-portal.md)

[リソース マネージャーの監査操作](../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** OS が一般化された Windows であり、一般化された設定でアップロード/キャプチャされた場合、エラーは発生しません。 同様に、OS が特殊化された Windows であり、特殊化された設定でアップロード/キャプチャされた場合、エラーは発生しません。

**アップロード エラー:**

**N<sup>1</sup>:** OS が一般化された Windows であり、特殊化された Windows としてアップロードされた場合、OOBE 画面の VM スタックでプロビジョニング タイムアウト エラーが発生します。

**N<sup>2</sup>:** OS が特殊化された Windows であり、一般化された Windows としてアップロードされた場合、新しい VM は元のコンピューター名、ユーザー名、パスワードを使用して実行されるため、OOBE 画面の VM スタックでプロビジョニング失敗エラーが発生します。

**解決策**

これらのエラーを解決するには、OS と同じ設定 (一般化/特殊化) を使用して、オンプレミスで使用可能な [元の VHD をアップロードするために Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx)を使用します。 一般化された OS としてアップロードするには、まず sysprep を必ず実行してください。

**キャプチャ エラー:**

**N<sup>3</sup>:** OS が一般化された Windows であり、特殊化された Windows としてキャプチャされた場合、一般化としてマークされた元の VM を使用できないため、プロビジョニング タイムアウト エラーが発生します。

**N<sup>4</sup>:** OS が特殊化された Windows であり、一般化された Windows としてキャプチャされた場合、新しい VM は元のコンピューター名、ユーザー名、パスワードを使用して実行されるため、プロビジョニング失敗エラーが発生します。 また、元の VM は特殊化としてマークされているので使用できません。

**解決策**

これらのエラーの両方を解決するには、ポータルから現在のイメージを削除し、OS と同じ設定 (一般化/特殊化) で[現在の VHD からイメージをキャプチャし直します](virtual-machines-windows-vhd-copy.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>問題: カスタム/ギャラリー/Marketplace イメージ - 割り当てエラー
このエラーは、新しい VM 要求が、要求されている VM サイズをサポートできないか、要求に対応するための使用可能な空き領域がないクラスターに固定されている場合に発生します。

**原因 1:** クラスターが要求された VM サイズをサポートできない。

**解決策 1:**

* VM サイズを小さくして要求を再試行します。
* 要求した VM のサイズを変更できない場合は、次の手順を実行します。
  * 可用性セットのすべての VM を停止します。
    **[リソース グループ]** > *対象のリソース グループ* > **[リソース]** > *対象の可用性セット* > **[Virtual Machines]** > *対象の仮想マシン* > **[停止]** の順にクリックします。
  * すべての VM が停止したら、目的のサイズで新しい VM を作成します。
  * 新しい VM を起動してから、停止している各 VM を選択し、 **[起動]**をクリックします。

**原因 2:** クラスターに空きリソースがない。

**解決策 2:**

* 後で要求を再試行します。
* 新しい VM を別の可用性セットに含めることができる場合
  * 新しい VM を (同じリージョンの) 別の可用性セットに作成します。
  * 新しい VM を同じ仮想ネットワークに追加します。

## <a name="next-steps"></a>次のステップ
Azure での停止していた Windows VM の再起動または既存の Windows VM のサイズ変更に問題が発生する場合は、 [Azure での既存の Windows 仮想マシンの再起動またはサイズ変更に関する Resource Manager デプロイメントの問題のトラブルシューティング](virtual-machines-windows-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)を参照してください。




<!--HONumber=Nov16_HO3-->


