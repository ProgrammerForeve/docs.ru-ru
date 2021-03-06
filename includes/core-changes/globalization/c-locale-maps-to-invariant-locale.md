---
ms.openlocfilehash: 9e9e443be9ea51d214e95c676fc28f0d8790af8b
ms.sourcegitcommit: a4b10e1f2a8bb4e8ff902630855474a0c4f1b37a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2019
ms.locfileid: "71117184"
---
### <a name="c-locale-maps-to-the-invariant-locale"></a>Языковой стандарт "C" сопоставляется с инвариантным языковым стандартом

.NET Core 2.2 и более ранних версий зависит от поведения ICU по умолчанию, которое сопоставляет языковой стандарт "C" с языковым стандартом en_US_POSIX. Языковой стандарт en_US_POSIX имеет нежелательное поведение сортировки, так как не поддерживает сравнение строк без учета регистра. Так как некоторые дистрибутивы Linux устанавливают языковой стандарт "C" в качестве используемого по умолчанию, пользователи сталкивались с непредвиденным поведением. 

#### <a name="details"></a>Подробные сведения

Начиная с .NET Core 3.0 сопоставление языкового стандарта "C" изменилось на использование инвариантного (Invariant) языкового стандарта вместо en_US_POSIX. Для обеспечения согласованности в Windows также применяется сопоставление языкового стандарта "C" с инвариантным.

Сопоставление "C" с языком и региональными параметрами en_US_POSIX вызывало путаницу у клиентов, так как en_US_POSIX не поддерживает операции сортировки и поиска строк без учета регистра. Так как в некоторых дистрибутивах Linux языковой стандарт "C" используется по умолчанию, клиенты столкнулись с нежелательным поведением в этих операционных системах. 

#### <a name="version-introduced"></a>Представленная версия

.NET Core 3.0

### <a name="recommended-action"></a>Рекомендуемое действие

Ничего конкретного, кроме сведений об этом изменении. Это изменение затрагивает только приложения, использующее сопоставление языкового параметра "C".

### <a name="category"></a>Категория

Глобализация 

### <a name="affected-apis"></a>Затронутые API

Это изменение затрагивает все API параметров сортировки и языка и региональных параметров.

<!--

-->
