---
## Front matter
lang: ru-RU
title: Лабораторная работа №11
subtitle: Управление загрузкой системы
author:
  - Эзиз Хатамов
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 27 октября 2025

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

Получить навыки работы с загрузчиком системы **GRUB2**  
и научиться восстанавливать систему при сбое загрузки.

# Ход выполнения работы

## Модификация параметров GRUB2

- В файле `/etc/default/grub` установлен параметр  
  **GRUB_TIMEOUT=10**
- Сохранены изменения и обновлён конфигурационный файл  
  `grub2-mkconfig > /boot/grub2/grub.cfg`

![Редактирование GRUB](Screenshot_1.png){ #fig:001 width=70% }

## Проверка работы GRUB

- После перезагрузки появилось меню загрузчика GRUB

![Меню загрузчика GRUB](Screenshot_3.png){ #fig:002 width=70% }

## Режим восстановления (rescue mode)

- Добавлен параметр **systemd.unit=rescue.target**
- После загрузки просмотрены активные модули  

![Режим восстановления](Screenshot_4.png){ #fig:003 width=70% }

## Режим экстренной загрузки (emergency mode)

- В конец строки ядра добавлен параметр **systemd.unit=emergency.target**
- После загрузки в emergency-режим список модулей минимален

![Emergency-режим](Screenshot_6.png){ #fig:004 width=70% }

## Сброс пароля root

- Загрузка остановлена на этапе initramfs
- Попытка смены пароля не удалась из-за ограничений окружения

![Сброс пароля root](Screenshot_8.png){ #fig:005 width=70% }

# Итоги работы

## Вывод

- Изучены принципы настройки и модификации параметров **GRUB2**  
- Отработаны навыки загрузки системы в **rescue** и **emergency** режимах  
- Освоены методы восстановления доступа при утере пароля **root**  
- Получены практические навыки администрирования загрузочного процесса Linux
