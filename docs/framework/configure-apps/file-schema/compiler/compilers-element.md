---
title: <compilers> 元素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#compilers
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.codedom/compilers
helpviewer_keywords:
- compiler configuration elements, <compilers> element
- <compilers> element
- compilers element
ms.assetid: d40fba59-98f9-4783-ae0c-2ebea27ce77b
ms.openlocfilehash: 744ef0d9bc58e6a0152dce53c40c24eb5283dc0f
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61705332"
---
# <a name="compilers-element"></a>\<编译器 > 元素
编译器配置元素的容器；包含零个或多个 [\<compiler>](../../../../../docs/framework/configure-apps/file-schema/compiler/compiler-element.md) 元素。  
  
 \<configuration>  
\<system.codedom>  
\<编译器 > 元素  
  
## <a name="syntax"></a>语法  
  
```xml  
<compilers>  
  <compiler ... />  
</compilers>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  
 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
 无。  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<compiler> Element](../../../../../docs/framework/configure-apps/file-schema/compiler/compiler-element.md)|指定语言提供程序的编译器配置属性。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<configuration> 元素](../../../../../docs/framework/configure-apps/file-schema/configuration-element.md)|公共语言运行时和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|  
|[\<system.codedom > 元素](../../../../../docs/framework/configure-apps/file-schema/compiler/system-codedom-element.md)|指定可用语言提供程序的编译器配置设置。|  
  
## <a name="remarks"></a>备注  
 [\<编译器 >](../../../../../docs/framework/configure-apps/file-schema/compiler/compilers-element.md)元素包含的计算机上的语言提供程序的编译器配置设置。 每个[\<编译器 >](../../../../../docs/framework/configure-apps/file-schema/compiler/compiler-element.md)元素指定特定的语言提供程序的编译器配置属性。  
  
 .NET Framework 计算机配置文件 (Machine.config) 中定义初始编译器和语言提供程序设置。 开发人员和编译器供应商可以添加新 <xref:System.CodeDom.Compiler.CodeDomProvider?displayProperty=nameWithType> 实现的配置设置。 使用 <xref:System.CodeDom.Compiler.CodeDomProvider.GetAllCompilerInfo%2A?displayProperty=nameWithType> 方法，以编程方式枚举计算机上的语言提供程序和编译器配置设置。  
  
## <a name="configuration-file"></a>配置文件  
 计算机配置文件和应用程序配置文件中，可以使用此元素。  
  
## <a name="example"></a>示例  
 以下示例说明典型的编译器配置元素。  
  
```xml  
<configuration>  
   <system.codedom>  
     <compilers>  
       <!-- zero or more compiler elements -->  
       <compiler   
          language="c#;cs;csharp"   
          extension=".cs"  
          type="Microsoft.CSharp.CSharpCodeProvider, System, Version=1.0.5000.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"  
          compilerOptions=""    
          warningLevel="1" />  
     </compilers>  
   </system.codedom>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.CodeDom.Compiler.CompilerInfo>
- <xref:System.CodeDom.Compiler.CodeDomProvider>
- [配置文件架构](../../../../../docs/framework/configure-apps/file-schema/index.md)
- [编译器和语言提供程序设置架构](../../../../../docs/framework/configure-apps/file-schema/compiler/index.md)
- [\<compiler> Element](../../../../../docs/framework/configure-apps/file-schema/compiler/compiler-element.md)
