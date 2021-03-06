---
title: Независимая установка
sidebar: flexberry-designer_sidebar
keywords: Flexberry Designer, Public
toc: true
permalink: en/fd_standalone-install.html
folder: products/flexberry-designer/about/
lang: en
---

Существует [несколько способов установить и настроить](fd_install.html) [Flexberry Designer](fd_landing_page.html). Один из них - установка приложения из архива.

1.Для запуска Flexberry Designer необходимо **наличие [поддерживаемой СУБД](fo_data-service.html)** (установленной на Вашем компьютере, или где-то в сети) и [установленной лицензии](fd_installation-licensing-files.html).  
2.Необходимо проверить наличие **доступа** к серверу, который Вы будете использовать, на Вашем компьютере.  
3.В файле конфигурации **Flexberry.exe.config** необходимо изменить [настройки для сервиса данных](fo_data-service-provider-data-service.html).  
Например, для [MSSQLDataService](fo_mssql-data-service.html) можно изменить строку подключения, указав путь к серверу :

```xml 
<add key="CustomizationStrings" value="SERVER=<адрес сервера>;Trusted_connection=yes;DATABASE=CASE;"/>
```

вместо <адрес сервера> необходимо указать путь до сервера. В качестве наименования базы можно взять любое, система сама предложит создать базу данных. При использовании MS SQL Server версии Express адрес сервера будет выглядеть как **.\SQLEXPRESS**.

4.После запуска Flexberry необходимо создать Репозиторий для работы, выбрав пункт меню `Репозиторий` – `Создать новый`:

![](/images/pages/products/flexberry-designer/about/create-new-repository.png)

5.Далее необходимо настроить новый Репозиторий, выбрав пункт меню `Репозиторий` – `Редактировать свойства`, в открывшемся окне можно изменить наименование репозитория, а также необходимо подключить плагины:

![](/images/pages/products/flexberry-designer/about/rep-properties.png)

6.После чего необходимо сохранить изменения.
7.После внесения изменений в свойства репозитория, необходимо создать Проект и Конфигурацию:

* Выделить репозиторий в дереве структуры 
* Нажать кнопку `Создать Project` 
* Ввести имя проекта
* Выделить созданный проект 
* Нажать кнопку `Создать Configuration` 
* Нажать кнопку `Создать Стадия` 
* Ввести имя стадии 
* Нажать кнопку `Создать System`
* Ввести наименование System. 

После выполнения этих действий появится возможность создавать [диаграммы поддерживаемых видов](fd_editing-diagram.html): 

![](/images/pages/products/flexberry-designer/about/create-diagram.png)

{% include note.html content="При возникновении вопросов обратитесь в службу техподдержки по почте support@flexberry.ru." %}
