---
title: クラシック Linux VM でのエンドポイントのセットアップ | Microsoft Docs
description: Azure 上で Linux 仮想マシンとの通信を許可するように Azure クラシック ポータルで Linux VM のエンドポイントをセットアップする方法について説明します。
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-service-management

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2016
ms.author: cynthn

---
# Azure 上で Linux のクラシック仮想マシンにエンドポイントをセットアップする方法
クラシック デプロイメント モデルを使用して Azure で作成したすべての Linux 仮想マシンが、プライベート ネットワーク チャネルを介して、同じクラウド サービスまたは仮想ネットワーク内の他の仮想マシンと自動的に通信できます。ただし、インターネットまたは他の仮想ネットワークにあるコンピューターと通信するには、仮想マシンへの着信ネットワーク トラフィックを転送するエンドポイントが必要になります。この記事は、[Windows 仮想マシン](virtual-machines-windows-classic-setup-endpoints.md)にも利用できます。

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

**Resource Manager** デプロイメント モデルでは、エンドポイントは **ネットワーク セキュリティ グループ (NSG)** を使用して構成します。詳細については、「[ポートおよびエンドポイントの開放](virtual-machines-linux-nsg-quickstart.md)」を参照してください。

Azure クラシック ポータルで Linux 仮想マシンを作成すると、通常は Secure Shell (SSH) 用のエンドポイントが自動的に作成されます。必要に応じて、仮想マシンの作成中や作成後に、追加のエンドポイントを構成できます。

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## 次のステップ
* VM エンドポイントは、[Azure コマンド ライン インターフェイス](../virtual-machines-command-line-tools.md)を使用して作成することもできます。**azure vm endpoint create** コマンドを実行します。
* Resource Manager デプロイメント モデルで仮想マシンを作成した場合は、Resource Manager モードで Azure CLI を使用して、VM へのトラフィックを制御する[ネットワーク セキュリティ グループを作成](../virtual-network/virtual-networks-create-nsg-arm-cli.md)できます。

<!---HONumber=AcomDC_0713_2016-->