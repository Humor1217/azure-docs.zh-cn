---
title: 新闻搜索 SDK Node 快速入门 | Microsoft Docs
description: 设置新闻搜索 SDK 控制台应用程序。
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 4ae99aa100b697a0dd75863c6f0c3c556dfa3d21
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2018
ms.locfileid: "35366695"
---
# <a name="news-search-sdk-node-quickstart"></a>新闻搜索 SDK Node 快速入门

必应新闻搜索 SDK 包含用于新闻查询以及对结果进行分析的 REST API 功能。 

Git Hub 上提供了 [Node 必应新闻搜索 SDK 示例的源代码](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js)。

## <a name="application-dependencies"></a>应用程序依赖项

若要使用必应新闻搜索 SDK 设置控制台应用程序，请在开发环境中运行 `npm install azure-cognitiveservices-newssearch`。

## <a name="news-search-client"></a>新闻搜索客户端
在“搜索”下获取[认知服务访问密钥](https://azure.microsoft.com/try/cognitive-services/)。 创建 `CognitiveServicesCredentials` 的实例：
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
然后对该客户端进行实例化：
```
const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
let client = new NewsSearchAPIClient(credentials);
```
使用客户端通过查询文本进行搜索，在本例中查询文本为“Winter Olympics”：
```
client.newsOperations.search('Winter Olympics').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```
代码会将 `result.value` 项输出至控制台，并且不会分析任何文本。 结果（如果每个类别都有结果）将包括：
- _type: 'NewsArticle'
- _type: 'WebPage'
- _type: 'VideoObject'
- _type: 'ImageObject'

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>后续步骤

[认知服务 Node.js SDK 示例](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
