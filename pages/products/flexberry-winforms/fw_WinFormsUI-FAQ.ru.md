---
title: WinForms UI FAQ
sidebar: product--sidebar
keywords: Windows UI (формы)
toc: true
permalink: ru/fw_win-forms-u-i--f-a-q.html
folder: products/flexberry-winforms/
lang: ru
---

# 1
## Вопрос
У нас есть объекты Корпус и Аудитория. Корпус является мастером для аудитории. Мы хотим сделать форму для редактирования этих объектов примерно в таком формате: вверху ObjectListView с Корпусами, внизу GroupEdit с аудиториями корпуса. При выборе в OLV корпуса, в GE подгружаются соответствующие аудитории. Аудитории в GE хочется редактировать, а не вызывать отдельно форму АудиторияE. Как лучше это сделать?

Я создал с кейсберри дополнительную listform для представления КорпусL. Для нее сгенерировалась соответствующая .Net форма, куда я попытался добавить GroupEdit. Но не могу понять, как указать объект данных в нем. Открывается какая-то форма с выбором пакетов, пустая.

Может можно как-нибудь по другому сделать?
## Ответ
Для решения этой задачи надо зайти с другой стороны: нужна форма редактирования корпуса с детейлами (аудитории должны быть детейлами чтобы можно их было редактировать в GroupEdit). Затем нужно переключать мастера так как описано в этой статье: <http://storm:2011/SwitchEditingObject.ashx>
В этом случае блокировки будут адекватно отрабатывать. Но, если аудитории отдельно тоже могут редактироваться (если существует отдельная форма редактирования для них), то GroupEdit должен быть настроен на работу с блокированием редактируемых объектов (<http://storm:2011/LockRowsInGroupEdit.ashx>). 

# 2
## Вопрос

## Ответ

# 3
## Вопрос

## Ответ
