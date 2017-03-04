+++
title = "E.ggtimer: таймер в пару кликов в любом браузере"
date = "2016-01-31T01:43:24"
slug = "timer-in-browser-with-1-second-time-to-start"
Tags = ["fast"]
+++

Бывает, что надо вспомнить о чем-то в ближайшее время: выключить чайник, выйти к подъезду через 10 минут, 
бросить заниматься фигней через полчаса - для таких вещей идеально подходит таймер. От таймера требуется только одно:
возможность установить его в течение 5 секунд. Недавно я нашел такой с такими плюсами:

- Настроить нужно один раз, после этого будет работать на всех системах, если включена синхронизация настроек браузера
- Пользоваться легко

UPD 05.03.2017: до сих пор пользуюсь через раз, либо этим способом, либо: "Окей, гугл, таймер на пять минут".

<img src="/images/2016-01/eggtimer.png" />

<!--more-->

http://e.ggtimer.com - с виду ничего особенного (так и есть), мне понравилась простота и задание времени через URL.
Какое-то время я пользовался им так: создал 2 таймера, на 5 и на 10 минут, сделал на них закладки на панель закладок.
Это не очень удобно по 2 причинам: на chrome мне жалко места на панели, а на firefox у меня панели закладок нет.

Выход такой: оба браузера поддерживают пользовательские поисковые системы, сделаем такие.

### Firefox

1. Добавить в закладки http://e.ggtimer.com/%smin
2. Находим эту закладку в списке и делаем ей Keyword: t

### Chrome

1. Настройки - Настроить поисковые системы
2. Добавить систему `e.ggtimer` - `t` - `http://e.ggtimer.com/%smin`

После этого таймер на 5 минут ставится так:

1. Выбираем любой браузер
2. Ставим фокус на поисковую строку (`F6` / `Ctrl+L`)
3. Пишем `t 5`, `Enter`

На самом деле в Firefox не синхронизируется Keyword закладок, придется прописывать на каждой системе.
