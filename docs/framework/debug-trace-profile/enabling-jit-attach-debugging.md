---
title: 启用 JIT 附加调试
ms.date: 03/30/2017
helpviewer_keywords:
- JIT-attach debugging
- debugging [.NET Framework], JIT-attach debugging
ms.assetid: f91fc5f7-de5a-4f23-b6ac-f450e63c662e
author: mairaw
ms.author: mairaw
ms.openlocfilehash: f1696f9054d44a5f80a1f67cc38e315a8627d295
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61874271"
---
# <a name="enabling-jit-attach-debugging"></a>启用 JIT 附加调试
JIT 附加调试是用于描述如何在发生错误时将调试器附加到进程的词组，它也可以由特定的方法或函数触发。  
  
 JIT 附加调试用于以下错误情况：  
  
- 未经处理的异常（在本机代码和托管代码中）。  
  
- <xref:System.Environment.FailFast%2A?displayProperty=nameWithType> 方法或 [RaiseFailFastException](https://go.microsoft.com/fwlink/?LinkId=182107) 函数（Windows 7 系列）。  
  
- 运行时灾难性错误。  
  
 也可通过调用以下方法和函数来触发 JIT 附加调试：  
  
- <xref:System.Diagnostics.Debugger.Launch%2A?displayProperty=nameWithType> 方法。  
  
- <xref:System.Diagnostics.Debugger.Break%2A?displayProperty=nameWithType> 方法。  
  
- [DebugBreak](https://go.microsoft.com/fwlink/?LinkId=182106) 函数 (Win32)。  
  
 [!INCLUDE[net_v40_long](../../../includes/net-v40-long-md.md)] 之前的 .NET Framework 提供单独的注册表项来控制本机调试器和托管调试器的行为。 从开始[!INCLUDE[net_v40_short](../../../includes/net-v40-short-md.md)]，控制合并在单个注册表项下：HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Current Version\AeDebug。 用户可为该注册表项设置值来确定是否调用调试器，如果调用，则确定是否使用需用户交互的对话框来调用。 有关设置此注册表项的信息，请参阅[配置自动调试](https://go.microsoft.com/fwlink/?LinkId=181767)。  
  
## <a name="see-also"></a>请参阅

- [调试、跟踪和分析](../../../docs/framework/debug-trace-profile/index.md)
- [使映像更易于调试](../../../docs/framework/debug-trace-profile/making-an-image-easier-to-debug.md)
