---
title: JavaScript jQuery API для WebObjectListView
sidebar: flexberry-aspnet_sidebar
keywords: Flexberry ASP-NET, JavaScript API
toc: true
permalink: en/fa_js-api-wolv.html
lang: en
---

Для манипуляций с [WOLV](fa_web-object-list-view.html) на стороне клиента следует использовать `WOLV JS API`, который представляет собой jQuery плагин (`icsWolv`).

## Методы

| Наименование | Параметры | Описание |
| ------------ | ------------------- | -------- |
| **Общее** |||
| chooseDataObject | PrimaryKey объекта | Генерация клика по строке с объектом, переданным в этот метод. | 
| refresh | Нет | Обновление содержимого WOLV. | 
| scroll | Нет | Прокрутка экрана к WOLV. | 
| scrollToObject | PrimaryKey объекта | Прокрутка списка к выбранному объекту. | 
| **Сортировка** |||
| resetSort | Нет | Метод для сброса пользовательской сортировки WOLV. | 
| sort | `append` - добавлять сортировку или установить заново <br> `colNum` - номер столбца | Изменить сортировку по столбцу WOLV. Последовательность смены параметров: `None -> ASC -> DESC -> None`. | 
| **Флажки WOLV** |||
| getSelectedItems | Нет | Получить идентификаторы всех выбранных элементов на всех страницах. ''__Если выбраны все элементы на всех страницах (при помощи соответствующей кнопки, а не ручным выбором), то придет '''пустой массив'''__''. | 
| getSelectedAllObjectsValue | Нет | Позволяет узнать, выделены ли все объекты на всех страницах. Возвращает `boolean`. | 
| selectAllObjects | Нет | Отметить все объекты на всех страницах. | 
| unselectAllObjects | Нет | Снять выделение со всех объектов на всех страницах. | 
| selectObjectsOnCurrentPage | Нет | Отметить все объекты на текущей странице. | 
| unselectObjectsOnCurrentPage | Нет | Снять выделение со всех объектов на текущей странице. | 

Пример использования:

```javascript
// Обновить содержимое WOLV.
$('#wolvid').icsWolv('refresh');
```

Генерация клика по строке:

```javascript
$('#LookUpFormWOLV').icsWolv('chooseDataObject', '{4c6bf554-41d5-4cc6-b526-21bed9867a5a}');
```

## События

| Наименование | Параметры | Описание |
| ------------ | --------- | -------- |
| `deleting.wolv` | `selectAll` - флаг "выделенности" всех объектов на всех страницах; <br> `selectedItems` - список первичных ключей выделенных объектов данных; | Удаление элемента(-ов) списка. |
| `editformclose.wolv` | `objectKey` - первичный ключ объекта, который редактировался на этой форме; | Закрытие формы редактирования. |
| `refreshing.wolv` | - | Обновление списка. |
| `rowclick.wolv` | `pk` - первичный ключ объекта, соответствующего строке, по которой кликнул пользователь; | Клик по строке списка. |
| `showaddingpage.wolv` | `addingUrl` - URL-адрес формы создания объекта; | Отображение страницы создания нового объекта данных. |
| `showeditingpage.wolv` | `pk` - первичный ключ редактируемого объекта; <br> `editingUrl` - URL-адрес страницы редактирования; | Отображение страницы редактирования объекта данных. |
| `showprototypingpage.wolv`  | `pk` - первичный ключ прототипируемого объекта; <br> `prototypingUrl` - URL-адрес страницы прототипирования; | Отображение формы прототипирования. |
| `showviewingpage.wolv` | `pk` - первичный ключ просматриваемого объекта; <br> `viewingUrl` - URL-адрес страницы просмотра; | Отображение страницы просмотра объекта данных. |

Пример использования:

```javascript
$('.ics-wolv').on('showeditingpage.wolv', function() {
    alert('Была отображена форма редактирования объекта.');
});
```

#### Клик по строке (rowclick.wolv)

При использовании события `rowclick.wolv` можно получить `PrimaryKey` объекта, по строке которого был произведен `click`:

```javascript
$('.ics-wolv').on('rowclick.wolv', function(e, d) { 
     e.preventDefault();
     alert('primaryKey: ' + d.data.pk);
});
```

#### Клиентская обработка обновления списка

```javascript
// Подписка на событие обновления WOLV.
// Событие сработает при нажатии пользователем кнопки "Обновить" на панели WOLV.
// Событие также сработает при вызове JS API метода $('#wolvid').icsWolv('refresh');
$('.ics-wolv').on('refreshing.wolv', function(e) { 
    // Отменим обновление WOLV.
    e.preventDefault(); 
    
    // Здесь можно описать свою логику обновления.
    alert('Список не был обновлен');
});
```

#### Клиентская обработка удаления

Имеется возможность подписаться на события перед удалением элементов и, если нужно, отменить его.

Обработчику передаются два аргумента: объект события и данные. У объекта данных в поле `data` содержится флаг `selectAll`, если он равен `false`, то также
передается массив первичных ключей выделенных объектов `selectedItems`.

Пример использования:

```javascript
$('#wolvid').on('deleting.wolv', function(e, eventData) { 
    if(eventData.data.selectAll)
        alert('Выделены все объекты на всех страницах');
    else
        alert(eventData.data.selectedItems); // Вывод выделенных ПК.
    
    alert('Удаление отменено');
    return false; 
});
```

#### Клиентская обработка закрытия всплывающего окна формы редактирования

По умолчанию, при закрытии формы редактирования, открытой во всплывающем окне, происходит обновление списковой формы, с которой была открыта форма
редактирования. Имеется возможность отменить это действие, аналогично удалению. Нужно подписать `WOLV`, с которого происходит открытие формы редактирования
(или `WOLV`, id которого задан в параметре `WolvId` к ней), на событие `editformclose.wolv`. В переданном обработчику объекте события будет содержаться поле
`objectKey`, хранящее первичный ключ объекта, редактировавшегося на закрытой форме. Отменить действие по умолчанию можно с помощью `return false;`
или `e.preventDefault();`.

Пример:

```javascript
$('#wolvid').on('editformclose.wolv', function (e) {
    alert('Закрывается форма редактирования объекта с ключом ' + e.objectKey);
    alert('Обновления списковой формы не будет');
    return false;
});
```
