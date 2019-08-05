---
title: Применение упрощенных шаблонов CQRS и DDD в микрослужбах
description: Архитектура микрослужб .NET для контейнерных приложений .NET | Понимание отношения между шаблонами CQRS и DDD.
ms.date: 10/08/2018
ms.openlocfilehash: 36bffce37176aed6c7d9daea7f2995952b58e895
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68674381"
---
# <a name="apply-simplified-cqrs-and-ddd-patterns-in-a-microservice"></a>Применение в микрослужбе упрощенных шаблонов CQRS и DDD

CQRS — это шаблон архитектуры, который разделяет модели для чтения и записи данных. Соответствующее понятие [Разделение команд и запросов (CQS)](https://martinfowler.com/bliki/CommandQuerySeparation.html) было дано Бертраном Мейером в его книге *Объектно-ориентированное конструирование программных систем*. Базовая идея состоит в том, что вы можете разделить операции системы на две четкие категории:

- Запросы. Они возвращают результат, не изменяют состояние системы и не обладают побочными эффектами.

- Команды. Они изменяют состояние системы.

CQS — простое понятие. Оно означает, что методы в одном и том же объекте должны быть либо запросами, либо командами. Каждый метод возвращает состояние или изменяет состояние, но не делает и то, и другое одновременно. Объект шаблона даже одного репозитория может соответствовать CQS. CQS может считаться фундаментальным принципом CQRS.

Принцип [разделения ответственности команд и запросов (CQRS)](https://martinfowler.com/bliki/CQRS.html) был введен Грегом Янгом и активно распространяется Уди Даханом и другими. Он основан на принципе CQS, но более детален. Его можно рассматривать как шаблон, основанный на командах и событиях с возможностью использовать асинхронные сообщения. Во многих случаях CQRS относится к более сложным сценариям, например, использование отдельных баз данных для операций чтения (запросов) и для операций записи (обновлений). Кроме того, в более сложной системе CQRS можно реализовать [Использование событий в качестве источников (ES)](https://martinfowler.com/eaaDev/EventSourcing.html) и хранить в модели предметной области только события вместо данных о текущем состоянии. Однако в данном руководстве этот подход не применяется. Мы используем простой подход CQRS, который включает только отделение запросов от команд.

Это отделение в CQRS достигается за счет группирования операций запроса на одном уровне и команд на другом уровне. На каждом уровне есть собственная модель данных (обратите внимание, что понятие "модель" не всегда означает другую базу данных), и она создается с использованием собственного сочетания шаблонов и технологий. Более того, эти два уровня могут находиться в пределах одного уровня или одной микрослужбы, как показано в примере, используемом в этом руководстве (микрослужба сортировки). Или они могут быть реализованы в различных микрослужбах или процессах, чтобы их можно было оптимизировать и масштабировать отдельно, так чтобы они не влияли друг на друга.

В CQRS для операций чтения и записи используются два объекта, а в других контекстах — один. База данных с денормализованными операциями чтения может быть удобной в некоторых ситуациях. О них можно узнать из более сложной литературы по CQRS. Но мы не будем использовать этот подход, так как наша цель — получить большую гибкость в запросах, а не ограничивать их из-за ограничений, присущих шаблонам DDD, таким как агрегаты.

Примером такого рода службы является микрослужба сортировки из примера приложения eShopOnContainers. Эта служба реализует микрослужбу на основе упрощенного подхода CQRS. В нем используется один источник данных или база данных, но две логических модели, а также шаблоны DDD для транзакционного домена, как показано на рис. 7-2.

![Логическая микрослужба заказа включает свою базу данных заказа, которая может находиться в том же узле Docker. Наличие базы данных в том же узле Docker хорошо подходит для разработки, но не подходит для рабочей среды.](./media/image2.png)

**Рис. 7-2**. Упрощенная микрослужба на основе CQRS и DDD

В качестве уровня приложения можно использовать само веб-API. Важный аспект создания такой архитектуры состоит в том, что в соответствии с шаблоном CQRS микрослужба отделила запросы и модели представлений (модели данных, созданные специально для клиентских приложений) от команд, модели предметной области и транзакций. Этот подход позволяет сохранить независимость запросов от ограничений шаблонов DDD, которые имеют смысл только для транзакций и обновлений, как описано в следующих разделах.

## <a name="additional-resources"></a>Дополнительные ресурсы

- **Грег Янг (Greg Young). Управление версиями в системе источника событий** (бесплатная электронная книга) \
   <https://leanpub.com/esversioning/read>

>[!div class="step-by-step"]
>[Назад](index.md)
>[Вперед](eshoponcontainers-cqrs-ddd-microservice.md)