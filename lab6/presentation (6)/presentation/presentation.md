---
## Front matter
lang: ru-RU
title: Лабораторная работа №6
subtitle: Управление процессами
author:
  - Руслан Алиев
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 3 октября 2025

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

Получение навыков управления заданиями и процессами в Linux.

# Ход выполнения работы

## Управление заданиями

![Запуск заданий](Screenshot_1.png){ #fig:001 width=70% }

## Управление заданиями

![Работа команды dd в top](Screenshot_2.png){ #fig:002 width=70% }

## Управление заданиями

![Завершение процесса dd в top](Screenshot_3.png){ #fig:003 width=70% }

## Управление процессами

![Запуск процессов dd](Screenshot_4.png){ #fig:004 width=70% }

## Управление процессами

![Просмотр иерархии процессов](Screenshot_5.png){ #fig:005 width=70% }

## Задание 1

![Запуск процессов dd](Screenshot_6.png){ #fig:006 width=70% }

## Задание 2

![Работа с процессами yes](Screenshot_7.png){ #fig:007 width=70% }

## Задание 2

![Проверка статуса заданий](Screenshot_8.png){ #fig:008 width=70% }

## Задание 2

![Запуск yes с nohup](Screenshot_9.png){ #fig:009 width=70% }

## Задание 2

![Работа yes в top](Screenshot_10.png){ #fig:010 width=70% }

## Задание 2

![Сравнение приоритетов процессов yes](Screenshot_11.png){ #fig:011 width=70% }

# Итоги работы

## Вывод

В ходе работы были изучены основные способы управления заданиями и процессами в Linux.  
Рассмотрены приёмы запуска процессов на переднем и фоновом режимах, их приостановка, возобновление и завершение.  
Особое внимание уделялось использованию команд **jobs**, **fg**, **bg**, **kill**, а также инструментов **nice** и **renice** для управления приоритетами.  
Дополнительно была изучена работа с утилитой **top** для мониторинга процессов и их завершения.  
