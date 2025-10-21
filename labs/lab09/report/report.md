---
## Front matter
title: "Отчёт по лабораторной работе №9"
subtitle: "Управление SELinux"
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

Получить навыки работы с контекстом безопасности и политиками SELinux.

# Отчёт по выполнению работы  

## Управление режимами SELinux  

1. В терминале были получены полномочия администратора с помощью команды **su -**.  

2. Для просмотра текущего состояния SELinux выполнена команда **sestatus -v**.  
   Вывод показал:  
   - **SELinux status: enabled** — SELinux включён.  
   - **SELinuxfs mount:** `/sys/fs/selinux` — точка монтирования файловой системы SELinux.  
   - **SELinux root directory:** `/etc/selinux` — каталог, где хранятся политики SELinux.  
   - **Loaded policy name:** `targeted` — используется целевая политика (защищаются только отдельные процессы).  
   - **Current mode:** `enforcing` — принудительный режим, в котором политика SELinux активно применяется.  
   - **Mode from config file:** `enforcing` — значение по умолчанию установлено в конфигурации.  
   - **Policy MLS status:** `enabled` — многоуровневая система безопасности (MLS) включена.  
   - **Policy deny_unknown status:** `allowed` — неизвестные объекты разрешены.  
   - **Memory protection checking:** `actual (secure)` — защита памяти активна.  
   - **Max kernel policy version:** `33` — версия политики ядра.  

   ![Вывод команды sestatus -v](Screenshot_1.png){ #fig:001 width=80% }

3. С помощью команды **getenforce** определён текущий режим SELinux — **Enforcing**.  

4. Командой **setenforce 0** режим был изменён на **Permissive**, что подтверждено повторным вызовом **getenforce**.  
   В этом режиме SELinux не блокирует действия, а только регистрирует возможные нарушения.  

   ![Переключение режима SELinux в Permissive](Screenshot_2.png){ #fig:002 width=80% }

5. В конфигурационном файле **/etc/sysconfig/selinux** параметр **SELINUX** изменён на значение **disabled**, что полностью отключает SELinux после перезагрузки системы.  

   ![Изменение файла /etc/sysconfig/selinux — отключение SELinux](Screenshot_3.png){ #fig:003 width=80% }

6. После перезагрузки системы выполнена команда **getenforce**, показавшая результат **Disabled**, что подтверждает успешное отключение SELinux.  

   ![SELinux отключён](Screenshot_4.png){ #fig:004 width=80% }

7. Попытка включить SELinux командой **setenforce 1** завершилась сообщением **“SELinux is disabled”**, что подтверждает невозможность переключения между режимами без перезагрузки.  

8. В файле **/etc/sysconfig/selinux** параметр был снова изменён на **SELINUX=enforcing**, после чего система перезагружена.  

   ![Включение режима enforcing в конфигурационном файле](Screenshot_5.png){ #fig:005 width=80% }

9. При загрузке системы появилось предупреждение **“SELinux targeted policy relabel is required”** — система автоматически перемаркировала файлы для восстановления корректных контекстов безопасности.  

   ![Автоматическое перемаркирование при загрузке системы](Screenshot_6.png){ #fig:006 width=80% }

10. После завершения загрузки команда **sestatus -v** подтвердила, что SELinux снова работает в режиме **Enforcing**.  

   ![SELinux включён и работает в режиме Enforcing](Screenshot_7.png){ #fig:007 width=80% }

## Восстановление контекста безопасности с помощью restorecon  

1. В терминале с полномочиями администратора просмотрен контекст безопасности файла **/etc/hosts** с помощью команды **ls -Z /etc/hosts**.  
   Тип контекста — **net_conf_t**.  

2. Файл **/etc/hosts** был скопирован в домашний каталог, где его контекст изменился на **admin_home_t**, так как копирование создаёт новый файл.  

3. После возврата файла в каталог **/etc** командой **mv ~/hosts /etc**, его контекст остался **admin_home_t**, что неверно.  

4. Для восстановления корректного контекста применена команда **restorecon -v /etc/hosts**, после чего контекст был успешно изменён обратно на **net_conf_t**.  

5. Для массового восстановления контекстов была создана специальная метка командой **touch /.autorelabel**.  
   После перезагрузки система автоматически перемаркировала файлы, что отразилось в загрузочных сообщениях.  

   ![Использование restorecon и автоматического перемаркирования](Screenshot_8.png){ #fig:008 width=80% }

## Настройка контекста безопасности для нестандартного расположения файлов веб-сервера  

1. В терминале были получены полномочия администратора и установлены необходимые пакеты httpd и lynx. Это обеспечило возможность запуска веб-сервера Apache и проверки его работы через текстовый браузер.  

2. Создан каталог /web, предназначенный для хранения файлов веб-сервера, и в нём создан файл index.html со строкой: Welcome to my web-server.  

3. В конфигурационном файле /etc/httpd/conf/httpd.conf была закомментирована стандартная строка DocumentRoot "/var/www/html" и добавлена новая строка DocumentRoot "/web". Также был изменён раздел Directory, определяющий политику доступа:  

AllowOverride None  
Require all granted  

Это разрешает доступ ко всем ресурсам каталога /web без ограничений.  

![Изменение конфигурации DocumentRoot и Directory](Screenshot_9.png){ #fig:009 width=80% }

4. После перезапуска службы httpd при обращении к веб-серверу через lynx http://localhost отобразилась стандартная страница теста Rocky Linux. Это свидетельствует о том, что SELinux заблокировал доступ к новому каталогу /web.  

![Стандартная тестовая страница Rocky Linux](Screenshot_10.png){ #fig:010 width=80% }

5. Для устранения этой проблемы новому каталогу был присвоен корректный контекст безопасности. Сначала добавлено новое правило контекста для каталога /web с типом httpd_sys_content_t, затем выполнено восстановление контекста. В результате каталогу /web и файлу index.html был назначен правильный тип безопасности, что позволило веб-серверу обращаться к содержимому.  

![Применение контекста безопасности для каталога /web](Screenshot_11.png){ #fig:011 width=80% }

6. После повторного обращения к серверу через браузер lynx страница успешно отобразила текст: Welcome to my web-server. Это подтверждает правильную настройку контекста безопасности.  

![Отображение пользовательской страницы веб-сервера](Screenshot_12.png){ #fig:012 width=80% }

## Работа с переключателями SELinux  

1. После получения прав администратора был выполнен просмотр всех переключателей SELinux, связанных со службой ftp. Из вывода видно, что параметр ftpd_anon_write имеет значение off, что означает запрет на анонимную запись.  

2. Для получения более подробной информации использовалась команда для вывода всех доступных переключателей ftpd_anon, что показало их текущее состояние и назначение — Allow ftpd to anon write.  

3. Значение переключателя ftpd_anon_write было изменено на on. После проверки состояние изменилось на активное, что означает включение временной настройки.  

4. Для сохранения изменения между перезагрузками параметр был установлен с постоянным флагом. Повторная проверка подтвердила, что ftpd_anon_write теперь активен как во временной, так и в постоянной конфигурации.  

![Проверка и изменение переключателей SELinux для ftpd_anon_write](Screenshot_13.png){ #fig:013 width=80% }

# Контрольные вопросы  

1. **Вы хотите временно поставить SELinux в разрешающем режиме. Какую команду вы используете?**  
   - `setenforce 0` — временно переводит SELinux в разрешающий режим (**Permissive**) без перезагрузки системы.  

2. **Вам нужен список всех доступных переключателей SELinux. Какую команду вы используете?**  
   - `getsebool -a` — отображает список всех доступных переключателей SELinux и их текущие состояния.  

3. **Каково имя пакета, который требуется установить для получения легко читаемых сообщений журнала SELinux в журнале аудита?**  
   - Пакет **setroubleshoot** — обеспечивает расшифровку и понятное представление сообщений SELinux в системных журналах.  

4. **Какие команды вам нужно выполнить, чтобы применить тип контекста httpd_sys_content_t к каталогу /web?**  
   - `semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"` — добавляет правило для каталога /web.  
   - `restorecon -R -v /web` — применяет новое правило и изменяет контекст безопасности каталога и его содержимого.  

5. **Какой файл вам нужно изменить, если вы хотите полностью отключить SELinux?**  
   - `/etc/sysconfig/selinux` — конфигурационный файл, где параметр **SELINUX** устанавливается в значение **disabled**.  

6. **Где SELinux регистрирует все свои сообщения?**  
   - Все сообщения SELinux записываются в файл **/var/log/audit/audit.log**.  

7. **Вы не знаете, какие типы контекстов доступны для службы ftp. Какая команда позволяет получить более конкретную информацию?**  
   - `semanage fcontext -l | grep ftp` — показывает все доступные типы контекстов, связанные со службой FTP.  

8. **Ваш сервис работает не так, как ожидалось, и вы хотите узнать, связано ли это с SELinux или чем-то ещё. Какой самый простой способ узнать?**  
   - `setenforce 0` — временно перевести SELinux в разрешающий режим (**Permissive**) и проверить, устранится ли проблема. Если сервис заработал, причина связана с политикой SELinux.  

# Заключение  

В ходе лабораторной работы были изучены принципы работы механизма SELinux, его режимы функционирования (**Enforcing**, **Permissive**, **Disabled**) и способы их изменения. На практике была выполнена настройка контекста безопасности для нестандартного каталога веб-сервера, что позволило обеспечить корректное взаимодействие Apache с системой безопасности.  