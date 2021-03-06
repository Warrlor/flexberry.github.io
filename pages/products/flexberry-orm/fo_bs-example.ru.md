---
title: Пример использования бизнес-сервера
sidebar: flexberry-orm_sidebar
keywords: Flexberry Designer, Flexberry ORM, Public, Бизнес-серверы
toc: true
permalink: ru/fo_bs-example.html
---

## Бизнес-сервер

Что такое бизнес-сервер и зачем он нужен можно прочитать **[здесь](fo_business-servers-wrapper-business-facade.html)**

## Пример

В качестве примера мы рассмотрим введение ограничений на создание новых объектов

#### Диаграмма

![](/images/pages/products/flexberry-orm/FilterExDiagram.PNG)

#### Задача

При попытке создания нового кредита необходимо проверить, существуют ли непогашенные кредиты для этого Клиента.

#### Добавление бизнес-сервера

Прежде всего необходимо [создать класс со стереотипом `businessserver` на диаграмме классов](fd_business-servers.html). Назовем его `КредитБС`.
''__Примечание__: чтобы класс не занимал много места, его можно свернуть, выбрав контекстное меню `Свернуть`''

![](/images/pages/products/flexberry-orm/BSExample.PNG)

Сохраним диаграмму, чтобы бизнес-сервер попал в список доступных классов.

Зайдем в свойства класса `Кредит` и в поле `BSClass` выберем из выпадающего списка созданный бизнес-сервер

![](/images/pages/products/flexberry-orm/BSExample1.PNG)

Сохраним и закроем окно.

Сохраним и закроем диаграмму классов.

Теперь в меню стадии `CSharp` -> `Генерировать` появилась возможность генерации бизнес-серверов и бизнес-фасадов.

![](/images/pages/products/flexberry-orm/BSExample2.PNG)

Сгенерируем их.

''__Примечание__: объекты тоже нужно перегенерировать, чтобы внеслись изменения касательно ссылок объектов и бизнес-серверов''

Добавим в проект Visual Studio сгенерированные проекты бизнес-серверов и фасадов.

Обратим внимание, что в классе `Кредит` появилась ссылка в виде атрибута класса на новый бизнес-сервер:

```cs 
[BusinessServer("IIS.Кредиты.КредитБС, Кредиты(BusinessServers)", ICSSoft.STORMNET.Business.DataServiceObjectEvents.OnAllEvents)] 
```

Откроем файл бизнес-сервера `КредитБС.cs`

Обратим внимание на [метод `OnUpdateКредит`, принимающий в качестве параметра объект типа `Кредит` и возвращающий объект типа `DataObject[]`](fo_bs-example.html)

```cs
        public virtual ICSSoft.STORMNET.DataObject[] OnUpdateКредит(IIS.Кредиты.Кредит UpdatedObject)
        {
            // *** Start programmer edit section *** (OnUpdateКредит)

            return new ICSSoft.STORMNET.DataObject[0];
            // *** End programmer edit section *** (OnUpdateКредит)
        }```

Как можно заметить, в этом методе ничего не происходит. Добавим в него логику по проверке вводимого значения суммы кредита: она должна быть неотрицательной.

```cs
        public virtual ICSSoft.STORMNET.DataObject[] OnUpdateКредит(IIS.Кредиты.Кредит UpdatedObject)
        {
            // *** Start programmer edit section *** (OnUpdateКредит)
            if (UpdatedObject.СуммаКредита < 0)
                throw new Exception("Сумма кредита не может быть отрицательной");

            return new ICSSoft.STORMNET.DataObject[0];
            // *** End programmer edit section *** (OnUpdateКредит)
        }
```

Запустим проект и попробуем ввести отрицательное число в поле `Сумма кредита`, получим

![](/images/pages/products/flexberry-orm/BSExample3.PNG)

Аналогично можно поставить проверки на `СрокКредита` и на `ДатуВыдачи`.

Однако, вернемся к нашей задаче. Нам необходимо проверить, нет ли у данного клиента незакрытых кредитов.

* Учтем, что данная проверка имеет место только при создании `Кредита`
* Найдем все кредиты `Клиента`, для которого создается `Кредит`
>__Примечание__: подробнее о вычитке данных из SQL базы данных описано в [другой статье](fo_sql-query.html)
* Проверим сколько из них не закрыты
* Выдадим сообщение об ошибке если обнаружим незакрытый Кредит

```cs
// Учтем, что данная проверка имеет место только при создании кредита
if (UpdatedObject.GetStatus() == ObjectStatus.Created)
{
     // Найдем все кредиты Клиента, для которого создается Кредит
     var кредиты = LoadAllByКлиент(UpdatedObject.Клиент);
     foreach (Кредит кредит in кредиты)
     {
         // Проверим, сколько из них не закрыты
         decimal sum = кредит.СтрокаПланаПогашения.Cast<СтрокаПланаПогашения>().Sum(stroke => stroke.Сумма);
         if (sum <= кредит.СуммаКредита)
         {
             // Выдадим сообщение об ошибке если обнаружим незакрытый Кредит
             throw new Exception("У данного клиента есть незакрытые кредиты.");
         }
     }
}
```

**Примечание**: Реализацию метода `LoadAllByClient` можно посмотреть в [этой статье](fo_func-eq.html).

**Примечание**: Обратите внимание на первую проверку **[UpdatedObject.GetStatus() == ObjectStatus.Created](fo_object-status-and-loading-state.html)**, она позволяет нам отсечь случаи обновления или удаления объекта. 

### Итоги

Теперь при попытке создания нового `Кредита` будет происходить проверка, описанная выше.

Бизнес-сервер можно создать для каждого класса и вводить необходимую логику, которая будет выполняться в момент сохранения изменений в объект, будь то создание, удаление или изменение состояния объекта.

