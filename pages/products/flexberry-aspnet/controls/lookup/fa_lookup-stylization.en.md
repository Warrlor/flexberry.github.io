---
title: Стилизация LookUp
sidebar: flexberry-aspnet_sidebar
keywords: Flexberry ASP-NET
toc: true
permalink: en/fa_lookup-stylization.html
lang: en
---

## Настройка внешнего вида иконок лукапа в теме Crimea

В тему Crimea были добвлены несколько вариатнов шрифтовых иконок для кнопок лукапа.

Для изменения иконок лукапа нужно в `Controls\Lookup\_Variables.less` поменять значение у соответствующих переменных:

```css
@lookupShowObjectBtn: '\e879';    // Кнопка "Показать объект".
@lookupLookupBtnIcon: '\e81d';    // Кнопка "Выбрать".
@lookupClearBtnIcon: '\e806';     // Кнопка "Очистить".
```

На выбор доступны следующие иконки:

![](/images/pages/products/flexberry-aspnet/themes/lookup-icons.png)
