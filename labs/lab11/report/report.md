---
## Front matter
title: "Отчёт по лабораторной работе №11"
subtitle: "Управление загрузкой системы"
author: "Эзиз Хатамов"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true
toc-depth: 2
lof: true
lot: true
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
    - spelling=modern
    - babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы

Получить навыки работы с загрузчиком системы GRUB2.

# Отчёт по выполнению работы  

## Модификация параметров GRUB2  

1. В терминале были получены полномочия администратора с помощью команды su -.  
   Затем был открыт файл /etc/default/grub для редактирования с использованием текстового редактора mcedit.  

2. В файле установлен параметр GRUB_TIMEOUT=10, задающий время отображения меню загрузки в секундах.  
   После внесения изменений файл был сохранён и закрыт.  

   ![Редактирование конфигурационного файла /etc/default/grub](Screenshot_1.png){ #fig:001 width=80% }  

3. Для применения изменений была выполнена команда grub2-mkconfig > /boot/grub2/grub.cfg.  
   В результате была сгенерирована новая конфигурация загрузчика и добавлена запись для UEFI Firmware Settings.  

   ![Обновление конфигурации загрузчика GRUB2](Screenshot_2.png){ #fig:002 width=80% }  

4. После перезагрузки системы в процессе загрузки появилось меню GRUB, содержащее несколько записей ядра.  
   Это подтверждает успешное применение параметра GRUB_TIMEOUT.  

   ![Отображение меню загрузчика GRUB при старте системы](Screenshot_3.png){ #fig:003 width=80% }  

## Режим восстановления (rescue mode)  

1. В меню загрузчика GRUB была выбрана текущая версия ядра, после чего нажата клавиша e для перехода в режим редактирования параметров загрузки.  

2. В конце строки, начинающейся с linux, был добавлен параметр systemd.unit=rescue.target.  
   После чего загрузка продолжена сочетанием клавиш Ctrl + X.  

   ![Редактирование параметров загрузки с указанием systemd.unit=rescue.target](Screenshot_4.png){ #fig:004 width=80% }  

3. После загрузки в режиме восстановления была выполнена команда systemctl list-units,  
   которая отобразила список активных системных модулей, подтверждая работу в минимальной среде.  

4. Далее была выполнена команда systemctl show-environment для отображения текущих переменных среды оболочки.  

   ![Просмотр активных юнитов и переменных среды в rescue-режиме](Screenshot_5.png){ #fig:005 width=80% }  

5. После проверки состояния системы была выполнена перезагрузка с помощью systemctl reboot.  

## Режим экстренной загрузки (emergency mode)  

1. После перезапуска снова открыт редактор параметров загрузки GRUB.  
   В конце строки ядра добавлен параметр systemd.unit=emergency.target.  
   Загрузка продолжена клавишами Ctrl + X.  

   ![Переход в emergency-режим через добавление параметра systemd.unit=emergency.target](Screenshot_6.png){ #fig:006 width=80% }  

2. После входа в систему в режиме emergency просмотрен список активных модулей с помощью systemctl list-units.  
   Количество загруженных юнитов минимально, что подтверждает работу в режиме экстренного восстановления.  

   ![Работа системы в emergency-режиме](Screenshot_7.png){ #fig:007 width=80% }  

## Сброс пароля root  

1. После очередной перезагрузки выбрана строка с текущим ядром и нажата клавиша e.  
   В конце строки ядра добавлен параметр rd.break.  
   Загрузка продолжена клавишами Ctrl + X.  

   ![Переход в initramfs с параметром rd.break для сброса пароля root](Screenshot_8.png){ #fig:008 width=80% }  

2. На этапе initramfs система остановила загрузку и предложила войти в режим обслуживания.  
   Для получения доступа к файловой системе в режиме чтения-записи введена команда mount -o remount,rw /sysroot.  

3. Попытка выполнить команды chroot /sysroot и passwd завершилась неудачно, поскольку данные утилиты отсутствовали в текущем окружении.  

   ![Попытка сброса пароля root в initramfs окружении](Screenshot_9.png){ #fig:009 width=80% }  

# Контрольные вопросы  

1. **Какой файл конфигурации следует изменить для применения общих изменений в GRUB2?**  
   - /etc/default/grub — в этом файле задаются основные параметры загрузчика, такие как время отображения меню, параметры ядра и режим вывода.  

2. **Как называется конфигурационный файл GRUB2, в котором вы применяете изменения для GRUB2?**  
   - /boot/grub2/grub.cfg — основной конфигурационный файл загрузчика GRUB2, формируемый автоматически на основе содержимого /etc/default/grub и других системных параметров.  

3. **После внесения изменений в конфигурацию GRUB2, какую команду вы должны выполнить, чтобы изменения сохранились и воспринялись при загрузке системы?**  
   - grub2-mkconfig > /boot/grub2/grub.cfg — команда генерирует новый файл конфигурации загрузчика с учётом внесённых изменений.  

# Заключение  

В ходе работы были изучены принципы настройки и модификации параметров загрузчика GRUB2, способы перехода в режимы восстановления и экстренной загрузки, а также методы сброса пароля пользователя root. Полученные навыки позволяют администратору эффективно управлять процессом загрузки и восстановлением системы Linux.  
