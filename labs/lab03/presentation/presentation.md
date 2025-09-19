---
## Front matter
lang: ru-RU
title: Лабораторная работа №3
subtitle: Настройка прав доступа
author:
  - Эзиз Хатамов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 18 сентября 2025

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

Получение навыков настройки базовых, специальных и расширенных прав доступа в ОС Linux.

# Ход выполнения работы

## Управление базовыми разрешениями

- Проверка доступа под пользователем **bob**:
  - доступ в `/data/main` → YES
  - доступ в `/data/third` → NO

![Управление доступом к каталогам](Screenshot_1.png){ #fig:001 width=70% }

## Управление специальными разрешениями

- Установлены **setgid** и **sticky-bit** на `/data/main`
- Новые файлы наследуют группу **main**
- Удаление чужих файлов блокируется

![Управление специальными разрешениями](Screenshot_2.png){ #fig:002 width=70% }

## Управление расширенными разрешениями (ACL)

- Настроены ACL:
  - `g:third:rx` для **/data/main**
  - `g:main:rx` для **/data/third**
- Назначены ACL по умолчанию:
  - `/data/main` → `d:g:third:rwx`
  - `/data/third` → `d:g:main:rwx`

![ACL проверка](Screenshot_3.png){ #fig:003 width=70% }

## Проверка ACL и sticky-bit

- Под пользователем **carol** (группа third):
  - Удаление файлов в `/data/main` → NO
  - Запись в чужие файлы → NO
- Sticky-бит и маска ACL ограничивают доступ

![Проверка доступа пользователем carol](Screenshot_6.png){ #fig:004 width=70% }

# Итоги работы

## Вывод

- Освоена работа с правами доступа в Linux:
  - **Базовые права** (`chmod`)
  - **Специальные биты** (setgid, sticky-bit)
  - **ACL** для гибкого управления доступом
- Sticky-бит и ACL обеспечивают защиту файлов и корректное наследование прав
