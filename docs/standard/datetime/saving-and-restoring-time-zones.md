---
title: Сохранение и восстановление часовых поясов
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- restoring time zones
- deserialization [.NET Framework], time zones
- serialization [.NET Framework], time zones
- time zone objects [.NET Framework], restoring
- saving time zones
- time zone objects [.NET Framework], deserializing
- time zones [.NET Framework], saving
- time zones [.NET Framework], restoring
- time zone objects [.NET Framework], serializing
- time zone objects [.NET Framework], saving
ms.assetid: 4028b310-e7ce-49d4-a646-1e83bfaf6f9d
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: ea44b29eaa0273baadbf02093108bc4da912a3e5
ms.sourcegitcommit: 6f28b709592503d27077b16fff2e2eacca569992
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2019
ms.locfileid: "70106746"
---
# <a name="saving-and-restoring-time-zones"></a>Сохранение и восстановление часовых поясов

<xref:System.TimeZoneInfo> Класс использует реестр для получения предопределенных данных часового пояса. Однако реестр является динамической структурой. Кроме того, сведения о часовом поясе, которые содержит реестр, используются операционной системой в основном для управления настройкой времени и преобразованиями в текущем году. Это имеет два существенных последствия для приложений, которые зависят от точных данных часового пояса:

- Часовой пояс, необходимый для приложения, может не быть определен в реестре, или он мог быть переименован или удален из реестра.

- Часовой пояс, определенный в реестре, может не иметь сведений о конкретных правилах коррекции, необходимых для преобразования исторических часовых поясов.

<xref:System.TimeZoneInfo> Класс обращается к этим ограничениям с помощью поддержки сериализации (сохранения) и десериализации (восстановления) данных часового пояса.

## <a name="time-zone-serialization-and-deserialization"></a>Сериализация и десериализация часовых поясов

Сохранение и восстановление часового пояса путем сериализации и десериализации данных часового пояса состоит только из двух вызовов методов:

- Можно сериализовать <xref:System.TimeZoneInfo> объект, вызвав <xref:System.TimeZoneInfo.ToSerializedString%2A> метод этого объекта. Метод не принимает параметров и возвращает строку, содержащую сведения о часовом поясе.

- Можно десериализовать <xref:System.TimeZoneInfo> объект из сериализованной строки, передав эту строку `static` методу (`Shared` в Visual Basic) <xref:System.TimeZoneInfo.FromSerializedString%2A?displayProperty=nameWithType> .

## <a name="serialization-and-deserialization-scenarios"></a>Сценарии сериализации и десериализации

Возможность сохранения (или сериализации) <xref:System.TimeZoneInfo> объекта в строку и восстановления (или десериализации) для последующего использования увеличивает и программу, и гибкость <xref:System.TimeZoneInfo> класса. В этом разделе рассматриваются некоторые ситуации, в которых сериализация и десериализация наиболее полезны.

### <a name="serializing-and-deserializing-time-zone-data-in-an-application"></a>Сериализация и десериализация данных часового пояса в приложении

Сериализованный часовой пояс можно восстановить из строки при необходимости. Приложение может сделать это, если часовой пояс, полученный из реестра, не может правильно преобразовать дату и время в определенном диапазоне дат. Например, данные часового пояса в реестре Windows XP поддерживают одно правило коррекции, а Часовые пояса, определенные в реестре Windows Vista, обычно предоставляют сведения о двух правилах коррекции. Это означает, что преобразования исторического времени могут быть неточными. Это ограничение может быть обработано сериализацией и десериализацией данных часового пояса.

В следующем примере пользовательский <xref:System.TimeZoneInfo> класс, не имеющий правил коррекции, определен для представления Восточного стандартного часового пояса с 1883 до 1917, до введения летнего времени в США. Пользовательский часовой пояс сериализуется в переменную с глобальной областью действия. Методу `ConvertUtcTime`преобразования часового пояса передается время в формате UTC для преобразования. Если дата и время происходят в 1917 или более ранней версии, настраиваемый Стандартный часовой пояс восстанавливается из сериализованной строки и заменяет часовой пояс, полученный из реестра.

[!code-csharp[System.TimeZone2.Serialization.1#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Serialization.1/cs/Serialization.cs#1)]
[!code-vb[System.TimeZone2.Serialization.1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Serialization.1/vb/Serialization.vb#1)]

### <a name="handling-time-zone-exceptions"></a>Обработка исключений часовых поясов

Поскольку реестр является динамической структурой, его содержимое подвержено случайному или намеренному изменению. Это означает, что часовой пояс, который должен быть определен в реестре и который необходим для успешного выполнения приложения, может отсутствовать. Без поддержки сериализации и десериализации часовых поясов у вас есть немало выбора, но для решения этой <xref:System.TimeZoneNotFoundException> программы необходимо завершить приложение. Однако с помощью сериализации и десериализации часовых поясов можно выполнить непредвиденную <xref:System.TimeZoneNotFoundException> обработку, восстановив требуемый часовой пояс из сериализованной строки, и приложение продолжит работу.

В следующем примере создается и сериализуется настраиваемый Центральный часовой пояс. Затем он пытается получить центральный стандартный часовой пояс из реестра. Если операция извлечения создает исключение <xref:System.TimeZoneNotFoundException> <xref:System.InvalidTimeZoneException>или, обработчик исключений десериализует часовой пояс.

[!code-csharp[System.TimeZone2.Serialization.2#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Serialization.2/cs/Serialization2.cs#1)]
[!code-vb[System.TimeZone2.Serialization.2#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Serialization.2/vb/Serialization2.vb#1)]

### <a name="storing-a-serialized-string-and-restoring-it-when-needed"></a>Сохранение сериализованной строки и ее восстановление при необходимости

В предыдущих примерах хранились сведения о часовом поясе в строковую переменную и восстановлена при необходимости. Однако строка, которая содержит сериализованные сведения о часовом поясе, сама по себе может храниться на некоторых носителях, например во внешнем файле, файле ресурсов, внедренном в приложение, или в реестре. (Обратите внимание, что сведения о пользовательских часовых поясах следует хранить отдельно от разделов системного часового пояса в реестре.)

Хранение сериализованной строки часового пояса таким образом также отделяет подпрограммы создания часового пояса от самого приложения. Например, можно выполнить процедуру создания часового пояса и создать файл данных, содержащий исторические сведения о часовом поясе, которые может использовать приложение. Файл данных можно затем установить вместе с приложением. его можно открыть, а также при необходимости десериализовать один или несколько часовых поясов, если они требуются приложению.

Пример использования внедренного ресурса для хранения сериализованных данных часового пояса см. в разделе [как Сохраняйте Часовые пояса во внедренном](../../../docs/standard/datetime/save-time-zones-to-an-embedded-resource.md) ресурсе и [последующее: Восстановление часовых поясов из внедренного](../../../docs/standard/datetime/restore-time-zones-from-an-embedded-resource.md)ресурса.

## <a name="see-also"></a>См. также

- [Даты, время и часовые пояса](../../../docs/standard/datetime/index.md)
