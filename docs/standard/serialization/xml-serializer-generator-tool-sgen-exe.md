---
title: Инструмент создания XML-сериализатора (Sgen.exe)
ms.date: 03/30/2017
ms.assetid: cc1d1f1c-fb26-4be9-885a-3fe84c81cec6
ms.openlocfilehash: 492337973f71b10dc061353b7083f596b402ae29
ms.sourcegitcommit: da2dd2772fcf32b44eb18b1cbe8affd17b1753c9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392709"
---
# <a name="xml-serializer-generator-tool-sgenexe"></a>Инструмент создания XML-сериализатора (Sgen.exe)
Генератор XML-сериализатора создает сборку сериализации XML для типов в указанной сборке, чтобы повысить производительность при запуске <xref:System.Xml.Serialization.XmlSerializer> во время сериализации или десериализации объектов указанных типов.  
  
## <a name="syntax"></a>Синтаксис  
  
```console  
sgen [options]  
```  
  
## <a name="parameters"></a>Параметры  
  
|Параметр|Описание|  
|------------|-----------------|  
|**/a @ no__t-1ssembly @ no__t-2:** _имя файла_|Создает код сериализации для всех типов, содержащихся в сборке, или исполняемого файла *имя_файла*. Можно указать только одно имя файла. Если этот аргумент повторяется, используется последнее имя файла.|  
|**/c @ no__t-1ompiler @ no__t-2:** _Options_|Задает параметры, которые следует передать компилятору C#. Поддерживаются все параметры csc.exe по мере их передачи компилятору. Этот параметр можно использовать для указания того, что сборка должна быть подписана, и для указания файла ключа.|  
|**/d @ no__t-1ebug @ no__t-2**|Создает образ, который может использоваться с отладчиком.|  
|**/f @ no__t-1orce @ no__t-2**|Принудительно перезаписывает существующую сборку с одинаковым именем. Значение по умолчанию — **false**.|  
|**/help или /?**|Отображает синтаксис команд и параметров программы.|  
|**/k @ no__t-1eep @ no__t-2**|Предотвращает удаление созданных исходных файлов и других временных файлов после их компиляции в сборку сериализации. Этот параметр можно использовать, чтобы определить, создает ли инструмент код сериализации для определенного типа.|  
|**/n @ no__t-1ologo @ no__t-2**|Отключает отображение эмблемы Майкрософт при запуске.|  
|**/o @ no__t-1ut @ no__t-2:** _путь_|Определяет каталог, в которой сохраняется созданная сборка. **Примечание.**  Имя созданной сборки представляет собой имя входной сборки и "xmlSerializers.dll".|  
|**/p @ no__t-1roxytypes @ no__t-2**|Создает код сериализации только для типов прокси XML-веб-службы.|  
|**/r @ no__t-1eference @ no__t-2:** _ассемблифилес_|Задает сборки, на которые ссылаются типы, требующие XML-сериализации. Принимает несколько файлов сборки, которые разделяются запятыми.|  
|**/s @ no__t-1ilent @ no__t-2**|Запрещает отображение сообщений об успешно выполненных операциях.|  
|**/t @ no__t-1ype @ no__t-2:** _тип_|Создает код сериализации только для указанного типа.|  
|**/v @ no__t-1erbose @ no__t-2**|Отображает подробную выходную информацию для отладки. Содержит список типов из целевой сборки, которые невозможно сериализовать с помощью <xref:System.Xml.Serialization.XmlSerializer>.|  
|**/?**|Отображает синтаксис команд и параметров программы.|  
  
## <a name="remarks"></a>Примечания  
 Если генератор XML-сериализатора не используется, <xref:System.Xml.Serialization.XmlSerializer> создает код сериализации и сборку сериализации для каждого типа при каждом запуске приложения. Чтобы повысить производительность запуска сериализации XML, используйте средство Sgen. exe, чтобы создать эти сборки заранее. Эти сборки можно развернуть вместе с приложением.  
  
 Инструмент создания XML-сериализатора также повышает производительность клиентов, использующих прокси XML-веб-служб для связи с серверами, поскольку производительность процесса сериализации не снижается при первой загрузке типа.  
  
 Эти созданные сборки нельзя использовать на стороне сервера веб-службы. Этот инструмент предназначен только для клиентов веб-служб и сценариев ручной сериализации.  
  
 Если сборка, содержащая сериализуемый тип, называется "MyType.dll", связанная сборка сериализации будет называться "MyType.XmlSerializers.dll".  
  
## <a name="examples"></a>Примеры  
 Следующая команда создает сборку с именем "Data.XmlSerializers.dll" для сериализации всех типов, содержащихся в сборке с именем "Data.dll".  
  
```console  
sgen Data.dll   
```  
  
 Код, для которого требуется сериализация и десериализация типов в Data.dll, может содержать ссылки на сборку Data.XmlSerializers.dll.  
  
## <a name="see-also"></a>См. также

- [Инструменты](../../../docs/framework/tools/index.md)
- [Командные строки](../../../docs/framework/tools/developer-command-prompt-for-vs.md)
