---
## Front matter
lang: ru-RU
title: Лабораторная работа №7
subtitle: Управление журналами событий в системе
author:
  - Эзиз Хатамов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 4 октября 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы

## Основная цель

Получить навыки работы с журналами мониторинга различных событий в системе Linux и изучить инструменты **rsyslog** и **systemd-journald**.

# Ход выполнения работы

## Мониторинг системных событий

- Запущен мониторинг событий в реальном времени с помощью:
- Проверена фиксация событий при ошибках авторизации и при вводе `logger hello`
- Анализ журнала `/var/log/secure` подтвердил регистрацию всех действий

![Мониторинг системных событий](Screenshot_1.png){ #fig:001 width=70% }

## Настройка правил rsyslog.conf

- Установлен и запущен веб-сервер **Apache**
- Добавлена строка `ErrorLog syslog:local1` в конфигурацию httpd
- Проверена регистрация сообщений

![Настройка правил rsyslog](Screenshot_8.png){ #fig:002 width=70% }

## Работа с journalctl

  - `journalctl` — общий вывод
  - `journalctl -f` — реальное время
  - `journalctl _UID=0` — фильтрация по пользователю root
  - `journalctl -p err` — вывод ошибок
  - `--since yesterday` — фильтрация по дате
  - `-o verbose` — детальный вывод записей

## Работа с journalctl

![Использование journalctl](Screenshot_15.png){ #fig:003 width=70% }

## Постоянный журнал journald

- Создан каталог `/var/log/journal`
- Установлены права:
  - `chown root:systemd-journal /var/log/journal`
  - `chmod 2755 /var/log/journal`

## Постоянный журнал journald

![Постоянный журнал journald](Screenshot_20.png){ #fig:004 width=70% }

# Итоги работы

## Вывод

В ходе выполнения работы были изучены механизмы системного логирования Linux, принципы настройки **rsyslog** и **journald**, а также использование утилиты **journalctl** для анализа событий.  
Полученные навыки позволяют администрировать систему, отслеживать события и выявлять ошибки в работе служб.
