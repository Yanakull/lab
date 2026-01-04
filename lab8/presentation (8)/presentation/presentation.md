---
## Front matter
lang: ru-RU
title: Лабораторная работа №8
subtitle: Планировщики событий cron и at
author:
  - Руслан Алиев
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 11 октября 2025

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

Получение навыков работы с планировщиками событий **cron** и **at** в операционной системе Linux.

# Ход выполнения работы

## Проверка службы cron

![Статус службы crond](Screenshot_1.png){ #fig:001 width=70% }

## Изучение файла /etc/crontab

![Содержимое файла /etc/crontab](Screenshot_2.png){ #fig:002 width=70% }

## Добавление задания cron

![Создание задания в crontab](Screenshot_3.png){ #fig:003 width=70% }

## Проверка выполнения задания

![Результат выполнения задания cron](Screenshot_4.png){ #fig:004 width=70% }

## Изменение расписания

![Изменённая запись в crontab](Screenshot_5.png){ #fig:005 width=70% }

## Создание сценария eachhour

![Создание сценария eachhour](Screenshot_6.png){ #fig:006 width=70% }

## Планирование через /etc/cron.d

![Файл расписания в /etc/cron.d](Screenshot_7.png){ #fig:007 width=70% }

## Проверка службы atd

![Статус службы atd](Screenshot_8.png){ #fig:008 width=70% }

## Планирование задачи at

![Результат выполнения задания at](Screenshot_9.png){ #fig:009 width=70% }

# Итоги работы

## Вывод

В ходе работы были изучены механизмы планирования задач в Linux с использованием **cron** и **at**, рассмотрены примеры периодических и одноразовых запусков, а также способы управления расписаниями для разных пользователей.
