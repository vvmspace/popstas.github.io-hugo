+++
title = "Переезд с Windows на Ubuntu 18.04, попытка номер 5"
date = "2018-04-30T18:40:00+06:00"
slug = "windows-ubuntu-18.04-migrate"
Tags = ["ubuntu", "ubuntu desktop", "18.04"]
+++

26.04.2018 вышла новая Ubuntu 18.04 LTS, и по традиции я решил попробовать на нее переехать, этот пост - отчет о переезде.

Решил не писать один пост месяц, а то будет как в тот раз (например, аналогичный пост по переезду на MacOS так никогда и не вышел, остался на gist - https://gist.github.com/popstas/d0fdf4cd5c37dc5b8d93), несколько других похожих постов вообще остались лежать на локалке.

Так что буду писать по мере наступания на грабли и пополнять статью.

UPD 13.07.2018: цикл статей оборвался из-за того, что я не смог побороть глюк Nvidia (см. комменты) и вернулся на Windows. Можно сказать, что это был такой сложный способ найти крутую скриншотилку Monosnap :)

<img src="/images/2018-04/windows-ubuntu.jpg" />
<!--more-->

Статьи цикла можно было бы найти по тегу 18.04, но hugo что-то сломался и выдает RSS ленту вместо HTML страницы, так что придется вручную.

#### Статьи цикла “Переезд на Ubuntu 18.04”:
- [Настройка Nvidia Geforce 1050Ti на Ubuntu 18.04](/blog/2018/04/30/nvidia-ubuntu-18.04/)
- [Черная полоса в яндекс браузере на Ubuntu 18.04, GPU ускорение](/blog/2018/04/30/ubuntu-yandex-browser-black-line/)
- [Настройка звука Asus DGX Xonar 5.1 на Ubuntu 18.04](/blog/2018/04/30/asus-dgx-xonar-ubuntu-18.04/)
- [Не спрашивать пароль от связки ключей при каждой загрузке на Ubuntu 18.04](/blog/2018/04/30/windows-ubuntu-18.04-migrate/)
- [Настройка времени в Ubuntu 18.04 так, чтобы при перезагрузке в Windows часы не слетали](/blog/2018/05/01/time-zone-ubuntu-windows-reboot/)
- [Настройка gxneur в Ubuntu 18.04](/blog/2018/05/01/gxneur-punto-switcher-ubuntu/)
- [Переключение окон назад по Alt+Shift+Tab в Ubuntu 18.04](/blog/2018/05/01/alt-shift-tab-in-ubuntu/)
- [Настройка Gnome Terminal: Solarized Dark и быстрый выбор профиля](/blog/2018/05/01/gnome-terminal-solarized/)

#### Зачем мне это надо
1. Всегда мечтал о терминале на десктопной системе, остальное в общем вытекает из этого пункта.
2. Ставить программы без костылей. Например, надо мне pylint, php, npm, shellcheck для нормальной работы IDE, хочу ставить их без костылей
3. Docker, ansible, vagrant без костылей на локалке
4. Прозрачная работа vim на удаленке, с буфером обмена
5. Настраивать систему по возможности через ansible
6. Больше пересечений десктопной и серверной оси, это в теории позволяет переносить какие-то решения между системами



## Список нерешенных проблем:
- ~~[Настройка времени так, чтобы при перезагрузке в Windows часы не слетали](/blog/2018/05/01/time-zone-ubuntu-windows-reboot/)~~
- ~~[Не спрашивать пароль от связки ключей при каждой загрузке](/blog/2018/04/30/windows-ubuntu-18.04-migrate/)~~
- ~~[Punto Switcher, без него жизни нет](/blog/2018/05/01/gxneur-punto-switcher-ubuntu/)~~
- ~~[Alt+Shift+Tab](/blog/2018/05/01/alt-shift-tab-in-ubuntu/)~~
- ~~[Настройка терминала, быстрый доступ к серверам, Solarized Dark](/blog/2018/05/01/gnome-terminal-solarized/)~~
- Ctrl+Tab перестал переключать вкладки в Crome, Vscode и других программах
- RDP, доступ с машины и к ней
- ~~Скриншоты, с рисованием на них и аплоадом~~ Monosnap
- ~~Удобный DM, допиливание Gnome 3 до рабочего варианта~~ [Мой список расширений](https://gist.github.com/popstas/53927ce7a36898e398a126d71886eea1)
- ~~Нормальный launcher, замена Alfred и Hain~~ Albert
- Работа виртуальных рабочих столов не хуже, чем в Windows (года 2 назад я бы не поверил, что скажу такое)
- ~~Монтирование сетевых дисков с медиа и проектами~~ это получилось через NFS
- Нормальная работа спящего режима (сейчас он не просыпается)
- ~~Удобное переключение на OpenVPN и обратно (не думал, что будет хуже, чем на винде, но сходу не нашел GUI)~~ стандартный гномовский UI работает, но не понимает .ovpn файлы, где всё вместе: ключи и конфиг, а также не нашел способа исключать из VPN домены



## Настройка железа

### Моя машина:
- Intel 6700K, 32 Gb DDR4, SSD 240 Gb
- Nvidia Geforce 1050Ti 4 Gb (Gigabyte)
- 2 монитора, 27” 2560x1440, 43” 3840x2160
- Внешний звук M-Audio Fast Track MK-2
- Внутренний звук Asus DGX Xonar 5.1

### Видео
[Настройка Nvidia Geforce 1050Ti на Ubuntu 18.04](/blog/2018/04/30/nvidia-ubuntu-18.04/)

### Звук
#### Asus DGX
[Настройка звука Asus DGX Xonar 5.1 на Ubuntu 18.04](/blog/2018/04/30/asus-dgx-xonar-ubuntu-18.04/)

#### M-Audio Fast Track MK-2
Еще не смотрел толком, но вывод звука работает.

## Настройка системы
### Дата и время
Если коротко, то `timedatectl set-local-rtc 1`. [Подробнее](/blog/2018/05/01/time-zone-ubuntu-windows-reboot/)

### Пароль от связки ключей при каждой загрузке
Смените пароль на папку “Вход” в связке на пустой. [Подробнее](/blog/2018/04/30/windows-ubuntu-18.04-migrate/)

### Спящий режим
#### Компьютер просыпается сразу после засыпания
Нужно отключить все триггеры. Определяем устройства, которые могут будить:

```
cat  /proc/acpi/wakeup | grep enabled
GLAN  S4 *enabled   pci:0000:00:1f.6
XHC   S4 *enabled   pci:0000:00:14.0
```

После чего отправляем туда же имена этих устройств:

```
echo GLAN > /proc/acpi/wakeup
echo XHC > /proc/acpi/wakeup
```

Мне это помогло.

## Настройка софта
- [Настройка gxneur в Ubuntu 18.04](/blog/2018/05/01/gxneur-punto-switcher-ubuntu/)
- [Переключение окон назад по Alt+Shift+Tab в Ubuntu 18.04](/blog/2018/05/01/alt-shift-tab-in-ubuntu/)
- [Настройка Gnome Terminal: Solarized Dark и быстрый выбор профиля](/blog/2018/05/01/gnome-terminal-solarized/)

### Windows - Linux Remote Desktop
Особо я не надеялся, что что-то получится и вообще точно знал, что такого гладкого RDP, как на Windows-Windows соединении не будет.

#### Что я пробовал:
1. TigerVNC Не получилось завести.
2. x2go На [хабре](https://habr.com/post/329066/) прочитал, что это семантический протокол, то есть передает только изменения, а не тупо картинку. Поставил, запустил, работает, но выводит 2 монитора как есть и тормозит.
3. xrdp & x11rdp x11rdp пробовал собрать через [X11RDP-o-Matic](https://github.com/scarygliders/X11RDP-o-Matic), в результате получился только [issue](https://github.com/scarygliders/X11RDP-o-Matic/issues/91).

~~Думаю, надо просто смириться с тем, что удаленки больше не будет~~ не смог смириться.