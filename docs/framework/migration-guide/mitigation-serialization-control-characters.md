---
title: 缓解：使用 DataContractJsonSerializer 对控制字符进行序列化
ms.date: 04/07/2017
helpviewer_keywords:
- .NET Framework 4.7 retargeting changes
- retargeting changes
- DataContractJsonSerializer changes
- serialization changes
ms.assetid: e065d458-a128-44f2-9f17-66af9d5be954
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 3355841298e039652eb81918eac98186c1a1f833
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59182112"
---
# <a name="mitigation-serialization-of-control-characters-with-the-datacontractjsonserializer"></a>缓解：使用 DataContractJsonSerializer 对控制字符进行序列化

自 .NET Framework 4.7 起，使用 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 对控制字符进行序列化的方式已更改为与 ECMAScript V6 和 V8 兼容。 
 
## <a name="impact"></a>影响

在 .NET framework 4.6.2 及更低版本中，<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 未按与 ECMAScript V6 和 V8 标准兼容的方式对一些特殊控制字符（如 `\b`、`\f` 和 `\t`）进行序列化。

对于定位 .NET Framework 4.7 及更高版本的应用程序，这些控制字符的序列化与 ECMAScript V6 和 V8 兼容。 以下 API 受到影响：

- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject%2A> 
- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject%2A>
- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject%2A>

## <a name="mitigation"></a>缓解

对于定位 .NET Framework 4.7 及更高版本的应用程序，此行为默认启用。

如果不需要此行为，可以在 app.config 或 web.config 文件的 `<runtime>` 部分中添加下面的代码行，从而选择禁用此功能：

```xml
<runtime>
   <AppContextSwitchOverrides value="Switch.System.Runtime.Serialization.DoNotUseECMAScriptV6EscapeControlCharacter=false" />
</runtime>
```
 
## <a name="see-also"></a>请参阅

- [.NET Framework 4.7 中的重定目标更改](../../../docs/framework/migration-guide/retargeting-changes-in-the-net-framework-4-7.md)
