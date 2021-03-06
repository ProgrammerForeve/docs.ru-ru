---
title: Объекты DataView
ms.date: 03/30/2017
ms.assetid: 0fe5dfa2-c1cd-435f-90b6-b4dd2e3ef34b
ms.openlocfilehash: 8a06accb11631f2dce6b0d39587d7274223c0e68
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70786346"
---
# <a name="dataviews"></a>Объекты DataView
<xref:System.Data.DataView> позволяет создавать различные представления данных, которые хранятся в <xref:System.Data.DataTable>. Эта возможность часто используется в приложениях связывания данных. С помощью **DataView**можно предоставлять данные в таблице с различными порядками сортировки, а данные можно фильтровать по состоянию строки или по критерию фильтра.  
  
 Объект **DataView** обеспечивает динамическое представление данных в базовом **объекте DataTable**: содержимое, упорядочение и членство отображают изменения по мере их появления. Это поведение отличается от метода **SELECT** объекта **DataTable**, <xref:System.Data.DataRow> который возвращает массив из таблицы на основе определенного фильтра и (или) порядка сортировки: это содержимое отражает изменения в базовой таблице, но его членство и Упорядочивание остается статическим. Динамические возможности **DataView** делают его идеальным для приложений привязки данных.  
  
 Объект **DataView** предоставляет динамическое представление одного набора данных, похожее на представление базы данных, к которому можно применить различные критерии сортировки и фильтрации. Однако в отличие от представления базы данных, **DataView** не может рассматриваться как таблица и не может предоставлять представление соединяемых таблиц. Кроме того, нельзя исключать столбцы, существующие в исходной таблице, добавлять столбцы (например, вычисляемые), которых нет в исходной таблице.  
  
 Можно использовать <xref:System.Data.DataView.DataViewManager%2A> для управления параметрами представления для всех таблиц в **наборе данных**. **DataViewManager** предоставляет удобный способ управления параметрами представления по умолчанию для каждой таблицы. При привязке элемента управления к более чем одной таблице **набора данных**привязка к **DataViewManager** является идеальным выбором.  
  
## <a name="in-this-section"></a>В этом разделе  
 [Создание DataView](creating-a-dataview.md)  
 Описывает создание **объекта DataView** для **DataTable**.  
  
 [Сортировка и фильтрация данных](sorting-and-filtering-data.md)  
 Описывает, как задать свойства **DataView** для возвращения подмножеств строк данных, отвечающих определенным условиям фильтра, или для возврата данных в определенном порядке сортировки.  
  
 [Объекты DataRow и DataRowView](datarows-and-datarowviews.md)  
 Описывает, как получить доступ к данным, предоставляемым **DataView**.  
  
 [Поиск строк](finding-rows.md)  
 Описывает, как найти определенную строку в **объекте DataView**.  
  
 [ChildView и отношения](childviews-and-relations.md)  
 Описывает создание представлений данных из связи типа «родители-потомки» с помощью **объекта DataView**.  
  
 [Изменение объектов DataView](modifying-dataviews.md)  
 Описывает, как изменить данные в базовой **таблице** данных с помощью **DataView**, включая включение или отключение обновлений.  
  
 [Обработка событий DataView](handling-dataview-events.md)  
 Описывает, как использовать событие **ListChanged** для получения уведомлений при обновлении содержимого или порядка **представления DataView** .  
  
 [Управление объектами DataView](managing-dataviews.md)  
 Описывает использование **DataViewManager** для управления параметрами **DataView** для каждой таблицы в **наборе данных**.  
  
## <a name="related-sections"></a>Связанные разделы  
 [Веб-приложения ASP.NET](https://docs.microsoft.com/previous-versions/655cec97(v=vs.100))  
 Описывает обзоры и подробные пошаговые инструкции по созданию приложений ASP.NET, Web Forms и веб-служб.  
  
 [Приложения Windows](https://docs.microsoft.com/previous-versions/ms184421(v=vs.100))  
 Содержит подробные сведения о работе с консольными приложениями и Windows Forms.  
  
 [Наборы данных, таблицы данных и объекты DataView](index.md)  
 Описывает объект **DataSet** и способы его использования для управления данными приложения.  
  
 [DataTables](datatables.md)  
 Описывает объект **DataTable** и способ его использования для управления данными приложения самостоятельно или в составе **набора данных**.  
  
 [ADO.NET](../index.md)  
 Описывает архитектуру и компоненты ADO.NET, а также использование ADO.NET для доступа к существующим источникам данных и управления данными приложений.  
  
## <a name="see-also"></a>См. также

- [Общие сведения об ADO.NET](../ado-net-overview.md)
