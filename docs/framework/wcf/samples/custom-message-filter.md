---
title: 自定义消息筛选器
ms.date: 03/30/2017
ms.assetid: 98dd0af8-fce6-4255-ac32-42eb547eea67
ms.openlocfilehash: 34e6d851bd0aa3515c5c43521be6213451b7ed12
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59773072"
---
# <a name="custom-message-filter"></a>自定义消息筛选器
此示例演示如何替换 Windows Communication Foundation (WCF) 用于将消息调度到终结点的消息筛选器。  
  
> [!NOTE]
>  本主题的最后介绍了此示例的设置过程和生成说明。  
  
 当通道上的第一个消息到达服务器时，服务器必须确定哪个（如果有）与该 URI 关联的终结点应该接收消息。 此过程受附加到 <xref:System.ServiceModel.Dispatcher.MessageFilter> 的 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher> 对象控制。  
  
 服务的每个终结点都有一个 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher>。 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher> 同时具有 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.AddressFilter%2A> 和 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.ContractFilter%2A>。 这两个筛选器的联合是用于该终结点的消息筛选器。  
  
 默认情况下，终结点的 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.AddressFilter%2A> 与发送到某个地址的任何消息匹配，该地址与服务终结点的 <xref:System.ServiceModel.EndpointAddress> 的匹配。 默认情况下<xref:System.ServiceModel.Dispatcher.EndpointDispatcher.ContractFilter%2A>终结点会检查传入消息的操作和操作时，对应于服务终结点协定的操作的操作之一的任何消息匹配 (仅`IsInitiating` = `true`操作可视为)。 因此，默认情况下，仅当消息的 To 标头为终结点的 <xref:System.ServiceModel.EndpointAddress> 并且消息的动作与终结点操作的动作之一匹配时，终结点的筛选器才与此消息匹配。  
  
 使用行为可以更改这些筛选器。 在本示例中，服务创建一个 <xref:System.ServiceModel.Description.IEndpointBehavior>，该行为替换 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.AddressFilter%2A> 上的 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.ContractFilter%2A> 和 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher>：  
  
```  
class FilteringEndpointBehavior : IEndpointBehavior …  
```  
  
 定义两个地址筛选器：  
  
```  
// Matches any message whose To address contains the letter 'e'  
class MatchEAddressFilter : MessageFilter …  
// Matches any message whose To address does not contain the letter 'e'  
class MatchNoEAddressFilter : MessageFilter  
```  
  
 使 `FilteringEndpointBehavior` 变得可配置并允许两个不同变体。  
  
```  
public class FilteringEndpointBehaviorExtension : BehaviorExtensionElement  
```  
  
 变体 1 仅匹配包含“e”的地址（但具有任何操作），而变体 2 仅匹配不含“e”的地址：  
  
```  
if (Variation == 1)  
    return new FilteringEndpointBehavior(  
        new MatchEAddressFilter(), new MatchAllMessageFilter());  
else  
    return new FilteringEndpointBehavior(  
        new MatchNoEAddressFilter(), new MatchAllMessageFilter());  
```  
  
 在配置文件中，服务注册新行为：  
  
```xml  
<extensions>  
    <behaviorExtensions>  
        <add name="filteringEndpointBehavior" type="Microsoft.ServiceModel.Samples.FilteringEndpointBehaviorExtension, service" />  
    </behaviorExtensions>  
</extensions>      
```  
  
 然后，服务为每个变体创建 `endpointBehavior` 配置：  
  
```xml  
<endpointBehaviors>  
    <behavior name="endpoint1">  
        <filteringEndpointBehavior variation="1" />  
    </behavior>  
    <behavior name="endpoint2">  
        <filteringEndpointBehavior variation="2" />  
    </behavior>  
</endpointBehaviors>  
```  
  
 最后，服务的终结点引用 `behaviorConfigurations` 之一：  
  
```xml  
<endpoint address=""  
        bindingConfiguration="ws"  
        listenUri=""   
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.IHello"   
        behaviorConfiguration="endpoint2" />  
```  
  
 客户端应用程序的实现非常简单；它通过将该值作为到 `via` 的第二个 (<xref:System.ServiceModel.Channels.IChannelFactory%601.CreateChannel%28System.ServiceModel.EndpointAddress%29>) 参数传入并在每个通道上发送单个消息，但分别使用不同的终结点地址，创建两个到达服务的 URI 的通道。 因此，来自客户端的出站消息具有不同的 To 标记，服务器做出相应响应，如客户端的输出所示：  
  
```  
Sending message to urn:e...  
Exception: The message with To 'urn:e' cannot be processed at the receiver, due to an AddressFilter mismatch at the EndpointDispatcher.  Check that the sender and receiver's EndpointAddresses agree.  
  
Sending message to urn:a...  
Hello  
```  
  
 切换服务器配置文件中的变体导致交换筛选器，客户端看到相反的行为（到 `urn:e` 的消息成功，而到 `urn:a` 的消息失败）。  
  
```xml  
<endpoint address=""  
          bindingConfiguration="ws"  
          listenUri=""   
          binding="wsHttpBinding"  
          contract="Microsoft.ServiceModel.Samples.IHello"   
          behaviorConfiguration="endpoint1" />  
```  
  
> [!IMPORTANT]
>  您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  如果此目录不存在，请转到[Windows Communication Foundation (WCF) 和.NET Framework 4 的 Windows Workflow Foundation (WF) 示例](https://go.microsoft.com/fwlink/?LinkId=150780)若要下载所有 Windows Communication Foundation (WCF) 和[!INCLUDE[wf1](../../../../includes/wf1-md.md)]示例。 此示例位于以下目录：  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageFilter`  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 若要生成解决方案，请按照中的说明[生成 Windows Communication Foundation 示例](../../../../docs/framework/wcf/samples/building-the-samples.md)。  
  
2. 若要在单计算机配置中运行示例，请按照中的说明[运行 Windows Communication Foundation 示例](../../../../docs/framework/wcf/samples/running-the-samples.md)。  
  
3. 若要跨计算机配置中运行示例，请按照的说明[运行 Windows Communication Foundation 示例](../../../../docs/framework/wcf/samples/running-the-samples.md)并更改 Client.cs 中的以下行。  
  
    ```  
    Uri serviceVia = new Uri("http://localhost/ServiceModelSamples/service.svc");  
    ```  
  
     用服务器的名称替换 localhost。  
  
    ```  
    Uri serviceVia = new Uri("http://servermachinename/ServiceModelSamples/service.svc");  
    ```  
