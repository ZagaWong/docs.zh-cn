---
title: 如何：实现静态分区的分区程序
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- tasks, how to create a static partitioner
ms.assetid: f4410508-cac6-4ba7-bef1-c5e68b2794f3
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 302d7d98d04e528d205edf38c3fa13bb3f2b2252
ms.sourcegitcommit: a885cc8c3e444ca6471348893d5373c6e9e49a47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2018
ms.locfileid: "43863068"
---
# <a name="how-to-implement-a-partitioner-for-static-partitioning"></a>如何：实现静态分区的分区程序
下面的示例展示了一种为执行静态分区的 PLINQ 实现简单自定义分区程序的方法。 由于分区程序不支持动态分区，因此无法通过 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 使用它。 对于每个元素需要越来越多处理时间的数据源，此分区程序可能会让速度提升（与默认范围分区程序相比）。  
  
## <a name="example"></a>示例  
 [!code-csharp[TPL_Partitioners#05](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_partitioners/cs/partitioners.cs#05)]  
  
 此示例中的分区都基于每个元素的处理时间呈线性增加的假设。 实际上，按这种方式预测处理时间可能会很困难。 如果将静态分区程序与特定数据源结合使用，可以优化源的分区公式，也可以添加负载均衡逻辑，或使用区块分区方法（如[如何：实现动态分区](../../../docs/standard/parallel-programming/how-to-implement-dynamic-partitions.md)中所述）。  
  
## <a name="see-also"></a>请参阅

- [PLINQ 和 TPL 的自定义分区程序](../../../docs/standard/parallel-programming/custom-partitioners-for-plinq-and-tpl.md)
