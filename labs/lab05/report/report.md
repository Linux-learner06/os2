---
## Front matter
title: "Отчёт по лабораторной работе №5"
subtitle: "Управление системными службами"
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

Получить навыки управления системными службами операционной системы посредством systemd.

# Отчёт по выполнению работы  

## Управление сервисами  

1. Сначала были получены полномочия администратора с помощью команды `su -`.  

2. Выполнена проверка статуса службы **Very Secure FTP (vsftpd)**.  
   Вывод показал, что сервис отсутствует в системе, так как он ещё не был установлен.  

   ![Проверка статуса vsftpd до установки](Screenshot_1.png){ #fig:001 width=80% }  

3. Установка службы **vsftpd** произведена с помощью пакетного менеджера `dnf`.  
   Процесс завершился успешно, и пакет был добавлен в систему.  

   ![Установка vsftpd](Screenshot_2.png){ #fig:002 width=80% }  

4. Служба **vsftpd** была запущена.  
   Проверка статуса подтвердила, что сервис находится в состоянии **active (running)**, однако он не был добавлен в автозапуск.  

   ![Запуск службы vsftpd](Screenshot_3.png){ #fig:003 width=80% }  

5. Далее сервис был включён в автозапуск.  
   Повторная проверка показала, что статус изменился на **enabled**.  

   ![Добавление vsftpd в автозапуск](Screenshot_4.png){ #fig:004 width=80% }  

6. После выполнения команды отключения сервис был удалён из автозапуска.  
   Проверка подтвердила, что состояние вновь стало **disabled**, при этом сам сервис продолжал работать.  

   ![Удаление vsftpd из автозапуска](Screenshot_5.png){ #fig:005 width=80% }  

7. Отображён список символических ссылок, отвечающих за запуск сервисов.  
   В изначальном выводе ссылка на `vsftpd.service` отсутствовала. После повторного включения сервиса в автозапуск ссылка на соответствующий юнит была создана.  

   ![Символические ссылки сервисов и повторное включение vsftpd](Screenshot_6.png){ #fig:006 width=80% }  

8. Для анализа зависимостей юнита был выведен список сервисов, от которых зависит работа `vsftpd`, а также перечень юнитов, которые зависят от него.  

   ![Просмотр зависимостей юнита vsftpd](Screenshot_7.png){ #fig:007 width=80% }  

## Конфликты юнитов  

1. Были получены полномочия администратора и установлены пакеты **iptables**.  

2. Проверен статус служб **firewalld** и **iptables**.  
   В результате было установлено, что **firewalld** активен и включён, а **iptables** — неактивен.  

3. Выполнена попытка запуска обеих служб.  
   При запуске **firewalld** сервис **iptables** оставался неактивным. После запуска **iptables** — наоборот, служба **firewalld** завершала работу. Это подтвердило наличие конфликта между юнитами.  

   ![Запуск firewalld и iptables](Screenshot_8.png){ #fig:008 width=80% }  

4. Просмотрен конфигурационный файл юнита **firewalld**.  
   В нём указано, что сервис конфликтует с `iptables.service`, `ip6tables.service`, `ebtables.service`, `ipset.service`.  

   ![Файл юнита firewalld](Screenshot_9.png){ #fig:009 width=80% }  

5. Просмотрен конфигурационный файл юнита **iptables**.  
   В нём отсутствует явное указание конфликтующих служб.  

   ![Файл юнита iptables](Screenshot_10.png){ #fig:010 width=80% }  

6. Служба **iptables** была остановлена, а затем запущен **firewalld**.  
   После этого выполнена команда `systemctl mask iptables`, которая создала символическую ссылку на `/dev/null` для файла `/etc/systemd/system/iptables.service`. Это действие сделало невозможным случайный запуск **iptables**.  

7. При попытке запуска **iptables** система выдала сообщение об ошибке, указав, что юнит замаскирован и не может быть активирован. Аналогично, при попытке добавить сервис в автозапуск было выведено предупреждение о том, что юнит замаскирован.  

   ![Замаскированный сервис iptables](Screenshot_11.png){ #fig:011 width=80% }  

## Изолируемые цели  

1. Были получены полномочия администратора. Затем выполнен переход в каталог `/usr/lib/systemd/system` и определён список целей, которые могут быть изолированы.  
   Результаты команды показали наличие строк `AllowIsolate=yes` у ряда целей, включая `rescue.target`, `multi-user.target`, `graphical.target`, `reboot.target` и другие.  

   ![Список изолируемых целей](Screenshot_12.png){ #fig:012 width=80% }  

2. Система была переведена в режим восстановления с помощью изоляции цели `rescue.target`.  
   После выполнения команды система потребовала ввод пароля пользователя root для доступа в режим обслуживания.  

   ![Переход в режим восстановления](Screenshot_13.png){ #fig:013 width=80% }  

3. Для перезапуска системы была изолирована цель `reboot.target`. Это действие привело к перезагрузке ОС.  

## Цель по умолчанию  

1. Выведена текущая цель, установленная по умолчанию.  
   По умолчанию системой использовалась цель `graphical.target`.  

   ![Проверка цели по умолчанию](Screenshot_14.png){ #fig:014 width=80% }  

2. Цель по умолчанию была изменена на `multi-user.target`.  
   После перезагрузки система загрузилась в текстовом режиме.  

3. Для возврата графического режима в качестве цели по умолчанию снова была установлена цель `graphical.target`.  
   После очередной перезагрузки система загрузилась в графическом режиме.  

   ![Установка графического режима по умолчанию](Screenshot_15.png){ #fig:015 width=80% }  

# Контрольные вопросы  

1. **Что такое юнит (unit)? Приведите примеры.**  
   - Юнит — это объект управления в systemd, описывающий, как должен запускаться, останавливаться и управляться ресурс.  
   - Примеры:  
     - `service` (службы, например `sshd.service`)  
     - `target` (цели, например `multi-user.target`)  
     - `mount` (точки монтирования, например `home.mount`)  
     - `timer` (таймеры, например `logrotate.timer`)  

2. **Какая команда позволяет убедиться, что цель больше не входит в список автоматического запуска при загрузке системы?**  
   - `systemctl disable <имя_юнита>` — отключает юнит из автозапуска.  
   - Для проверки: `systemctl is-enabled <имя_юнита>` — если юнит отключён, будет показано *disabled*.  

3. **Какую команду вы должны использовать для отображения всех сервисных юнитов, которые в настоящее время загружены?**  
   - `systemctl list-units --type=service`  

4. **Как создать потребность (wants) в сервисе?**  
   - `systemctl enable <имя_сервиса>` — создаёт символическую ссылку в каталоге `wants/`.  
   - Пример: `systemctl enable vsftpd` — добавляет службу **vsftpd** в автозапуск.  

5. **Как переключить текущее состояние на цель восстановления (rescue target)?**  
   - `systemctl isolate rescue.target`  

6. **Поясните причину получения сообщения о том, что цель не может быть изолирована.**  
   - У некоторых целей в unit-файле отсутствует параметр `AllowIsolate=yes`.  
   - Если он не задан, systemd не разрешает перевод системы в изоляцию этой цели.  

7. **Вы хотите отключить службу systemd, но, прежде чем сделать это, вы хотите узнать, какие другие юниты зависят от этой службы. Какую команду вы бы использовали?**  
   - `systemctl list-dependencies <имя_юнита> --reverse`  
   - Она показывает все юниты, которые зависят от указанного.  


# Заключение  

В ходе работы были рассмотрены изолируемые цели и цели по умолчанию в системе **systemd**.  
Были изучены способы переключения режимов работы системы (rescue, multi-user, graphical, reboot), проверка доступных для изоляции целей, а также методы изменения цели, используемой при загрузке системы.  
Закреплены навыки управления целями systemd, что позволяет гибко настраивать режим работы операционной системы в зависимости от потребностей.  
