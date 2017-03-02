---
title: "更改 HL7 FHIR 资源的源 - Azure DocumentDB | Microsoft Docs"
description: "了解如何使用 Azure 逻辑应用、DocumentDB 和服务总线设置 HL7 FHIR 患者卫生保健记录的更改通知。"
keywords: hl7 fhir
services: documentdb
author: hedidin
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 0d25c11f-9197-419a-aa19-4614c6ab2d06
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: b-hoedid
translationtype: Human Translation
ms.sourcegitcommit: c25274ad48edb0c89e3f177277af1a4ae5fb3eec
ms.openlocfilehash: dafd6aa1172661e82bccb35dc29fd59b2c04dd6e


---

# <a name="notifying-patients-of-hl7-fhir-health-care-record-changes-using-logic-apps-and-documentdb"></a>使用逻辑应用和 DocumentDB 通知患者 HL7 FHIR 医疗保健记录的更改

一家医疗保健组织最近联系了 Azure MVP Howard Edidin，表示希望向他们的患者门户添加新功能。 他们需要在患者的健康记录更新时向患者发送通知，并且希望患者能订阅这些更新。 

本文将指导如何使用 DocumentDB、逻辑应用和服务总线更改为此医疗保健组织创建的源通知解决方案。 

## <a name="project-requirements"></a>项目要求
- 提供程序需发送 XML 格式的 HL7 综合临床文档架构 (C-CDA) 文档。 C-CDA 文档差不多包含每种类型的临床文档，包括家族病史和免疫记录等临床文档，以及管理、工作流和财务文档。 
- 将 C-CDA 文档转换为 JSON 格式的 [HL7 FHIR 资源](http://hl7.org/fhir/2017Jan/resourcelist.html)。
- 通过电子邮件发送 JSON 格式的修改后的 FHIR 资源文档。

## <a name="solution-workflow"></a>解决方案工作流 

概括而言，此项目需要以下工作流步骤： 
1. 将 C-CDA 文档转换为 FHIR 资源。
2. 为修改后的 FHIR 资源执行定期触发轮询。 
2. 调用自定义应用 FhirNotificationApi 以连接到 DocumentDB，并查询新的或修改后的文档。
3. 将响应保存到服务总线队列。
4. 轮询服务总线队列中的新消息。
5. 向患者发送电子邮件通知。

## <a name="solution-architecture"></a>解决方案体系结构
此解决方案需要三个逻辑应用才能满足上述要求并完成解决方案工作流。 这三个逻辑应用包括：
1. **HL7-FHIR-Mapping 应用**：接收 HL7 C-CDA 文档，将其转换为 FHIR资源，然后保存到 DocumentDB。
2. **EHR 应用**：查询 DocumentDB FHIR 存储库，并将响应保存到服务总线队列。 此逻辑应用使用 [API 应用](#api-app)检索新的和更改后的文档。
3. **进程通知应用**：发送电子邮件通知，正文中包括 FHIR 资源文档。

![此 HL7 FHIR 医疗保健解决方案中使用的三个逻辑应用](./media/documentdb-change-feed-hl7-fhir-logic-apps/documentdb-health-care-solution-hl7-fhir.png)



### <a name="azure-services-used-in-the-solution"></a>解决方案中使用的 Azure 服务

#### <a name="documentdb"></a>DocumentDB
如下图中所示，DocumentDB 是 FHIR 资源的存储库。

![此 HL7 FHIR 医疗保健教程中使用的 Azure DocumentDB 帐户](./media/documentdb-change-feed-hl7-fhir-logic-apps/documentdb-account.png)

#### <a name="logic-apps"></a>Logic Apps
逻辑应用将处理工作流进程。 以下屏幕快照显示了为此解决方案创建的逻辑应用。 


1. **HL7-FHIR-Mapping 应用**：接收 HL7 C-CDA 文档，并使用适用于逻辑应用的 Enterprise Integration Pack 将其转换为 FHIR 资源。 Enterprise Integration Pack 将处理从 C-CDA 到 FHIR 资源的映射。

    ![用于接收 HL7 FHIR 医疗保健记录的逻辑应用](./media/documentdb-change-feed-hl7-fhir-logic-apps/documentdb-hl7-fhir-logic-apps-json-transform.png)


2. **EHR 应用**：查询 DocumentDB FHIR 存储库，并将响应保存到服务总线队列。 下面是 GetNewOrModifiedFHIRDocuments 应用的代码。

    ![用于查询 Azure DocumentDB 的逻辑应用](./media/documentdb-change-feed-hl7-fhir-logic-apps/documentdb-hl7-fhir-logic-apps-api-app.png)

3. **进程通知应用**：发送电子邮件通知，正文中包括 FHIR 资源文档。

    ![向患者发送电子邮件（正文中包括 HL7 FHIR 资源）的逻辑应用](./media/documentdb-change-feed-hl7-fhir-logic-apps/documentdb-hl7-fhir-logic-apps-send-email.png)

#### <a name="service-bus"></a>服务总线
下图显示患者队列。 Tag 属性值将用于电子邮件主题。

![此 HL7 FHIR 教程中使用的服务总线队列](./media/documentdb-change-feed-hl7-fhir-logic-apps/documentdb-hl7-fhir-service-bus-queue.png)

<a id="api-app"></a>

#### <a name="api-app"></a>API 应用
API 应用将连接到 DocumentDB，并按资源类型查询新的或修改后的 FHIR 文档。 此应用具有一个控制器 **FhirNotificationApi**，它带有一个操作 **GetNewOrModifiedFhirDocuments**，请参阅 [API 应用的源](#api-app-source)。

我们使用的是 DocumentDB .NET API 中的 [`CreateDocumentChangeFeedQuery`](https://msdn.microsoft.com/en-us/library/azure/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery.aspx) 类。 有关详细信息，请参阅 [DocumentDB 更改源文章](https://docs.microsoft.com/en-us/azure/documentdb/documentdb-change-feed)。 

##### <a name="getnewormodifiedfhirdocuments-operation"></a>GetNewOrModifiedFhirDocuments 操作

**输入**
- 数据库 ID
- 集合 ID
- HL7 FHIR 资源类型名称
- 布尔：从头开始
- 整型：返回的文档数目

**输出**
- 成功：状态代码：200，响应：文档列表（JSON 数组）
- 失败：状态代码：404，响应：“没有找到‘资源名称’资源类型的文档”

<a id="api-app-source"></a>

**API 应用的源**

```C#

    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Web.Http;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Swashbuckle.Swagger.Annotations;
    using TRex.Metadata;
    
    namespace FhirNotificationApi.Controllers
    {
        /// <summary>
        ///     FHIR Resource Type Controller
        /// </summary>
        /// <seealso cref="System.Web.Http.ApiController" />
        public class FhirResourceTypeController : ApiController
        {
            /// <summary>
            ///     Gets the new or modified FHIR documents from Last Run Date 
            ///        or create date of the collection
            /// </summary>
            /// <param name="databaseId"></param>
            /// <param name="collectionId"></param>
            /// <param name="resourceType"></param>
            /// <param name="startfromBeginning"></param>
            /// <param name="maximumItemCount">-1 returns all (default)</param>
            /// <returns></returns>
            [Metadata("Get New or Modified FHIR Documents",
                "Query for new or modifed FHIR Documents By Resource Type " +
                "from Last Run Date or Begining of Collection creation"
            )]
            [SwaggerResponse(HttpStatusCode.OK, type: typeof(Task<dynamic>))]
            [SwaggerResponse(HttpStatusCode.NotFound, "No New or Modifed Documents found")]
            [SwaggerOperation("GetNewOrModifiedFHIRDocuments")]
            public async Task<dynamic> GetNewOrModifiedFhirDocuments(
                [Metadata("Database Id", "Database Id")] string databaseId,
                [Metadata("Collection Id", "Collection Id")] string collectionId,
                [Metadata("Resource Type", "FHIR resource type name")] string resourceType,
                [Metadata("Start from Beginning ", "Change Feed Option")] bool startfromBeginning,
                [Metadata("Maximum Item Count", "Number of documents returned. '-1 returns all' (default)")] int maximumItemCount = -1
            )
            {
                var collectionLink = UriFactory.CreateDocumentCollectionUri(databaseId, collectionId);
    
                var context = new DocumentDbContext();    
    
                var docs = new List<dynamic>();
    
                var partitionKeyRanges = new List<PartitionKeyRange>();
                FeedResponse<PartitionKeyRange> pkRangesResponse;
    
                do
                {
                    pkRangesResponse = await context.Client.ReadPartitionKeyRangeFeedAsync(collectionLink);
                    partitionKeyRanges.AddRange(pkRangesResponse);
                } while (pkRangesResponse.ResponseContinuation != null);
    
                foreach (var pkRange in partitionKeyRanges)
                {
                    var changeFeedOptions = new ChangeFeedOptions
                    {
                        StartFromBeginning = startfromBeginning,
                        RequestContinuation = null,
                        MaxItemCount = maximumItemCount,
                        PartitionKeyRangeId = pkRange.Id
                    };
    
                    using (var query = context.Client.CreateDocumentChangeFeedQuery(collectionLink, changeFeedOptions))
                    {
                        do
                        {
                            if (query != null)
                            {
                                var results = await query.ExecuteNextAsync<dynamic>().ConfigureAwait(false);
                                if (results.Count > 0)
                                    docs.AddRange(results.Where(doc => doc.resourceType == resourceType));
                            }
                            else
                            {
                                throw new HttpResponseException(new HttpResponseMessage(HttpStatusCode.NotFound));
                            }
                        } while (query.HasMoreResults);
                    }
                }
                if (docs.Count > 0)
                    return docs;
                var msg = new StringContent("No documents found for " + resourceType + " Resource");
                var response = new HttpResponseMessage
                {
                    StatusCode = HttpStatusCode.NotFound,
                    Content = msg
                };
                return response;
            }
        }
    }
    
```

### <a name="testing-the-fhirnotificationapi"></a>测试 FhirNotificationApi 

下图展示了如何将 Swagger 用于测试 [FhirNotificationApi](#api-app-source)。

![用于测试 API 应用的 Swagger 文件](./media/documentdb-change-feed-hl7-fhir-logic-apps/documentdb-hl7-fhir-testing-app.png)


### <a name="azure-portal-dashboard"></a>Azure 门户仪表板

下图显示在 Azure 门户中运行的此解决方案的所有 Azure 服务。

![Azure 门户显示在此 HL7 FHIR 教程中使用的所有服务](./media/documentdb-change-feed-hl7-fhir-logic-apps/documentdb-hl7-fhir-portal.png)


## <a name="summary"></a>摘要

- 已了解到了 DocumentDB 具有对新文档或修改后文档的通知的本机支持，以及使用此功能非常简单。 
- 通过利用逻辑应用，可以创建工作流，而无需编写任何代码。
- 使用 Azure 服务总线队列处理 HL7 FHIR 文档的分布。

## <a name="next-steps"></a>后续步骤
有关 DocumentDB 的详细信息，请参阅 [DocumentDB 主页](https://azure.microsoft.com/en-us/services/documentdb/)。 有关逻辑应用的详细信息，请参阅[逻辑应用](https://azure.microsoft.com/en-us/services/logic-apps/)。





<!--HONumber=Feb17_HO2-->

