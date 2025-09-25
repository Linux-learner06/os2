---
## Front matter
lang: ru-RU
title: Лабораторная работа №4
subtitle: Работа с программными пакетами
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

Получение навыков работы с репозиториями и менеджерами пакетов в Linux.

# Ход выполнения работы

## Работа с репозиториями

- Проверка репозиториев в `/etc/yum.repos.d`
- Просмотр списка доступных репозиториев `dnf repolist`
- Поиск пакетов по ключевому слову `dnf search user`

![Список репозиториев](Screenshot_2.png){ #fig:001 width=70% }

## Установка и удаление пакета nmap

- Поиск и информация о пакете `dnf search nmap`, `dnf info nmap`
- Установка:
  - `dnf install nmap`
  - `dnf install nmap*`
- Удаление:
  - `dnf remove nmap`
  - `dnf remove nmap*`

![Установка пакета nmap](Screenshot_4.png){ #fig:002 width=70% }

## Работа с группами пакетов

- Просмотр групп: `dnf groups list`
- Информация: `dnf groups info "RPM Development Tools"`
- Установка: `dnf groupinstall "RPM Development Tools"`
- Удаление: `dnf groupremove "RPM Development Tools"`

![Установка группы пакетов](Screenshot_7.png){ #fig:003 width=70% }

## Использование rpm (lynx)

- Загрузка пакета: `dnf install lynx --downloadonly`
- Установка: `rpm -Uhv lynx.rpm`
- Проверка:
  - `which lynx`
  - `rpm -qi lynx`
  - `rpm -ql lynx`, `rpm -qd lynx`
- Удаление: `rpm -e lynx`

![Запуск lynx](Screenshot_15.png){ #fig:004 width=60% height=60% }

## Использование rpm (dnsmasq)

- Проверка:
  - `which dnsmasq`
  - `rpm -qi dnsmasq`
  - `rpm -qc dnsmasq`
  - `rpm -q --scripts dnsmasq`

![Документация dnsmasq](Screenshot_19.png){ #fig:005 width=60% height=60% }

# Итоги работы

## Вывод

- Освоены приёмы работы с пакетами в Linux:
  - поиск, установка и удаление пакетов с помощью **dnf**
  - работа с группами пакетов
  - управление rpm-пакетами вручную
- Получены практические навыки администрирования ПО в Linux
