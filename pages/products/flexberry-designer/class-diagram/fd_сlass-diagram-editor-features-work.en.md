---
title: Особенности работы с редактором диаграммы классов
sidebar: flexberry-designer_sidebar
keywords: Flexberry Designer, Public
toc: true
permalink: en/fd_class-diagram-editor-features-work.html
folder: products/flexberry-designer/class-diagram/
lang: en
---

Редактор [диаграммы классов](fd_class-diagram.html) в Flexberry Designer имеет ряд особенностей:

* **Переименование классов**
Для того чтобы **переименовать класс** на диаграмме следует нажать на него правой кнопкой мыши и выбрать **пункт выпадающего меню "Переименовать"**. Будет предложено ввести новое имя класса.  
Если изменить имя класса непосредственно на диаграмме, то создастся новый класс с указанным именем, а старый будет удален. Как следствие, удалятся все атрибуты и представления старого класса.
* **Изменение роли у ассоциации**
Если изменить роль у ассоциации непосредственно на диаграмме (не поднимая форму редактирования ассоциации), то будет создана новая ассоциация. Все атрибуты старой ассоциации будут удалены (например, `Storage`, `Description`).  
Если необходимо **изменить роль ассоциации, сохраняя все старые атрибуты**, то следует нажать правой кнопкой мыши по ассоциации и выбрать **пункт выпадающего меню "Редактировать свойства"** и редактировать свойства ассоциации в появившемся окне.
* **Автоматическое прописывание Storage у ассоциации**
При добавлении новой ассоциации, ее свойство `Storage` прописывается автоматически в соответствие с заданной ролью, если роль не задана, то прописывается имя стартового класса. 
