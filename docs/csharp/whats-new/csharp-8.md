---
title: Новые возможности C# 8.0. Руководство по языку C#
description: Обзор новых функций, доступных в C# 8.0. В этой статье представлены возможности предварительной версии 2.
ms.date: 02/12/2019
ms.openlocfilehash: 874420775215502ebdacb8420b3fe0e027d6660f
ms.sourcegitcommit: 8f95d3a37e591963ebbb9af6e90686fd5f3b8707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2019
ms.locfileid: "56747626"
---
# <a name="whats-new-in-c-80"></a>Новые возможности C# 8.0

Для языка C# представлено множество улучшений, которые вы можете опробовать уже в предварительной версии 2. Ниже приведены новые возможности, добавленные в предварительной версии 2.

- [Улучшения сопоставления шаблонов](#more-patterns-in-more-places):
  * [выражения switch](#switch-expressions);
  * [шаблоны свойств](#property-patterns);
  * [шаблоны кортежей](#tuple-patterns);
  * [позиционные шаблоны](#positional-patterns).
- [Объявления using](#using-declarations).
- [Статические локальные функции](#static-local-functions).
- [Удаляемые ссылочные структуры](#disposable-ref-structs).

Перечисленные ниже возможности языка впервые появились в предварительной версии 1 C# 8.0:

- [Ссылочные типы, допускающие значение NULL](#nullable-reference-types)
- [Асинхронные потоки](#asynchronous-streams).
- [Индексы и диапазоны](#indices-and-ranges).

> [!NOTE]
> Эта статья последний раз обновлялась для предварительной версии 2 C# 8.0.

В остальных разделах этой статьи кратко описываются эти возможности. Здесь приведены ссылки на эти подробные руководства и обзоры (если они доступны).

## <a name="more-patterns-in-more-places"></a>Дополнительные шаблоны в нескольких расположениях

Возможность **сопоставления шаблонов** позволяет работать с шаблонами в зависимости от формата в связанных, но различных типах данных. В C# 7.0 появился синтаксис для шаблонов типа и шаблонов константы, использующий выражение [`is`](../language-reference/keywords/is.md) и инструкцию [`switch`](../language-reference/keywords/switch.md). Эти функции представляют первые пробные шаги на пути к поддержке парадигм программирования, где данные и функции разделены. По мере того как отрасль переходит на использование микрослужб и других облачных архитектур, необходимы также средства других языков.

В C# 8.0 расширены эти возможности, так что вы можете использовать дополнительные выражения шаблонов в нескольких расположениях в коде. Учитывайте эти функции, когда данные и функциональные возможности представлены отдельно. Рассмотрите возможность сопоставления шаблонов, когда алгоритмы зависят от фактов, отличных от типа среды выполнения объекта. Эти методы предоставляют другой способ выражения проектов.

Помимо новых шаблонов в новых расположениях, в C# 8.0 добавлены **рекурсивные шаблоны**. Результат выражения любого шаблона — это выражение языка. Рекурсивный шаблон является просто выражением шаблона, которое применяется к результатам другого выражения шаблона.

### <a name="switch-expressions"></a>Выражения switch

Часто инструкция [`switch`](../language-reference/keywords/switch.md) возвращает значение в каждом из блоков `case`. **Выражения switch** позволяет использовать более краткий синтаксис выражения. В нем меньше повторяющихся ключевых слов `case` и `break`, а также меньше фигурных скобок.  Например, рассмотрим следующее перечисление, которое выводит список цветов радуги:

```csharp
public enum Rainbow
{
    Red,
    Orange,
    Yellow,
    Blue,
    Indigo,
    Violet
}
```

Вы можете преобразовать значения `Rainbow` в RGB-значения, используя следующий метод, содержащий выражение switch:

```csharp
public static RGBColor FromRainbow(Rainbow colorBand) =>
    colorBand switch
    {
        Rainbow.Red    => new RGBColor(0xFF, 0x00, 0x00),
        Rainbow.Orange => new RGBColor(0xFF, 0x7F, 0x00),
        Rainbow.Yellow => new RGBColor(0xFF, 0xFF, 0x00),
        Rainbow.Blue   => new RGBColor(0x00, 0x00, 0xFF),
        Rainbow.Indigo => new RGBColor(0x4B, 0x00, 0x82),
        Rainbow.Violet => new RGBColor(0x94, 0x00, 0xD3),
        _              => throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand)),
    };
```

Здесь представлено несколько улучшений синтаксиса:

- Переменная расположена перед ключевым словом `switch`. Другой порядок позволяет визуально легко отличить выражение switch от инструкции switch.
- Элементы `case` и `:` заменяются на `=>`. Это более лаконично и интуитивно понятно.
- Случай `default` заменяется пустой переменной `_`.
- Тексты являются выражениями, а не инструкциями.

Сравните это с эквивалентным кодом, где используется классическая инструкция `switch`:

```csharp
public static RGBColor fromRainbowClassic(Rainbow colorBand)
{
    switch (colorBand)
    {
        case Rainbow.Red:
            return new RGBColor(0xFF, 0x00, 0x00);
        case Rainbow.Orange:
            return new RGBColor(0xFF, 0x7F, 0x00);
        case Rainbow.Yellow:
            return new RGBColor(0xFF, 0xFF, 0x00);
        case Rainbow.Blue:
            return new RGBColor(0x00, 0x00, 0xFF);
        case Rainbow.Indigo:
            return new RGBColor(0x4B, 0x00, 0x82);
        case Rainbow.Violet:
            return new RGBColor(0x94, 0x00, 0xD3);
        default:
            throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand));
    };
}
```

### <a name="property-patterns"></a>Шаблоны свойств

**Шаблон свойств** позволяет сопоставлять свойства исследуемого объекта. Рассмотрим сайт электронной коммерции, на котором должен вычисляться налог с продаж по адресу покупателя. Это вычисление не является основной задачей класса `Address`. Оно меняется со временем и, скорее всего, чаще, чем изменения формата адреса. Сумма налога с продаж зависит от свойства `State` адреса. В следующем методе используется шаблон свойства для вычисления налога с продаж по адресу и цене:

```csharp
public static decimal ComputeSalesTax(Address location, decimal salePrice) =>
    location switch
    {
        { State: "WA" } => salePrice * 0.06M,
        { State: "MN" } => salePrice * 0.75M,
        { State: "MI" } => salePrice * 0.05M,
        // other cases removed for brevity...
        _ => 0M
    };
    ```

Pattern matching creates a concise syntax for expressing this algorithm.

### Tuple patterns

Some algorithms depend on multiple inputs. **Tuple patterns** allow you to switch based on multiple values expressed as a [tuple](../tuples.md).  The following code shows a switch expression for the game *rock, paper, scissors*:

```csharp
public static string RockPaperScissors(string first, string second)
    => (first, second) switch
    {
        ("rock", "paper") => "rock is covered by paper. Paper wins.",
        ("rock", "scissors") => "rock breaks scissors. Rock wins.",
        ("paper", "rock") => "paper covers rock. Paper wins.",
        ("paper", "scissors") => "paper is cut by scissors. Scissors wins.",
        ("scissors", "rock") => "scissors is broken by rock. Rock wins.",
        ("scissors", "paper") => "scissors cuts paper. Scissors wins.",
        (_, _) => "tie"
    };
    ```

The messages indicate the winner. The discard case represents the three combinations for ties, or other text inputs.

### Positional patterns

Some types include a `Deconstruct` method that deconstructs its properties into discrete variables. When a `Deconstruct` method is accessible, you can use **positional patterns** to inspect properties of the object and use those properties for a pattern.  Consider the following `Point` class that includes a `Deconstruct` method to create discrete variables for `X` and `Y`:

```csharp
public class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y) => (X, Y) = (x, y);

    public void Deconstruct(out int x, out int y) =>
        (x, y) = (X, Y);
}
```

В следующем методе используется **позиционный шаблон** для извлечения значений `x` и `y`. Затем используется предложение `when` для определения квадранта точки:

```csharp
static string Quadrant(Point p) => p switch
{
    (0, 0) => "origin",
    (var x, var y) when x > 0 && y > 0 => "Quadrant 1",
    (var x, var y) when x < 0 && y > 0 => "Quadrant 2",
    (var x, var y) when x < 0 && y < 0 => "Quadrant 3",
    (var x, var y) when x > 0 && y < 0 => "Quadrant 4",
    (var x, var y) => "on a border",
    _ => "unknown"
};
```

Шаблон пустой переменной в предыдущем операторе switch совпадает с выражением, если значение `x` или `y`, но не оба, равно 0. Выражение switch должно создавать значение или исключение. Если ни один из вариантов не совпадает, выражение switch создает исключение. Компилятор создает предупреждение, если в выражении switch не охватываются все возможные случаи.

## <a name="using-declarations"></a>Объявления using

**Объявление using** — это объявление переменной, которому предшествует ключевое слово `using`. Оно сообщает компилятору, что объявляемая переменная должна быть удалена в конце области видимости. Для примера рассмотрим следующий код, в котором записывается текстовый файл:

```csharp
static void WriteLinesToFile(IEnumerable<string> lines)
{
    using var file = new System.IO.StreamWriter("WriteLines2.txt");
    foreach (string line in lines)
    {
        // If the line doesn't contain the word 'Second', write the line to the file.
        if (!line.Contains("Second"))
        {
            file.WriteLine(line);
        }
    }
// file is disposed here
}
```

В приведенном выше примере файл удаляется при достижении закрывающей фигурной скобки метода. Это конец области, в котором объявляется `file`. Приведенный выше код эквивалентен следующему коду, в котором используется классическая инструкция [using](../language-reference/keywords/using-statement.md):


```csharp
static void WriteLinesToFile(IEnumerable<string> lines)
{
    using (var file = new System.IO.StreamWriter("WriteLines2.txt"))
    {
        foreach (string line in lines)
        {
            // If the line doesn't contain the word 'Second', write the line to the file.
            if (!line.Contains("Second"))
            {
                file.WriteLine(line);
            }
        }
    } // file is disposed here
}
```

В приведенном выше примере файл удаляется при достижении закрывающей фигурной скобки, связанной с инструкцией `using`.

В обоих случаях компилятор создает вызов метода `Dispose()`. Компилятор создает ошибку, если выражение в инструкции using не может быть удалено.

## <a name="static-local-functions"></a>Статические локальные функции

Теперь вы можете добавить модификатор `static` для локальных функций, чтобы убедиться, что локальная функция не захватывает (не ссылается на) какие-либо переменные из области видимости. Это приводит к возникновению ошибки `CS8421`: "A static local function can't contain a reference to <variable>" (Статическая локальная функция не может содержать ссылку на <variable>). 

Рассмотрим следующий код. Локальная функция `LocalFunction` обращается к переменной `y`, объявленной в области видимости (метод `M`). Таким образом `LocalFunction` не может объявляться с помощью модификатора `static`:

```csharp
int M()
{
    int y;
    LocalFunction();
    return y;

    void LocalFunction() => y = 0;
}
```

Следующий код содержит статическую локальную функцию. Она может быть статической, так как она не обращается ко всем переменным в области видимости:

```csharp
int M()
{
    int y = 5;
    int x = 7;
    return Add(x, y);

    static int Add(int left, int right) => left + right;
}
```

## <a name="disposable-ref-structs"></a>Удаляемые ссылочные структуры

Объект `struct`, объявленный с помощью модификатора `ref`, не может реализовывать интерфейсы и поэтому не может реализовать <xref:System.IDisposable>. Таким образом, чтобы объект `ref struct` можно было удалить, у него должен быть доступный метод `void Dispose()`. Это также относится к объявлениям `readonly ref struct`.

## <a name="nullable-reference-types"></a>Ссылочные типы, допускающие значение null

Внутри контекста заметок, допускающих значение null, любая переменная ссылочного типа считается переменной **ссылочного типа, не допускающего значения null**. Если вы хотите указать, что переменная может принимать значение null, необходимо добавить к имени типа `?`, чтобы объявить переменную как переменную **ссылочного типа, допускающего значения null**.

Для ссылочных типов, не допускающих значение null, компилятор использует анализ потока, чтобы убедиться, что локальные переменные инициализируются в значение, отличное от null, при объявлении. Поля должны быть инициализированы во время построения. Компилятор создает предупреждение, если переменная не задана с помощью вызова любого из доступных конструкторов или с помощью инициализатора. Кроме того, ссылочным типам, не допускающим значение null, нельзя задать значение, которое может быть null.

Ссылочные типы, допускающие значение null, не проверяются на предмет того, назначено ли им значение null или получили ли они его после инициализации. Тем не менее, компилятор использует анализ потока, чтобы убедиться, что любая переменная ссылочного типа, допускающего значение null, проверяется на наличие значения null, прежде чем обратиться к ней или назначить ей ссылочный тип, не допускающий значение null.

Дополнительные сведения о функции см. в обзоре [ссылочных типов, допускающих значение null](../nullable-references.md). Попробуйте самостоятельно применить эту возможность в новом приложении в этом [руководстве по ссылочным типам, допускающим значение null](../tutorials/nullable-reference-types.md). Дополнительные сведения о шагах переноса существующей базы кода для использования ссылочных типов, допускающих значение null, см. в [этом руководстве](../tutorials/upgrade-to-nullable-references.md).

## <a name="asynchronous-streams"></a>Асинхронные потоки

Начиная с C# версии 8.0 можно создавать и использовать потоки асинхронно. В методе, который возвращает асинхронный поток, есть три свойства:

1. Он был объявлен с помощью модификатора `async`.
1. Он возвращает интерфейс <xref:System.Collections.Generic.IAsyncEnumerable%601>.
1. Метод содержит инструкции `yield return` для возвращения последовательных элементов в асинхронном потоке.

Для использования асинхронного потока требуется добавить ключевое слово `await` перед ключевым словом `foreach` при перечислении элементов потока. Для добавления ключевого слова `await` требуется, чтобы метод, который перечисляет асинхронный поток, был объявлен с помощью модификатора `async` и возвращал тип, допустимый для метода `async`. Обычно это означает возвращение структуры <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601>. Это также может быть структура <xref:System.Threading.Tasks.ValueTask> или <xref:System.Threading.Tasks.ValueTask%601>. Метод может использовать и создавать асинхронный поток. Это означает, что будет возвращен интерфейс <xref:System.Collections.Generic.IAsyncEnumerable%601>. Следующий код создает последовательность чисел от 1 до 20, ожидая 100 мс между формированием каждого числа:

```csharp
public static async System.Collections.Generic.IAsyncEnumerable<int> GenerateSequence()
{
    for (int i = 0; i < 20; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}
```

Вы бы перечислили последовательность с использованием инструкции `await foreach`:

```csharp
await foreach (var number in GenerateSequence())
{
    Console.WriteLine(number);
}
```

Вы можете попробовать асинхронные потоки самостоятельно в нашем руководстве по [созданию и использованию асинхронных потоков](../tutorials/generate-consume-asynchronous-stream.md).

## <a name="indices-and-ranges"></a>Индексы и диапазоны

Диапазоны и индексы обеспечивают лаконичный синтаксис для указания поддиапазонов массива: <xref:System.Span%601> или <xref:System.ReadOnlySpan%601>.

Вы можете указать индекс **с конца**. Указать индекс **с конца** можно с помощью оператора `^`. Вы знакомы с выражением `array[2]`, то есть элемент "2 с самого начала". Теперь выражение `array[^2]` означает элемент "2 с конца". Индекс `^0` означает "конец" или индекс, который следует за последним элементом.

Вы можете указать **диапазон** с помощью **оператора диапазона**: `..`. Например, оператор `0..^0` указывает весь диапазон массива: 0 от начала до, но не включая 0 с конца. Любой операнд может использовать элемент "от начала" или "с конца". Кроме того, можно опустить один из операндов. По умолчанию оператор `0` используется для начального индекса, а `^0` — для конечного индекса.

Рассмотрим несколько примеров. Обратите внимание на следующий массив, который помечен индексом от начала и от конца:

```csharp
var words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};
```

Индекс каждого элемента вводит понятие "от начала" и "от конца", и диапазоны указаны без учета конца диапазона. "Начало" всего массива является первым элементом. "Конец" всего массива следует *после* последнего элемента.

Вы можете получить последнее слово с помощью индекса `^1`:

```csharp
Console.WriteLine($"The last word is {words[^1]}");
// writes "dog"
```

Следующий код создает поддиапазон со словами "quick", "brown" и "fox". Он включает в себя элементы от `words[1]` до `words[3]`. Элемент `words[4]` находится вне диапазона.

```csharp
var brownFox = words[1..4];
```

Следующий код создает поддиапазон со словами "lazy" и "dog". Он включает элементы `words[^2]` и `words[^1]`. Конечный индекс `words[^0]` не включен:

```csharp
var lazyDog = words[^2..^0];
```

В следующих примерах создаются диапазоны, которые должны быть открыты для начала, конца или в обоих случаях:

```csharp
var allWords = words[..]; // contains "The" through "dog".
var firstPhrase = words[..4]; // contains "The" through "fox"
var lastPhrase = words[6..]; // contains "the, "lazy" and "dog"
```

Вы также можете объявить диапазоны как переменные:

```csharp
Range phrase = 1..4;
```

Затем вы можете использовать диапазон внутри символов `[` и `]`:

```csharp
var text = words[phrase];
```