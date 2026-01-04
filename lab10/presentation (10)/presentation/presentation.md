---
## Front matter
lang: ru-RU
title: Лабораторная работа №10
subtitle: Основы работы с модулями ядра операционной системы
author:
  - Руслан Алиев
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 23 октября 2025

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

Получить навыки работы с утилитами управления модулями ядра операционной системы Linux.

# Ход выполнения работы

## Управление модулями ядра

![Список устройств и модулей ядра](Screenshot_1.png){ #fig:001 width=70% }

## Управление модулями ядра

![Список загруженных модулей ядра](Screenshot_2.png){ #fig:002 width=70% }

## Проверка и загрузка ext4

![Загрузка модуля ext4](Screenshot_3.png){ #fig:003 width=70% }

## Информация о модуле ext4

![Информация о модуле ext4](Screenshot_4.png){ #fig:004 width=70% }

## Выгрузка модулей ext4 и xfs

![Выгрузка модулей ext4 и xfs](Screenshot_5.png){ #fig:005 width=70% }

## Проверка и загрузка bluetooth

![Загрузка модуля bluetooth](Screenshot_6.png){ #fig:006 width=70% }

## Информация о модуле bluetooth

![Информация о модуле bluetooth](Screenshot_7.png){ #fig:007 width=70% }

## Выгрузка bluetooth

![Выгрузка модуля bluetooth](Screenshot_8.png){ #fig:008 width=70% }

## Проверка версии ядра

![Просмотр версии ядра и списка пакетов kernel](Screenshot_9.png){ #fig:009 width=70% }

## Обновление системы и ядра

![Обновление](Screenshot_8.png){ #fig:010 width=70% }

## Проверка после перезагрузки

![Информация о системе и ядре после обновления](Screenshot_9.png){ #fig:011 width=70% }

# Итоги работы

## Вывод

В ходе работы были изучены основные приёмы управления модулями ядра операционной системы Linux.  
Были рассмотрены команды для просмотра списка устройств, загруженных модулей и их параметров, а также способы их загрузки и выгрузки.  
Также выполнено обновление ядра системы и проверена работа новой версии.  
Получены практические навыки администрирования ядра Linux.
