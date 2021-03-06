---
title: Создание приложения
sidebar: flexberry-designer_sidebar
keywords: Flexberry Designer
toc: true
permalink: en/fd_flexberry.html
folder: products/flexberry-designer/creating/
lang: en
---

## [Подход к разработке на Flexberry](fd_model-driven-architecture.html)

При разработке на Flexberry используется подход "разработка, управляемая моделями" (Model-Driven Development).

Описание подхода можно найти в [статье Описание методологии MDD](fd_model-driven-architecture.html) (а также в [википедии](http://ru.wikipedia.org/wiki/Model_Driven_Architecture)).
Реализация приложения делится на следующие этапы:

*	[Анализ](fd_analys.html)
*	[Прототипирование приложения](fd_using-quick-prototyping.html)
*	[Доработка прототипа до уровня конечного приложения](fd_development.html)

В результате возможны два сценария работы с приложением:

*	Генерация прототипа на стороне платформы и публикация в облако (в качестве облака используется [Windows Azure](http://www.windowsazure.com))
*	Генерация исходных кодов на стороне платформы и размещение их в Git-репозитории.

## [Анализ](fd_analys.html)

1.	Осознать проблему
2.	Визуально описать приложение с помощью UML
    *	Создать все необходимые диаграммы 
        * [активностей](fd_activity-diagram.html)
        * [состояний](fd_statechart-diagram.html)
        * [вариантов использования](fd_use-case-diagram.html)
        * [развертывания](fd_deployment-diagram.html)
        * [сотрудничества](fd_collaboration-diagram.html)
        * [последовательностей](fd_sequence-diagram.html)
    *	[Создать диаграмму классов](fd_editing-diagram.html)
        *	Создать классы для сущностей
        *	Установить связи между классами

## [Прототипирование приложения](fd_prototype-creation.html)

1.Создать UML-описание форм при помощи [операции быстрого прототипирования](fd_prototype-creation.html)
2.Построить прототип приложения
* 	Включает в себя (происходит автоматически):
    *	Создание базы приложения и полномочий
    *	Генерация приложения
    *	Публикация в Azure/Git-репозитории

*На данном этапе приложение уже можно запустить*

3.Подключить необходимые функциональные модули
*	[При необходимости отключить встроенную подсистему полномочий](efs_secutity.html)
*	[Подключить и настроить подсистему Аудита](fd_audit-flexberry-setup.html)
*	Подключить и настроить подсистему интеграции и запуска отчетов
*	Подключить и настроить ГИС-компонент для работы с электронными картами
4.[Выбрать тему визуального оформления приложения](fa_choose-theme.html)

## [Доработка прототипа до уровня конечного приложения](fd_application-development.html)

### Общие принципы доработки приложения с помощью Flexberry

1.	[Разработка через модификацию модели](fd_code-generation.ashx)
2.	Описание процесса получения исходного кода.
3.	[Описание процесса перегенерации кода](fd_code-generation.html).
4.	[Использование скобок программиста](fo_programmer-brackets.html).

#### Сценарии доработки приложения

1.	Управление формами приложения
    *	Добавление новых форм
    *	Настройка списковых форм
        *	Настройка представлений
        *	Настройка кнопок
    *	Настройка форм редактирования и просмотра
        *	Настройка представлений
2.	Настройка пользовательского интерфейса
    *	Установка заголовков форм
    *	Выравнивание элементов на формах
    *	Приведение к общему стилю, цвету и шрифтам элементов форм
    *	Изменение размера форм
    *	Добавление локализации
3.	Настройка визуальной логики
    *	Подключение предиктивного ввода
    *	Реализация режима “Read-only”
4.	Настройка валидации данных
    *	Настройка валидации на клиентской стороне
    *	Настройка валидации на серверной стороне
5.	Реализация бизнес-логики
    *	Добавление проверки при обновлении данных через бизнес-сервер.
    *	Обновление нескольких объектов одной транзакцией.
    *	Проверка уникальности введенных данных.
    *	Проверка валидности изменений данных объекта.
    *	Добавление методов дочитки\вычитки данных
6.	Настройка обработки ошибок
    *	Включение\выключение ведения лога ошибок
    *	Настройка формы ошибок
    *	Настройка перехвата исключительных ситуаций


