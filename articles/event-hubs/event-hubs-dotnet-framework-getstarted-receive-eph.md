---
title: 使用 .NET Framework 从 Azure 事件中心接收事件 | Microsoft 文档
description: 按照本教程的说明，使用 .NET Framework 从 Azure 事件中心接收事件。
services: event-hubs
documentationcenter: ''
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: sethm
ms.openlocfilehash: 4f74b0f90795362d3e509fdbd33e5f358227f147
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436865"
---
# <a name="receive-events-from-azure-event-hubs-using-the-net-framework"></a>使用 .NET Framework 从 Azure 事件中心接收事件

## <a name="introduction"></a>介绍

Azure 事件中心是一个服务，可用于处理来自连接设备和应用程序的大量事件数据（遥测数据）。 将数据采集到事件中心后，可以使用任何实时分析提供程序或存储群集来转换和存储数据。 这种大规模事件收集和处理功能是现代应用程序体系结构（包括物联网 (IoT)）的重要组件。

本教程显示如何编写 .NET Framework 控制台应用程序，以使用**[事件处理程序主机][Event Processor Host]** 从事件中心接收消息。 要使用 .NET Framework 发送事件，请参阅[使用 .NET Framework 将事件发送到 Azure 事件中心](event-hubs-dotnet-framework-getstarted-send.md)一文，或者单击左侧目录中的相应发送语言。

[事件处理程序主机][EventProcessorHost]是一个 .NET 类，它通过从事件中心管理持久检查点和并行接收来简化从那些事件中心接收事件的过程。 使用事件处理程序主机，可跨多个接收方拆分事件，即使在不同节点中托管时也是如此。 此示例演示如何为单一接收方使用事件处理程序主机。 [扩大事件处理][Scale out Event Processing with Event Hubs]示例显示如何将事件处理程序主机用于多个接收方。

## <a name="prerequisites"></a>先决条件

若要完成本教程，需要具备以下先决条件：

* [Microsoft Visual Studio 2017 或更高版本](http://visualstudio.com)。
* 有效的 Azure 帐户。 如果没有帐户，只需几分钟的时间就能创建一个免费帐户。 有关详细信息，请参阅 [Azure 免费试用](https://azure.microsoft.com/free/)。

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>创建事件中心命名空间和事件中心

第一步是使用 [Azure 门户](https://portal.azure.com)创建事件中心类型的命名空间，并获取应用程序与事件中心进行通信所需的管理凭据。 若要创建命名空间和事件中心，请按照[本文](event-hubs-create.md)中的步骤进行操作，然后继续执行本教程的以下步骤。

## <a name="create-an-azure-storage-account"></a>创建 Azure 存储帐户

若要使用[事件处理程序主机][EventProcessorHost]，必须有一个 [Azure 存储帐户][Azure Storage account]：

1. 登录到 [Azure 门户][Azure portal]，单击屏幕左上角的“创建资源”。

2. 单击“存储”，并单击“存储帐户”。
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)

3. 在“创建存储帐户”窗格中，键入存储帐户的名称。 选择 Azure 订阅、资源组和创建该资源的位置。 然后单击“创建”。
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

4. 在存储帐户列表中，单击新建的存储帐户。

5. 在“存储帐户”窗格中，单击“访问密钥”。 复制 **key1** 的值，在本教程的后面部分使用。
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a>创建接收方控制台应用程序

1. 在 Visual Studio 中，使用 **控制台应用程序** 项目模板创建一个新的 Visual C# 桌面应用项目。 将该项目命名为 **Receiver**。
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. 在解决方案资源管理器中，右键单击“Receiver”项目，并单击“为解决方案管理 NuGet 包”。
3. 单击“浏览”选项卡，并搜索 `Microsoft Azure Service Bus Event Hub - EventProcessorHost`。 单击“安装” 并接受使用条款。
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Visual Studio 下载、安装 [Azure 服务总线事件中心 - EventProcessorHost NuGet 包](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)及其所有依赖项并添加对它们的引用。
4. 右键单击 **Receiver** 项目，单击“添加”，并单击“类”。 将新类命名为 **SimpleEventProcessor**，然后单击“添加”以创建该类。
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. 在 SimpleEventProcessor.cs 文件的顶部添加以下语句：
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  然后，用以下代码替换该类的正文：
    
  ```csharp
  class SimpleEventProcessor : IEventProcessor
  {
    Stopwatch checkpointStopWatch;
    
    async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
    {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
    
    Task IEventProcessor.OpenAsync(PartitionContext context)
    {
        Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
        this.checkpointStopWatch = new Stopwatch();
        this.checkpointStopWatch.Start();
        return Task.FromResult<object>(null);
    }
    
    async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData eventData in messages)
        {
            string data = Encoding.UTF8.GetString(eventData.GetBytes());
    
            Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                context.Lease.PartitionId, data));
        }
    
        //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
        if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
        {
            await context.CheckpointAsync();
            this.checkpointStopWatch.Restart();
        }
    }
  }
  ```
    
  此类由 EventProcessorHost 调用，用于处理从事件中心接收的事件。 `SimpleEventProcessor` 类使用秒表定期对 EventProcessorHost 上下文调用检查点方法。 此操作确保接收方重启时，其丢失的处理工作不会超过五分钟。
6. 在 **Program** 类中，在文件顶部添加以下 `using` 语句：
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  然后，将 `Program` 类中的 `Main` 方法替换为以下代码，从而替换为以前保存的事件中心名称和命名空间级别连接字符串，以及在前面部分复制的存储帐户和密钥。 
    
  ```csharp
  static void Main(string[] args)
  {
    string eventHubConnectionString = "{Event Hubs namespace connection string}";
    string eventHubName = "{Event Hub name}";
    string storageAccountName = "{storage account name}";
    string storageAccountKey = "{storage account key}";
    string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
    
    string eventProcessorHostName = Guid.NewGuid().ToString();
    EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
    Console.WriteLine("Registering EventProcessor...");
    var options = new EventProcessorOptions();
    options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
    eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
    
    Console.WriteLine("Receiving. Press enter key to stop worker.");
    Console.ReadLine();
    eventProcessorHost.UnregisterEventProcessorAsync().Wait();
  }
  ```

7. 运行程序，并确保没有任何错误。
  
祝贺你！ 现在已使用事件处理程序主机从事件中心接收消息。


> [!NOTE]
> 本教程使用单个 [EventProcessorHost][EventProcessorHost] 实例。 若要增加吞吐量，建议运行多个 [EventProcessorHost][EventProcessorHost] 实例，如[扩大事件处理][扩大事件处理]示例中所示。 在这些情况下，为了对接收的事件进行负载均衡，各个实例会自动相互协调。 如果希望多个接收方都各自处理 *全部* 事件，则必须使用 **ConsumerGroup** 概念。 在从不同计算机中接收事件时，根据部署 [EventProcessorHost][EventProcessorHost] 实例的计算机（或角色）来指定该实例的名称可能会很有用。 有关这些主题的详细信息，请参阅[事件中心概述][Event Hubs overview]和[事件中心编程指南][Event Hubs Programming Guide]主题。
> 
> 

## <a name="next-steps"></a>后续步骤

现在已生成了一个可以创建事件中心以及发送和接收数据的有效应用程序，接下来请访问以下链接，继续学习：

* [事件处理程序主机概述][Event Processor Host]
* [事件中心概述][Event Hubs overview]
* [事件中心常见问题](event-hubs-faq.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Event Hubs overview]: event-hubs-about.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: event-hubs-event-processor-host.md
[Azure portal]: https://portal.azure.com
