---
## Front matter
lang: ru-RU
title: Лабораторная работа №9
subtitle: Управление SELinux
author:
  - Эзиз Хатамов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 16 октября 2025

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

Получить навыки работы с контекстом безопасности и политиками SELinux.

# Ход выполнения работы

## Управление режимами SELinux

![Проверка состояния SELinux](Screenshot_1.png){ #fig:001 width=70% }

## Восстановление контекста безопасности

- Копирование файла изменяет контекст на **admin_home_t**
- Восстановление корректного типа:
  - `restorecon -v /etc/hosts`

![Восстановление контекста безопасности](Screenshot_8.png){ #fig:002 width=70% }

## Настройка контекста безопасности веб-сервера

- В конфигурации Apache изменён **DocumentRoot** на `/web`
- Применён контекст **httpd_sys_content_t**:

![Работа пользовательского веб-сервера](Screenshot_12.png){ #fig:003 width=70% }

## Работа с переключателями SELinux

- Изменение параметра **ftpd_anon_write** с **off** на **on**

![Настройка переключателя ftpd_anon_write](Screenshot_13.png){ #fig:004 width=70% }

# Итоги работы

## Вывод

- Изучены принципы функционирования SELinux и его режимы (**Enforcing**, **Permissive**, **Disabled**)
- Выполнена настройка контекста безопасности для нестандартного каталога веб-сервера
- Изучены инструменты **semanage**, **restorecon**, **setsebool** и **getsebool**
- Освоены методы управления контекстами и политиками безопасности в Linux
