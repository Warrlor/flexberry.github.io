---
title: Межформенное взаимодействие в Web-приложениях
sidebar: flexberry-aspnet_sidebar
keywords: Flexberry ASP-NET
toc: true
permalink: ru/fa_form-interaction.html
folder: products/flexberry-aspnet/
lang: ru
---


## Сценарий открытия страницы редактирования со списка

Адрес страницы редактирования определяется настройкой [WebObjectListView](fa_web-object-list-view.html).`EditPage`. На страницу посылаются параметры через адресную строку:

* `Mode` - режим открытия страницы, может быть `readonly`.
* `ReturnUrl` - адрес возврата, содержит не только адрес страницы, но и адрес WOLV, с которого была вызвана страница редактирования.
* `DataObject` - передается вместе с POST запросом (передается через Request.Form["DataObject"]). Хранит в себе сериализованный в XML экземпляр нового объекта (подробнее см. [статью Пример открытия web-формы редактирования с передачей объекта с частично заполненными полями](fa_open-edit-form-web-example.html)).
* `PK` - первичный ключ редактируемого объекта.

{% include note.html content="Если объект новый, то при первом открытии `PK` не посылается, однако, при первом сохранении объекта (без возврата на страницу со списком), этот параметр появляется в адресной строке." %}

Страница редактирования наследуется от [BaseEditForm](fa_base-edit-form.html).

При загрузке страницы срабатывает несколько виртуальных методов [BaseEditForm](fa_base-edit-form.html), которые можно переопределить на [странице редактирования](fa_web-edit-form.html):

* `PreLoad()` - метод вызывается самым первым в методе `Page_Load()`.
* `PrepareDataObject` - подготовка объекта к выгрузке на страницу. В этом методе (на BaseEditForm) происходит вычитка объекта по `PK`, или десериализация из POST-запроса (XML). Не рекомендуется переопределять этот метод.
* `PreApplyToControls()` - метод вызывается перед "размазыванием" данных объекта по контролам страницы. На момент вызова этого метода объект уже вычитан из базы (либо при PostBack'e значения уже записаны в объект), можно совершить какие-то дополнительные манипуляции с объектом прежде чем его значения попадут на страницу.
* `AfterApplyToControl()` - метод вызывается после "размазывания" данных объекта по контролам страницы. На момент вызова этого метода все контролы на странице уже обновлены в соответствии с данными объекта. Также метод вызывается после сохранения объекта (подробнее см. ниже).
* `PostApplyToControl()` - вызывается сразу за `AfterApplyToControls()`
* `PostLoad()` - вызывается самым последним при загрузке страницы.

## Сценарий сохранения объекта

Последовательность срабатывания методов при нажатии кнопки `Сохранить`:

* javaScript метод `saveBtnClickHandler` (файл: ~\shared\script\jquery.icsEditForm.js)
    * Запуск ASP.NET валидаторов
    * Запуск установленных прикладными разработчикаи обработчиков события сохранения
    * Ручная отправка PostBack'а на сервер
* Серверный виртуальный метод `SaveBtn_Click` - обработчик нажатия на кнопку сохранения `BaseEditForm`.
    * `PreSave()` - виртуальный метод, срабатывающий перед фактическим сохранением объекта. Возвращает значение типа `bool`, если возвращено значение `false`, то объект не уйдет на сохранение.
    * `SaveObject()` - виртуальный метод, в котором происходит непосредственно сохранение объекта. Не рекомендуется переопределять этот метод без вызова базового метода.
    * `PostSaveObject()` - виртуальный метод, срабатывающий после сохранения объекта. На момент вызова метода объект уже попадает в базу, следовательно срабатывает бизнес-сервер обновления этого объекта и все изменения, происходящие с объектом в бизнес-сервере уже доступны. Сразу после вызова этого метода происходит "размазывание" данных объекта по контролам.
    * `AfterApplyToControls()` - метод, описанный в блоке `Сценарий открытия страницы редактирования со списка`.
* Если на момент вызова метода сохранения объект был новый (то есть его не было в базе), то происходит редирект на эту же самую страницу редактирования, но в адресную строку добавляется `PrimaryKey` объекта.

{% include note.html content="Если страница не проходит валидацию, данные на сервер не отправляются вовсе. Таким образом снижается нагрузка на сервер, однако стоит понимать, что серверный обработчик кнопки сохранения не сработает, если страница не прошла валидацию." %}

{% include note.html content="
После сохранения объекта в методе SaveObject на базовой форме редактирования (BaseEditForm) вызывается метод [WebBinder-а](fa_web-binder.html) ApplyDataToControls,
в нем, для контролов, которые реализуют ISaveActionCompatible сбрасывается флаг IsSaveNow, ну и всем контролам, соответствующим полям объекта данных проставляются актуальные значения из объекта.

Если при сохранении возникло необработанное исключение (например выбрашенное в [бизнес-сервере](fo_bs-example.html)), выполнение метода SaveObject прервется, и метод [WebBinder-а](fa_web-binder.html) ApplyDataToControls не будет вызван.  
В контролах не будет сброшен флаг IsSaveNow, это черевото тем, что групэдит не будет разбираться со статусами строк, соответствующих детейлам, и оставит у всех строк статус WGEStatus.None,
а клиентский код групедита не посылает детейлы с таким статусом обратно на сервер (он посылает только новые, отредактированные или удаленные), из-за чего, если после возникновения исключения пользователь ничего не менял в строках групэдита, и вновь отправил форму на сохранение, то у объекта данных не будет детейлов (они просто не будут отправлены с клиента).

Для решения этой проблемы базовая форма редактирования отлавливает исключения при сохранении объекта, и в случае их возникновения вызывает метод [WebBinder-а](fa_web-binder.html) ApplySavingFlagToSaveActionCompatibleControls, который сбросит флаг IsSaveNow у контролов реализующих ISaveActionCompatible, затем базовая форма редактирования пробрасывает исключение дальше.
Внутри метода SaveObject это выглядит следующим образом:

``` csharp
try
{
   ds.UpdateObject(ref dObj);
   PostSaveObject();
}
catch
{
   wb.ApplySavingFlagToSaveActionCompatibleControls(Controls, View, false);
 
   throw;
}
```

Если в прикладном коде используется не технологическая, а прикладная базовая форма редактирования, переопределяющая метод SaveObject, обязательно нужно, чтобы подобный код обрабатывал исключения при сохранении объекта.
" %}

## LookUp'ы

### Сценарий открытия страницы со списком на LookUp

Настройки [LookUp](fa_lookup-overview.html):
* `LimitFunction` - ограничение, накладываемое на отображаемые на открываемой LookUp-странице данные.
* `LookUpFormURL` - адрес страницы, которая вызовается на [LookUp](fa_lookup-overview.html). По умолчанию смотрит на `LookUpForm`.

{% include note.html content="На странице должен быть [WebObjectListView|WOLV) с установленной настройкой `LookUp = true;`, а также должен происходить разбор передаваемых через адресную строку параметров. Подробнее см. [LookUpForm](fa_lookup-form.html)" %}

При открытии страницы на [LookUp](fa_lookup-overview.html) передается адрес web-контрола, с которого он был инициирован (страница, а также местоположение самого контрола). Также передается PrimaryKey выбранного объекта (если такой был на момент открытия [LookUp-Overview|LookUp'a)) для подсветки его на открываемой странице.

### Сценарий выбора значения мастера на LookUp-странице

При выборе какого-либо объекта на списке LookUp-страницы происходит возврат PrimaryKey этого объекта по адресу, отправленному при открытии LookUp-страницы.

После возврате значения на списковую web-форму, при необходимости, заполняются поля выбранного мастера.