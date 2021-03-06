---
title: include 文件
description: include 文件
services: virtual-machines
author: rogara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/03/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 399479de0ce9bab29d0338b1155f8b0c1bab542c
ms.sourcegitcommit: caebf2bb2fc6574aeee1b46d694a61f8b9243198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2018
ms.locfileid: "35414580"
---
# <a name="azure-managed-disks-overview"></a>Azure 托管磁盘概述

Azure 托管磁盘通过管理与 VM 磁盘关联的[存储帐户](../articles/storage/common/storage-introduction.md)简化了 Azure IaaS VM 的磁盘管理。 只需指定所需的磁盘类型（[标准 HDD](../articles/virtual-machines/windows/standard-storage.md)、标准 SSD 或[高级 SSD](../articles/virtual-machines/windows/premium-storage.md)）和大小，Azure 即可创建和管理磁盘。

## <a name="benefits-of-managed-disks"></a>托管磁盘的好处

让我们看看使用托管磁盘的一些好处，情观看 9 频道视频 - [托管磁盘使 Azure VM 更加可靠](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency)。
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency/player]

### <a name="simple-and-scalable-vm-deployment"></a>简单且可缩放的 VM 部署

托管磁盘在幕后处理存储。 以前，必须创建存储帐户来存储 Azure VM 的磁盘（VHD 文件）。 进行扩展时，必须确保创建了额外的存储帐户，以便任何磁盘都不会超出对存储的 IOPS 限制。 使用托管磁盘处理存储时，不再受限于存储帐户限制（例如 20,000 IOPS / 帐户）。 也不再需要将自定义映像（VHD 文件）复制到多个存储帐户。 可以在一个中心位置管理自定义映像（每个 Azure 区域一个存储帐户），并使用它们在一个订阅中创建数百台 VM。

托管磁盘支持在每个区域中的一个订阅中创建最多 50,000 个同一类型的 VM 磁盘，这使得可以在单个订阅中创建数以万计的 VM。 通过允许使用某个市场映像在一个虚拟机规模集中创建多达一千台 VM，此功能还可以进一步增加[虚拟机规模集](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)的可伸缩性。 

### <a name="better-reliability-for-availability-sets"></a>可用性集更加可靠

通过确保[可用性集中的 VM ](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set)的磁盘彼此之间完全隔离以避免单点故障，托管磁盘为可用性集提供了更佳的可靠性。 磁盘自动放置于不同的存储缩放单元（模块）。 如果某个模块因硬件或软件故障而失败，则只有其磁盘在该模块上的 VM 实例会失败。 例如，假定某个应用程序在 5 台 VM 上运行并且这些 VM 位于一个可用性集中。 这些 VM 的磁盘不会存储在同一个模块中，因此，如果一个模块失败，该应用程序的其他实例可以继续运行。

### <a name="highly-durable-and-available"></a>高度持久和可用

Azure 磁盘具备 99.999% 的可用性。 数据具有三个副本，高持久性可让用户高枕无忧。 如果其中一个或两个副本出现问题，剩下的副本能够确保数据的持久性和对故障的高耐受性。 此架构有助于 Azure 为 IaaS 磁盘持续提供企业级的持久性，年化故障率为 0%，达到行业领先水平。 

### <a name="granular-access-control"></a>粒度访问控制

可以使用 [Azure 基于角色的访问控制 (RBAC)](../articles/role-based-access-control/overview.md) 将对托管磁盘的特定权限分配给一个或多个用户。 托管磁盘公开了各种操作，包括读取、写入（创建/更新）、删除，以及检索磁盘的[共享访问签名 (SAS) URI](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)。 可以仅将某人员执行其工作所需的操作的访问权限授予该人员。 例如，如果不希望某人员将某个托管磁盘复制到存储帐户，则可以选择不授予对该托管磁盘的导出操作的访问权限。 类似地，如果不希望某人员使用 SAS URI 复制某个托管磁盘，则可以选择不授予对该托管磁盘的该权限。

### <a name="azure-backup-service-support"></a>Azure 备份服务支持
将 Azure 备份服务与托管磁盘配合使用，创建具有基于时间的备份、轻松 VM 还原和备份保留策略的备份作业。 托管存储仅支持使用本地冗余存储 (LRS) 作为复制选项。 数据的三个副本保留在同一个区域中。 对于区域性灾难恢复，必须使用 [Azure 备份服务](../articles/backup/backup-introduction-to-azure-backup.md)和作为备份保管库的 GRS 存储帐户来备份不同区域中的 VM 磁盘。 当前 Azure 备份支持所有磁盘大小（包括 4TB 磁盘）。 若要支持 4TB 磁盘，需[将 VM 备份堆栈升级到 V2](../articles/backup/backup-upgrade-to-vm-backup-stack-v2.md)。 有关详细信息，请参阅[为具有托管磁盘的 VM 使用 Azure 备份服务](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup)。

## <a name="pricing-and-billing"></a>定价和计费

使用托管磁盘时，将考虑以下计费事项：
* 存储类型

* 磁盘大小

* 事务数

* 出站数据传输

* 托管磁盘快照（全磁盘复制）

下面将更详细地介绍各选项。

**存储类型：** 托管磁盘提供 3 个性能层：[标准 HDD](../articles/virtual-machines/windows/standard-storage.md)、标准 SSD（预览）和[高级](../articles/virtual-machines/windows/premium-storage.md)。 托管磁盘的计费取决于为磁盘选择的存储类型。


**磁盘大小**：托管磁盘的计费取决于磁盘的预配大小。 Azure 会将预配大小映射（向上舍入）到下面各表中指定的最接近的托管磁盘选项。 每个托管磁盘都映射到其中一种受支持的预配大小并相应地进行计费。 例如，如果创建了一个标准托管磁盘并将预配大小指定为 200 GB，则会根据 S15 磁盘类型的定价向你收费。

下面是高级托管磁盘可用的磁盘大小：

| **高级托管<br>磁盘类型** | **P4** | **P6** |**P10** | **P15** | **P20** | **P30** | **P40** | **P50** | 
|------------------|---------|---------|---------|---------|---------|----------------|----------------|----------------|  
| 磁盘大小        | 32 GiB   | 64 GiB   | 128 GiB  | 256 GiB  | 512 GiB  | 1024 GiB (1 TiB) | 2048 GiB (2 TiB) | 4095 GiB (4 TiB) | 

下面是标准 SSD 托管磁盘可用的磁盘大小：

| 标准 SSD 托管<br>磁盘类型 | E10 | E15 | E20 | E30 | E40 | E50 |
|------------------|--------|--------|--------|----------------|----------------|----------------| 
| 磁盘大小        | 128 GiB | 256 GiB | 512 GiB | 1024 GiB (1 TiB) | 2048 GiB (2 TiB) | 4095 GiB (4 TiB) | 

下面是标准 HDD 托管磁盘可用的磁盘大小：

| 标准 HDD 托管<br>磁盘类型 | **S4** | **S6** | **S10** | **S15** | **S20** | **S30** | **S40** | **S50** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------| 
| 磁盘大小        | 32 GiB  | 64 GiB  | 128 GiB | 256 GiB | 512 GiB | 1024 GiB (1 TiB) | 2048 GiB (2 TiB) | 4095 GiB (4 TiB) | 


**事务数**：会根据你对标准托管磁盘执行的事务数向你收费。

标准 SSD 盘使用大小为 256 KB 的 IO 单位。 如果要传输的数据小于 256 KB，该数据会被视为 1 个 I/O 单位。 更大的 I/O 大小被视为多个 256 KB 大小的 I/O。 例如，1,100 KB I/O 会被视为 5 个 I/O 单位。

高级托管磁盘没有事务费用。

**出站数据传输**：[出站数据传输](https://azure.microsoft.com/pricing/details/data-transfers/)（Azure 数据中心送出的数据）会产生带宽使用费。

有关托管磁盘的详细定价信息，请参阅[托管磁盘定价](https://azure.microsoft.com/pricing/details/managed-disks)。


## <a name="managed-disk-snapshots"></a>托管磁盘快照

托管快照是托管磁盘的只读完整副本，默认情况下它作为标准托管磁盘进行存储。 使用快照，可以在任意时间点备份托管磁盘。 这些快照独立于源磁盘而存在，并可用来创建新的托管磁盘。 基于已使用大小对这些快照进行计费。 例如，如果创建预配容量为 64 GiB、实际使用数据大小为 10 GiB 的托管磁盘的快照，将仅针对已用数据大小 10 GiB 对该快照进行计费。  

目前，托管磁盘不支持[增量快照](../articles/virtual-machines/windows/incremental-snapshots.md)。

若要了解有关如何使用托管磁盘创建快照的详细信息，请查看下列资源：

* [在 Windows 中使用快照创建存储为托管磁盘的 VHD 的副本](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [在 Linux 中使用快照创建存储为托管磁盘的 VHD 的副本](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)


## <a name="images"></a>映像

托管磁盘还支持创建托管自定义映像。 可以从存储帐户中的自定义 VHD 创建映像或者直接从通用化 (sys-prepped) VM 创建映像。 该过程会将与 VM 关联的所有托管磁盘捕获到单个映像中，包括 OS 和数据磁盘。 该托管自定义映像支持使用自定义映像创建数百台 VM，且不需要复制或管理任何存储帐户。

有关创建映像的信息，请查看以下文章：
* [如何在 Azure 中捕获通用 VM 的托管映像](../articles/virtual-machines/windows/capture-image-resource.md)
* [如何使用 Azure CLI 2.0 用化和捕获 Linux 虚拟机](../articles/virtual-machines/linux/capture-image.md)

## <a name="images-versus-snapshots"></a>映像与快照

经常看到词语“映像”与 VM 一起使用，现在还看到了“快照”。 了解这些术语之间的区别很重要。 使用托管磁盘，可以创建已解除分配的通用 VM 的映像。 此映像将包括附加到该 VM 的所有磁盘。 可以使用此映像创建新的 VM，并且它将包括所有磁盘。

快照是磁盘在创建快照那一刻的副本。 它仅应用于一个磁盘。 如果存在仅具有一个磁盘（OS）的 VM，则可以为其创建快照或映像，并且可以通过该快照或映像创建 VM。

如果 VM 具有五个磁盘且这些磁盘是条带化的，会怎样？ 可以创建每个磁盘的快照，但是系统对于 VM 中的磁盘状况没有意识 – 快照只知道那一个磁盘的状况。 在这种情况下，快照彼此之间需要相互协调，目前不支持此功能。

## <a name="managed-disks-and-encryption"></a>托管磁盘和加密

以下介绍托管磁盘的两种加密方式。 第一种是存储服务加密 (SSE)，由存储服务执行。 第二种是 Azure 磁盘加密，可以在 VM 的 OS 和数据磁盘上启用。

### <a name="storage-service-encryption-sse"></a>存储服务加密 (SSE)

[Azure 存储服务加密](../articles/storage/common/storage-service-encryption.md) 可提供静态加密和保护你的数据，使你的组织能够信守在安全性与符合性方面所做的承诺。 默认情况下，所有托管磁盘都启用了 SSE，所有可用托管磁盘的区域都有快照和映像。 从 2017 年 6 月 10 日起，所有新的托管磁盘/快照/映像和写入到现有托管磁盘的新数据默认情况下都会使用由 Microsoft 托管的密钥自动静态加密。 有关详细信息请访问[托管磁盘常见问题解答页](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption)。


### <a name="azure-disk-encryption-ade"></a>Azure 磁盘加密 (ADE)

Azure 磁盘加密允许加密 IaaS 虚拟机使用的 OS 磁盘和数据磁盘。 此加密包括托管磁盘。 对于 Windows，驱动器是使用行业标准 BitLocker 加密技术加密的。 对于 Linux，磁盘是使用 DM-Crypt 技术加密的。 加密过程与 Azure Key Vault 集成，可让你控制和管理磁盘加密密钥。 有关详细信息，请参阅[适用于 Windows 和 Linux IaaS VM 的 Azure 磁盘加密](../articles/security/azure-security-disk-encryption.md)。

## <a name="next-steps"></a>后续步骤

有关托管磁盘的详细信息，请参考以下文章。

### <a name="get-started-with-managed-disks"></a>托管磁盘入门

* [使用资源管理器和 PowerShell 创建 VM](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md)

* [使用 Azure CLI 2.0 创建 Linux VM](../articles/virtual-machines/linux/quick-create-cli.md)

* [使用 PowerShell 将托管数据磁盘附加到 Windows VM](../articles/virtual-machines/windows/attach-disk-ps.md)

* [将托管磁盘添加到 Linux VM](../articles/virtual-machines/linux/add-disk.md)

* [托管磁盘 PowerShell 示例脚本](https://github.com/Azure-Samples/managed-disks-powershell-getting-started)

* [在 Azure 资源管理器模板中使用托管磁盘](../articles/virtual-machines/windows/using-managed-disks-template-deployments.md)

### <a name="compare-managed-disks-storage-options"></a>比较托管磁盘存储选项

* [高级·SSD 盘](../articles/virtual-machines/windows/premium-storage.md)

* [标准 SSD 和 HDD 盘](../articles/virtual-machines/windows/standard-storage.md)

### <a name="operational-guidance"></a>操作指南

* [从 AWS 和其他平台迁移到 Azure 中的托管磁盘](../articles/virtual-machines/windows/on-prem-to-azure.md)

* [将 Azure VM 转换为 Azure 中的托管磁盘](../articles/virtual-machines/windows/migrate-to-managed-disks.md)
