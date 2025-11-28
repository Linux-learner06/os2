---
## Front matter
lang: ru-RU
title: Лабораторная работа №13
subtitle: Фильтр пакетов (firewalld)
author:
  - Эзиз Хатамов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 07 ноября 2025
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

Получение практических навыков настройки брандмауэра Linux с использованием `firewall-cmd` и `firewall-config`.

# Ход выполнения работы

## Определение параметров брандмауэра

- Определена зона по умолчанию → **public**
- Просмотрены доступные зоны и список доступных сервисов

![Список зон и сервисов](Screenshot_1.png){ #fig:001 width=70% }

## Проверка активных сервисов

- Просмотрены сервисы, разрешённые в зоне `public`
- Сравнение `--list-all` и `--list-all --zone=public`
- Зона `public` является зоной по умолчанию

![Вывод конфигурации зоны](Screenshot_2.png){ #fig:002 width=70% }

## Добавление сервиса VNC (runtime)

- Добавлен сервис: `vnc-server`
- Проверено наличие сервиса — отображается

![Добавление сервиса VNC](Screenshot_3.png){ #fig:003 width=70% }

## Добавление сервиса VNC (permanent)

- Добавлен как постоянный (`--permanent`)
- Для применения настроек выполнен `firewall-cmd --reload`
- Сервис появился в списке активных

![Добавление permanent + reload](Screenshot_4.png){ #fig:004 width=70% }

## Используем графический интерфейс

- Запущено приложение `firewall-config`
- Выбрана конфигурация **Permanent**
- В зоне `public` включены сервисы: `http`, `https`, `ftp`

![GUI: выбор сервисов](Screenshot_5.png){ #fig:005 width=70% }

## Добавление порта через GUI

- На вкладке *Ports* добавлен: `2022/udp`
- После `firewall-cmd --reload` порт стал активен

![GUI: добавление порта](Screenshot_6.png){ #fig:006 width=55% }

## Добавление сервисов доступа

- Добавлено:
  - `telnet` — через командную строку
  - `imap`, `pop3`, `smtp` — через GUI

![GUI: SMTP/IMAP/POP3](Screenshot_8.png){ #fig:007 width=70% }

## Все службы добавлены корректно

- Конфигурация стала постоянной

![Финальная конфигурация](Screenshot_9.png){ #fig:008 width=70% }

# Вывод

## Итоги выполнения работы

- Изучены команды `firewall-cmd` и принципы runtime/permanent конфигураций
- Освоено добавление сервисов и портов (CLI + GUI)
- Получены навыки управления зонами и сетевыми разрешениями в Linux

