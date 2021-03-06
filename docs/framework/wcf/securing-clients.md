---
title: 保证客户端的安全
ms.date: 03/30/2017
helpviewer_keywords:
- clients [WCF], security considerations
ms.assetid: 44c8578c-9a5b-4acd-8168-1c30a027c4c5
ms.openlocfilehash: b357ee12dce823e49e61171d21356ca36b74f7c5
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59331801"
---
# <a name="securing-clients"></a>保证客户端的安全
在 Windows Communication Foundation (WCF)，由服务规定客户端的安全要求。 即，由服务指定要使用的安全模式以及客户端是否必须提供凭据。 因此，保证客户端安全的过程非常简单：使用从服务那里获得的元数据（如果已发布）来生成客户端。 元数据指定如何配置客户端。 如果服务要求客户端提供凭据，您必须获得能够满足要求的凭据。 本主题进一步详细讨论此过程。 有关创建安全服务的详细信息，请参阅[Securing Services](../../../docs/framework/wcf/securing-services.md)。  
  
## <a name="the-service-specifies-security"></a>服务指定安全性  
 默认情况下，WCF 绑定已启用的安全功能。 （<xref:System.ServiceModel.BasicHttpBinding> 是一个例外。）因此，如果服务使用 WCF 创建的是更好地实现安全性，以确保身份验证、 保密性和完整性。 在这种情况下，服务提供的元数据将指示建立安全通信通道需要哪些条件。 如果服务元数据不包含安全要求，则无法对服务强行实施安全方案，例如 Secure Sockets Layer (SSL) over HTTP。 但是，如果服务要求客户端提供凭据，则客户端开发人员、部署人员或管理员必须提供客户端用来向该服务证明自己身份的实际凭据。  
  
## <a name="obtaining-metadata"></a>获取元数据  
 创建客户端时，第一步是获取客户端将与其通信的服务的元数据。 有两种方法可以做到这一点。 首先，如果该服务发布元数据交换 (MEX) 终结点，或者使其元数据可通过 HTTP 或 HTTPS，则可以下载元数据中使用[ServiceModel Metadata Utility Tool (Svcutil.exe)](../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)，以生成同时客户端，以及配置文件的代码文件。 (有关使用该工具的详细信息，请参阅[使用 WCF 客户端访问服务](../../../docs/framework/wcf/accessing-services-using-a-wcf-client.md)。)其次，如果服务没有发布 MEX 终结点，也没有使自己的元数据可通过 HTTP 或 HTTPS 获得，您必须与服务创建者联系，以获得描述安全要求和元数据的文档。  
  
> [!IMPORTANT]
>  建议使用来自受信任的源且未被篡改的元数据。 使用 HTTP 协议检索到的元数据是以明文形式发送的，可能被篡改。 如果服务使用 <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A> 和 <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetUrl%2A> 属性，请根据服务创建者提供的 URL，使用 HTTPS 协议下载数据。  
  
## <a name="validating-security"></a>验证安全性  
 元数据源可以分为两大类：受信任的源和不受信任的源。 如果信任某个源，并且已从该源的安全 MEX 终结点下载客户端代码和其他元数据，则可以生成客户端，为其提供正确的凭据，然后无所顾忌地运行该客户端。  
  
 但是，如果您选择从一个知之甚少的源下载客户端和元数据，请务必验证代码所使用的安全措施。 例如，一定不要简单地创建一个向服务发送个人或财务信息的客户端，除非该服务要求保密性和完整性（这是最基本的要求）。 您对服务所有者的信任应达到您愿意向其公开这些信息的程度，因为他或她将能够看到这些信息。  
  
 因此，请记住如下规则：在使用来自不受信任的源的代码和元数据时，应检查这些代码和元数据，以确保它们满足你所要求的安全级别。  
  
## <a name="setting-a-client-credential"></a>设置客户端凭据  
 在客户端上设置客户端凭据的过程包含两个步骤：  
  
1. 确定*客户端凭据类型*服务要求。 这是通过两种方法之一完成的。 首先，如果您拥有来自服务创建者的文档，则其中应指定了服务所要求的客户端凭据类型（如果有要求的话）。 其次，如果您仅拥有由 Svcutil.exe 工具生成的配置文件，则可以检查各个绑定，以确定要求何种凭据类型。  
  
2. 指定一个实际客户端凭据。 实际的客户端凭据称为*客户端凭据值*类型区分开来。 例如，如果客户端凭据类型指定了证书，您必须提供由该服务所信任的证书颁发机构颁发的 X.509 证书。  
  
### <a name="determining-the-client-credential-type"></a>确定客户端凭据类型  
 如果有配置文件由 Svcutil.exe 工具生成，请检查[\<绑定 >](../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)部分可以确定哪些客户端凭据类型是必需的。 该节包含指定安全需求的绑定元素。 具体而言，检查\<安全 > 的每个绑定元素。 该元素包含 `mode` 属性，该属性可设置为以下三个可能的值之一：`Message`、`Transport` 或 `TransportWithMessageCredential`。 该属性的值确定模式，而模式则确定哪个子元素有效。  
  
 `<security>`元素可以包含`<transport>`或`<message>`元素，或两者。 有效元素是指与安全模式匹配的那个元素。 例如，下面的代码指定安全模式为 `"Message"`，`<message>` 元素的客户端凭据类型为 `"Certificate"`。 在这种情况下，`<transport>` 元素可以忽略。 但是，`<message>` 元素指定必须提供 X.509 证书。  

```xml  
<wsHttpBinding>  
    <binding name="WSHttpBinding_ICalculator">  
       <security mode="Message">  
           <transport clientCredentialType="Windows"   
                      realm="" />  
           <message clientCredentialType="Certificate"   
                    negotiateServiceCredential="true"  
                    algorithmSuite="Default"   
                    establishSecurityContext="true" />  
       </security>  
    </binding>  
</wsHttpBinding>  
```  

 请注意，如果按照下面的示例所示，将 `clientCredentialType` 属性设置为 `"Windows"`，则无需提供实际凭据值。 这是因为，Windows 集成安全性提供了正在运行客户端的用户的实际凭据（一个 Kerberos 令牌）。  
  
```xml  
<security mode="Message">  
    <transport clientCredentialType="Windows "   
        realm="" />  
</security>  
```  
  
### <a name="setting-the-client-credential-value"></a>设置客户端凭据值  
 如果确定客户端必须提供凭据，请使用适当的方法来配置客户端。 例如，若要设置客户端证书，请使用 <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential.SetCertificate%2A> 方法。  
  
 一种常见的证书形式是 X.509 证书。 可以通过两种方式来提供凭据：  
  
-   在客户端代码中对其进行编程（使用 `SetCertificate` 方法）。  
  
 通过添加[\<行为 >](../../../docs/framework/configure-apps/file-schema/wcf/behaviors.md)部分中的客户端的配置文件和使用`clientCredentials`元素 （如下所示）。  
  
#### <a name="setting-a-clientcredentials-value-in-code"></a>设置\<clientCredentials > 在代码中的值  
 若要设置[ \<clientCredentials >](../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md)值在代码中，你必须访问<xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A>属性<xref:System.ServiceModel.ClientBase%601>类。 该属性返回一个 <xref:System.ServiceModel.Description.ClientCredentials> 对象，使用该对象可以访问各种凭据类型，如下表所示。  
  
|ClientCredential 属性|描述|说明|  
|-------------------------------|-----------------|-----------|  
|<xref:System.ServiceModel.Description.ClientCredentials.ClientCertificate%2A>|返回一个 <xref:System.ServiceModel.Security.X509CertificateInitiatorClientCredential>|表示客户端提供的 X.509 证书，客户端使用该证书向服务器证明自己的身份。|  
|<xref:System.ServiceModel.Description.ClientCredentials.HttpDigest%2A>|返回一个 <xref:System.ServiceModel.Security.HttpDigestClientCredential>|表示 HTTP 摘要式凭据。 该凭据是用户名和密码的哈希。|  
|<xref:System.ServiceModel.Description.ClientCredentials.IssuedToken%2A>|返回一个 <xref:System.ServiceModel.Security.IssuedTokenClientCredential>|表示由安全令牌服务颁发的一个自定义安全令牌，通常用在联合方案中。|  
|<xref:System.ServiceModel.Description.ClientCredentials.Peer%2A>|返回一个 <xref:System.ServiceModel.Security.PeerCredential>|表示一个用于参与 Windows 域上的对等网格的对等凭据。|  
|<xref:System.ServiceModel.Description.ClientCredentials.ServiceCertificate%2A>|返回一个 <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential>|表示服务在带外协商中提供的 X.509 证书。|  
|<xref:System.ServiceModel.Description.ClientCredentials.UserName%2A>|返回一个 <xref:System.ServiceModel.Security.UserNamePasswordClientCredential>|表示一个用户名和密码对。|  
|<xref:System.ServiceModel.Description.ClientCredentials.Windows%2A>|返回一个 <xref:System.ServiceModel.Security.WindowsClientCredential>|表示 Windows 客户端凭据（Kerberos 凭据）。 该类的属性是只读的。|  
  
#### <a name="setting-a-clientcredentials-value-in-configuration"></a>设置\<clientCredentials > 配置中的值  
 凭据值指定为的子元素使用的终结点行为[ \<clientCredentials >](../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md)元素。 所使用的元素取决于客户端凭据类型。 例如，下面的示例演示设置 X.509 证书使用的配置 <[\<clientCertificate >](../../../docs/framework/configure-apps/file-schema/wcf/clientcertificate-of-clientcredentials-element.md)。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <endpointBehaviors>  
        <behavior name="myEndpointBehavior">  
          <clientCredentials>  
            <clientCertificate findvalue="myMachineName"   
            storeLocation="Current" X509FindType="FindBySubjectName" />  
          </clientCredentials>  
        </behavior>              
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
 若要在配置中设置客户端凭据，添加[ \<endpointBehaviors >](../../../docs/framework/configure-apps/file-schema/wcf/endpointbehaviors.md)到配置文件的元素。 此外，必须添加的行为元素链接到服务的终结点使用`behaviorConfiguration`的属性[\<终结点 > 的\<客户端 >](../configure-apps/file-schema/wcf/endpoint-of-client.md)元素，如以下示例所示。 `behaviorConfiguration` 属性的值必须与行为 `name` 属性的值相匹配。  

```xml
<configuration>
  <system.serviceModel>
    <client>
      <endpoint address="http://localhost/servicemodelsamples/service.svc"
                binding="wsHttpBinding"
                bindingConfiguration="Binding1"
                behaviorConfiguration="myEndpointBehavior"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </client>
  </system.serviceModel>
</configuration>
```
  
> [!NOTE]
>  有些客户端凭据值无法使用应用程序配置文件来设置，例如，用户名和密码值或 Windows 用户和密码值。 这种凭据值只能在代码中指定。  
  
 有关设置客户端凭据的详细信息，请参阅[如何：指定客户端凭据值](../../../docs/framework/wcf/how-to-specify-client-credential-values.md)。  
  
> [!NOTE]
>  当 `ClientCredentialType` 设置为 `SecurityMode` 时，`"TransportWithMessageCredential",` 将被忽略，如下面的示例配置所示。  
  
```xml  
<wsHttpBinding>  
    <binding name="PingBinding">  
        <security mode="TransportWithMessageCredential"  >  
           <message  clientCredentialType="UserName"   
               establishSecurityContext="false"    
               negotiateServiceCredential="false" />  
           <transport clientCredentialType="Certificate"  />  
         </security>  
    </binding>  
</wsHttpBinding>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A>
- <xref:System.ServiceModel.ClientBase%601>
- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A>
- <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetUrl%2A>
- [\<bindings>](../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)
- [配置编辑器工具 (SvcConfigEditor.exe)](../../../docs/framework/wcf/configuration-editor-tool-svcconfigeditor-exe.md)
- [保护服务](../../../docs/framework/wcf/securing-services.md)
- [使用 WCF 客户端访问服务](../../../docs/framework/wcf/accessing-services-using-a-wcf-client.md)
- [如何：指定客户端凭据值](../../../docs/framework/wcf/how-to-specify-client-credential-values.md)
- [ServiceModel 元数据实用工具 (Svcutil.exe)](../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)
- [如何：指定客户端凭据类型](../../../docs/framework/wcf/how-to-specify-the-client-credential-type.md)
