+++
title = "Подсветка Python синтаксиса в PhpStorm"
date = "2017-03-13T00:30:00"
slug = "python-highlight-syntax-in-phpstorm"
Tags = ["phpstorm", "python", "textmate"]
+++

Я долго терпел, но сегодня решил выяснить: можно ли добавить поддержку Python в PhpStorm.

Оказалось, что можно и делается хоть и не за минуту, а за 5-10 минут.

Tl;dr: можно сделать только подсветку, Solarized Dark нельзя.
[Официальная документация](https://confluence.jetbrains.com/display/PhpStorm/TextMate+Bundles+in+PhpStorm)

<img src="/images/2017-03/phpstorm-python.gif" />
<!--more-->

## Wat?!
Пару слов о том, почему я я хочу странного.

Я пишу на PHP и использую все, что может дать PhpStorm. В ресурсах рабочей машины я особо не ограничен, поэтому все проекты
(все, что больше одного файла) я открываю в нем.

У меня есть обвешенные плагинами Sublime Text и Atom, но JetBrains все-таки их делает: большая часть идет из коробки,
остальное настроено лучше, чем в редакторах, т.к. в PhpStorm я провожу больше всего времени.

На JetBrains IDEA пересаживаться не очень хочется, тем более из-за одного Python, т.к. остальные языки, на которых я пишу
(bash, ansible, markdown, go, lua) в разной степени поддерживаются штормом.

С Python я соприкасаюсь мало, по большей части читаю чужие проекты, поэтому я долго просто открывал python-проекты в шторме
и читал их в черно-белом варианте.

## Настройка

Итак, решение: можно добавить в PhpStorm TextMate bundle с синтаксисом языка, который поддерживает TextMate.

1. Гуглим `TextMate Python`, находим [бандл](https://github.com/textmate/python.tmbundle), качаем.
2. Идем в Settings - Editor - TextMate Bundles в шторме, там добавляем скачанную папку.
3. Идем в Settings - Editor - File Types, находим там `Files supported viaTextMate bundles`, добавляем маску `*.py`.
4. Там же мапим используемые штормовские цветовые схемы на схемы TextMate.

Вот в общем-то и все, после этого PhpStorm начинает подсвечивать Python, кроме этого он ничего не даст, если нужен 
список функций, переходы к определениям и прочее, то в PhpStorm это настроить нельзя.
