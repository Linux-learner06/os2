---
## Front matter
title: "Отчёт по лабораторной работе №13"
subtitle: "Фильтр пакетов"
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

Получить навыки настройки пакетного фильтра в Linux.

# Отчёт по выполнению работы  

## Управление брандмауэром с помощью firewall-cmd

1. Получены административные полномочия (`su -`).  

2. Определена зона брандмауэра по умолчанию (`public`).  

3. Выведен список доступных зон.  

4. Просмотрены службы, поддерживаемые брандмауэром.  

   ![firewall zones and services](Screenshot_1.png){ #fig:001 width=80% }

5. Получен перечень разрешённых сервисов в активной зоне.  

6. Проанализирована полная конфигурация зоны по умолчанию и отдельный запрос конфигурации зоны `public`.  
   Выводы совпали, так как зона `public` является используемой зоной по умолчанию.

   ![firewall list-all output](Screenshot_2.png){ #fig:002 width=80% }

### Добавление сервиса VNC

7. Сервис `vnc-server` был добавлен в конфигурацию времени выполнения.  

8. Проверено наличие сервиса — он присутствовал среди разрешённых.  

   ![firewall add vnc](Screenshot_3.png){ #fig:003 width=80% }

9. После перезапуска службы `firewalld` сервис исчез.  
   **Причина:** изменение относилось только к runtime-конфигурации, которая сбрасывается при перезапуске.

### Добавление VNC-сервера как постоянной настройки

10. Сервис `vnc-server` был добавлен уже как постоянный.  

11. После добавления он отсутствовал в выводе конфигурации — изменения были сохранены, но не применены.  

12. После перезагрузки конфигурации `firewalld` сервис стал отображаться среди разрешённых.

   ![firewall add permanent + reload](Screenshot_4.png){ #fig:004 width=80% }

### Добавление порта 2022/TCP

13. В конфигурацию добавлен порт `2022/tcp` как постоянный.  
14. После перезагрузки конфигурации порт появился в списке разрешённых.

## Управление брандмауэром с помощью GUI firewall-config

1. Запущено приложение `firewall-config`.  
2. Выбрано значение *Permanent* для сохранения всех вносимых изменений.  
3. В зоне `public` включены службы `http`, `https` и `ftp`.

   ![GUI service enable](Screenshot_5.png){ #fig:005 width=80% }

4. На вкладке **Ports** добавлен порт `2022/udp`.

   ![GUI add port](Screenshot_6.png){ #fig:006 width=55% }

5. В конфигурации времени выполнения изменения отсутствовали — они были внесены только в постоянную конфигурацию.  
6. После перезагрузки конфигурации настройки вступили в силу.

   ![GUI reload results](Screenshot_7.png){ #fig:007 width=80% }

## Самостоятельная работа

1. В конфигурацию брандмауэра добавлены службы:
   - telnet (через командную строку)
   - imap
   - pop3
   - smtp  
   (три последних — через `firewall-config` на вкладке **Services**)

   ![GUI enable imap pop3 smtp](Screenshot_8.png){ #fig:008 width=80% }

2. После перезагрузки конфигурации все службы отображаются как разрешённые и являются постоянными.

   ![final config](Screenshot_9.png){ #fig:009 width=80% }

# Контрольные вопросы

1. **Какая служба должна быть запущена перед началом работы с менеджером конфигурации брандмауэра firewall-config?**  
   - Должна быть запущена служба `firewalld`.

2. **Какая команда позволяет добавить UDP-порт 2355 в конфигурацию брандмауэра в зоне по умолчанию?**  
   - `firewall-cmd --add-port=2355/udp`

3. **Какая команда позволяет показать всю конфигурацию брандмауэра во всех зонах?**  
   - `firewall-cmd --list-all-zones`

4. **Какая команда позволяет удалить службу vnc-server из текущей конфигурации брандмауэра?**  
   - `firewall-cmd --remove-service=vnc-server`

5. **Какая команда firewall-cmd позволяет активировать новую конфигурацию, добавленную опцией --permanent?**  
   - `firewall-cmd --reload`

6. **Какой параметр firewall-cmd позволяет проверить, что новая конфигурация была добавлена в текущую зону и теперь активна?**  
   - `firewall-cmd --list-all`

7. **Какая команда позволяет добавить интерфейс eno1 в зону public?**  
   - `firewall-cmd --zone=public --add-interface=eno1`

8. **Если добавить новый интерфейс в конфигурацию брандмауэра, пока не указана зона, в какую зону он будет добавлен?**  
   - Он будет добавлен в зону по умолчанию (обычно `public`).

# Заключение

В ходе работы были освоены команды и инструменты для управления конфигурацией брандмауэра в Linux с помощью `firewall-cmd` и `firewall-config`, включая добавление сервисов, портов и изменение настроек зон.
