---
title: 如何：确保 GridSplitter 可见
ms.date: 03/30/2017
helpviewer_keywords:
- GridSplitter control [WPF], ensuring visibility of
ms.assetid: 0a62a964-89c8-48f0-9023-5df721a8cf47
ms.openlocfilehash: b7543d14ba39d854b5a2c3f4d0d19b9a457ea89b
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61770846"
---
# <a name="how-to-make-sure-that-a-gridsplitter-is-visible"></a>如何：确保 GridSplitter 可见
此示例演示如何以确保<xref:System.Windows.Controls.GridSplitter>中的其他控件不隐藏控件<xref:System.Windows.Controls.Grid>。  
  
## <a name="example"></a>示例  
 <xref:System.Windows.Controls.Panel.Children%2A>的<xref:System.Windows.Controls.Grid>标记或代码中定义的顺序呈现控件。 <xref:System.Windows.Controls.GridSplitter> 其他控件可以隐藏控件，如果您没有定义它们中的最后一个元素作为<xref:System.Windows.Controls.Panel.Children%2A>集合，或者如果您提供其他控制更高<xref:System.Windows.Controls.Panel.ZIndexProperty>。  
  
 若要防止隐藏<xref:System.Windows.Controls.GridSplitter>控件，执行下列操作之一。  
  
- 请确保<xref:System.Windows.Controls.GridSplitter>控件是最后<xref:System.Windows.Controls.Panel.Children%2A>添加到<xref:System.Windows.Controls.Grid>。 下面的示例演示<xref:System.Windows.Controls.GridSplitter>中的最后一个元素<xref:System.Windows.Controls.Panel.Children%2A>的集合<xref:System.Windows.Controls.Grid>。  
  
 [!code-xaml[GridSplitterSnips#GridSplitterLastChild](~/samples/snippets/csharp/VS_Snippets_Wpf/GridSplitterSnips/CSharp/Window1.xaml#gridsplitterlastchild)]  
  
- 设置<xref:System.Windows.Controls.Panel.ZIndexProperty>上<xref:System.Windows.Controls.GridSplitter>大于一个控件，否则将其隐藏。 下面的示例会<xref:System.Windows.Controls.GridSplitter>控制更高<xref:System.Windows.Controls.Panel.ZIndexProperty>比<xref:System.Windows.Controls.Button>控件。  
  
 [!code-xaml[GridSplitterSnips#GridSplitterZIndex](~/samples/snippets/csharp/VS_Snippets_Wpf/GridSplitterSnips/CSharp/Window1.xaml#gridsplitterzindex)]  
  
- 否则会隐藏在控件上设置边距<xref:System.Windows.Controls.GridSplitter>，以便<xref:System.Windows.Controls.GridSplitter>得以实现。 下面的示例设置上一个控件，否则会覆盖和隐藏边距<xref:System.Windows.Controls.GridSplitter>。  
  
 [!code-xaml[GridSplitterSnips#GridSplitterMargin](~/samples/snippets/csharp/VS_Snippets_Wpf/GridSplitterSnips/CSharp/Window1.xaml#gridsplittermargin)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Windows.Controls.GridSplitter>
- [帮助主题](gridsplitter-how-to-topics.md)
