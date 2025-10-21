---
## Front matter
lang: ru-RU
title: Лабораторная работа №8
subtitle: Планировщики событий
author:
  - Эзиз Хатамов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 13 октября 2025

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

Получение навыков работы с планировщиками заданий **cron** и **at** в операционной системе Linux.

# Ход выполнения работы

## Проверка службы cron

- Проверена работа службы **crond** — активна  
- Просмотрен файл **/etc/crontab**

![Проверка службы crond](Screenshot_1.png){ #fig:001 width=70% }

## Добавление задания cron

- Выполнение — каждая минута  
- Проверено через журнал `/var/log/messages`

![Добавление и проверка задания cron](Screenshot_2.png){ #fig:002 width=70% }

## Изменение расписания cron

- Запуск каждый час в рабочие дни  
- Проверка выполнения через журнал

![Изменённое расписание cron](Screenshot_4.png){ #fig:003 width=70% }

## Создание сценария eachhour

- В каталоге **/etc/cron.hourly** создан скрипт **eachhour**  
- Сделан исполняемым  
- Проверена запись в журнал

![Сценарий eachhour в /etc/cron.hourly](Screenshot_5.png){ #fig:004 width=70% }

## Добавление задания в /etc/cron.d

- Создан файл **eachhour** в **/etc/cron.d**  
- Выполнение каждые 11 минут часа от имени root

![Задание в /etc/cron.d](Screenshot_6.png){ #fig:005 width=70% }

# Планирование с помощью at

## Проверка службы atd

- Проверено состояние службы **atd** — активна  
- Создано однократное задание
- Проверено выполнение через **atq** и лог `/var/log/messages`

![Проверка выполнения задания at](Screenshot_7.png){ #fig:006 width=70% }

# Итоги работы

## Вывод

- Изучены планировщики **cron** и **at**  
- Освоены методы настройки периодических и разовых заданий  
- Получены навыки проверки выполнения через системные журналы  
- Изучена автоматизация системных процессов в Linux
