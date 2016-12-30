---
title: Автодополнение в лукапе
sidebar: ember-flexberry_sidebar
keywords: Flexberry Ember
toc: true
permalink: ru/ef_lookup-autocomplete.html
folder: products/ember-flexberry/controls/lookup/
lang: ru
summary: Описаны основные особенности использования лукапа в режиме автодополнения.
---

## Описание

Автодополнение в [лукапе](ef_lookup.html) позволяет осуществлять ввод с клавиатуры части значения и последующий выбор из предложенных вариантов.

Чтобы перевести лукап в режим автодополнения, достаточно добавить свойство:

```
{{flexberry-lookup
	choose='showLookupDialog'
	remove='removeLookupValue'
	value=model.employee1
	relationName='employee1'
	projection='EmployeeL'
	title='Employees'
	readonly=readonly
	displayAttributeName='FirstName'

	autocomplete=true	
}}
```


## Свойства, используемые в автодополнении

Ниже указаны основные добавленные в лукап свойства для работы автодополнения.

{|
! Свойство !! Описание !! Дефолтное значение
|- class="tablerow"
| autocomplete || Режим автокомплита, в режиме "Только для чтения" не работает || false
|- class="tablerow"
| minCharacters || Минимальное количество символов для автокомпилита, в режиме автокомплита и в режиме выпадающего списка || 1
|- class="tablerow"
| maxResults || Максимальное количество отображаемых записей в режиме автокомплита и в режиме в режиме выпадающего списка, не обязательное свойство || 10
|- class="tablerow"
| 
|}

## Особенности запроса к серверу за результатами автодополнения

{% include important.html content="Информация устарела, свойство limitFunction удалено, загрузка через `store`." %}

При выполнении запроса к серверу за результатами автодополнения передаются следующим образом сформированные опции:

* Максимальное количество возвращаемых значений определяется как autocompleteMaxResults.
* Накладываемое ограничение на возвращаемые значения определяются как объединение через "И" наложенной функции ограничения на лукап (limitFunction) и функции Contains.