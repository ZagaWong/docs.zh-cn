---
title: 从 XAML 加载
ms.date: 03/30/2017
ms.assetid: 1f103ef6-7bed-4f16-ae52-9e665c5a43d7
ms.openlocfilehash: 5a3b3673812c0b5500a13ae9ce79ce8206aa4834
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59772851"
---
# <a name="load-from-xaml"></a>从 XAML 加载
此示例演示如何在不运行 XamlBuildTask 工具的情况下动态加载 XAML 工作流。 此示例改为调用 <xref:System.Activities.XamlIntegration.ActivityXamlServices.Load%2A> 方法。 该示例是一个 Windows Presentation Foundation (WPF) 客户端应用程序加载 XAML 工作流使用<xref:System.Activities.XamlIntegration.ActivityXamlServices>类并执行它们。 在使用 <xref:System.Activities.XamlIntegration.ActivityXamlServices> 类加载完这些工作流后，将返回一个可执行的 <xref:System.Activities.DynamicActivity%601>。

#### <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2010 打开 LoadFromXAML.sln 解决方案文件。

2. 要生成解决方案，按 Ctrl+Shift+B。

3. 若要运行解决方案，请按 Ctrl+F5。

> [!IMPORTANT]
>  您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  如果此目录不存在，请转到[Windows Communication Foundation (WCF) 和.NET Framework 4 的 Windows Workflow Foundation (WF) 示例](https://go.microsoft.com/fwlink/?LinkId=150780)若要下载所有 Windows Communication Foundation (WCF) 和[!INCLUDE[wf1](../../../../includes/wf1-md.md)]示例。 此示例位于以下目录：  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Built-InActivities\DynamicActivity\LoadFromXAML`