---
title: 将不会从此事件处理程序中移除 Lambda 表达式
ms.date: 07/20/2015
f1_keywords:
- bc42326
- vbc42326
helpviewer_keywords:
- BC42326
ms.assetid: 63214dc6-0112-4245-8ebf-7c9e8f5a5782
ms.openlocfilehash: 20e83306925e91e579aca52f2e7c209c8c686dee
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "58817590"
---
# <a name="lambda-expression-will-not-be-removed-from-this-event-handler"></a>将不会从此事件处理程序中移除 Lambda 表达式
从此事件处理程序不会移除 lambda 表达式。 将 lambda 表达式分配给一个变量并将该变量来添加和删除该事件。  
  
 当事件处理程序中使用 lambda 表达式时，可能无法看到所需的行为。 编译器将生成新的方法对于每个 lambda 表达式定义，即使它们是完全相同。 因此，下面的代码显示`False`。  
  
```vb  
Module Module1  
  
    Sub Main()  
        Dim fun1 As ChangeInteger = Function(p As Integer) p + 1  
        Dim fun2 As ChangeInteger = Function(p As Integer) p + 1  
        Console.WriteLine(fun1 = fun2)  
    End Sub  
  
    Delegate Function ChangeInteger(ByVal x As Integer) As Integer  
  
End Module  
```  
  
 当事件处理程序中使用 lambda 表达式时，这可能会导致意外的结果。 在以下示例中，lambda 表达式添加`AddHandler`情况下不删除`RemoveHandler`语句。  
  
```vb  
Module Module1  
  
    Event ProcessInteger(ByVal x As Integer)  
  
    Sub Main()  
  
        ' The following line adds one listener to the event.  
        AddHandler ProcessInteger, Function(m As Integer) m  
  
        ' The following statement searches the current listeners   
        ' for a match to remove. However, this lambda is not the same  
        ' as the previous one, so nothing is removed.  
        RemoveHandler ProcessInteger, Function(m As Integer) m  
  
    End Sub  
End Module  
```  
  
 默认情况下，此消息是一个警告。 有关如何隐藏警告或将警告视为错误的详细信息，请参阅 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。  
  
 **错误 ID:** BC42326  
  
## <a name="to-correct-this-error"></a>更正此错误  
  
-   若要避免出现警告，并删除 lambda 表达式，lambda 表达式分配给一个变量，并在这种使用该变量`AddHandler`和`RemoveHandler`语句，如下面的示例中所示。  
  
```vb  
Module Module1  
  
    Event ProcessInteger(ByVal x As Integer)  
  
    Dim PrintHandler As ProcessIntegerEventHandler  
  
    Sub Main()  
  
        ' Assign the lambda expression to a variable.  
        PrintHandler = Function(m As Integer) m  
  
        ' Use the variable to add the listener.  
        AddHandler ProcessInteger, PrintHandler  
  
        ' Use the variable again when you want to remove the listener.  
        RemoveHandler ProcessInteger, PrintHandler  
  
    End Sub  
End Module  
```  
  
## <a name="see-also"></a>请参阅

- [Lambda 表达式](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
- [宽松委托转换](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
- [事件](../../../visual-basic/programming-guide/language-features/events/index.md)
