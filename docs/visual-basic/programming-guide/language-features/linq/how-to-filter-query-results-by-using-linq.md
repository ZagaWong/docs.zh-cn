---
title: 如何：使用 LINQ (Visual Basic) 筛选查询结果
ms.date: 07/20/2015
helpviewer_keywords:
- filtering [Visual Basic]
- filtering data [LINQ in Visual Basic]
- filtering [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], filtering results
- querying databases [LINQ]
- queries [LINQ in Visual Basic], how-to topics
- query samples [Visual Basic]
- filtering data [Visual Basic]
ms.assetid: ef103092-9bed-4134-97f4-2db696e83c12
ms.openlocfilehash: fc4d43ef9181f1a290d37c137b4fc6f7f16588b7
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59332048"
---
# <a name="how-to-filter-query-results-by-using-linq-visual-basic"></a>如何：使用 LINQ (Visual Basic) 筛选查询结果
语言集成查询 (LINQ) 轻松地访问数据库的信息和执行查询。  
  
 下面的示例演示如何创建新应用程序对 SQL Server 数据库执行查询并使用特定值筛选结果`Where`子句。 有关详细信息，请参阅[Where 子句](../../../../visual-basic/language-reference/queries/where-clause.md)。  
  
 本主题中的示例使用 Northwind 示例数据库。 如果在开发计算机上没有此数据库，您可以从 Microsoft 下载中心下载它。 有关说明，请参阅[下载示例数据库](../../../../framework/data/adonet/sql/linq/downloading-sample-databases.md)。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-create-a-connection-to-a-database"></a>若要创建与数据库的连接  
  
1. 在 Visual Studio 中打开**服务器资源管理器**/**数据库资源管理器**通过单击**服务器资源管理器**/**数据库资源管理器**上**视图**菜单。  
  
2. 右键单击**数据连接**中**服务器资源管理器**/**数据库资源管理器**，然后单击**添加连接**。  
  
3. 指定到 Northwind 示例数据库的有效连接。  
  
### <a name="to-add-a-project-that-contains-a-linq-to-sql-file"></a>若要添加包含 LINQ to SQL 文件的项目  
  
1. 在 Visual Studio 中的“文件”菜单上，指向“新建”，然后单击“项目”。 选择 Visual Basic **Windows 窗体应用程序**作为项目类型。  
  
2. 在 **“项目”** 菜单上，单击 **“添加新项”**。 选择**LINQ to SQL 类**项模板。  
  
3. 为 `northwind.dbml` 文件命名。 单击 **添加**。 此时，northwind.dbml 文件将打开对象关系设计器 （O/R 设计器）。  
  
### <a name="to-add-tables-to-query-to-the-or-designer"></a>若要添加到 O/R 设计器查询的表  
  
1. 在中**服务器资源管理器**/**数据库资源管理器**，扩展连接到 Northwind 数据库。 展开**表**文件夹。  
  
     如果已关闭 O/R 设计器，您可以通过双击前面添加的 northwind.dbml 文件重新打开它。  
  
2. 单击客户表并将其拖到设计器的左窗格中。 单击订单表并将其拖到设计器的左窗格中。  
  
     在设计器创建新`Customer`和`Order`为你的项目的对象。 请注意，在设计器会自动检测表之间的关系并创建子相关对象的属性。 例如，IntelliSense 将显示`Customer`对象具有`Orders`与该客户相关的所有订单的属性。  
  
3. 保存所做的更改并关闭设计器。  
  
4. 保存你的项目。  
  
### <a name="to-add-code-to-query-the-database-and-display-the-results"></a>添加代码以查询数据库并显示结果  
  
1. 从**工具箱**，拖动<xref:System.Windows.Forms.DataGridView>控件拖到你的项目，Form1 的默认 Windows 窗体上。  
  
2. 双击 Form1 以将代码添加到`Load`窗体的事件。  
  
3. 当表添加到 O/R 设计器时，在设计器添加<xref:System.Data.Linq.DataContext>为你的项目的对象。 此对象包含必须具有访问这些表，除了单个对象和集合的每个表的代码。 <xref:System.Data.Linq.DataContext>对象将项目命名为根据.dbml 文件的名称。 对于此项目，<xref:System.Data.Linq.DataContext>对象被命名为`northwindDataContext`。  
  
     可以创建的实例<xref:System.Data.Linq.DataContext>代码和查询中指定由 O/R 设计器的表。  
  
     将以下代码添加到`Load`事件以查询作为数据上下文的属性公开的表。 查询筛选结果，并返回位于中的客户`London`。  
  
     [!code-vb[VbLINQToSQLHowTos#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form5.vb#11)]  
  
4. 按 F5 以运行您的项目并查看结果。  
  
5. 以下是可以尝试一些其他筛选器。  
  
     [!code-vb[VbLINQToSQLHowTos#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form5.vb#12)]  
  
## <a name="see-also"></a>请参阅

- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [查询](../../../../visual-basic/language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [DataContext 方法（O/R 设计器）](/visualstudio/data-tools/datacontext-methods-o-r-designer)
