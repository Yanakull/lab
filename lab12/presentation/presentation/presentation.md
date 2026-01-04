---
## Front matter
lang: ru-RU
title: Лабораторная работа №12
subtitle: Настройки сети в Linux
author:
  - Руслан Алиев
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 02 ноября 2025

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

Получение навыков настройки сетевых параметров системы в Linux с использованием инструментов **ip**, **nmcli**, **nmtui** и **NetworkManager**.

# Ход выполнения работы

## Проверка конфигурации сети

![Информация о сетевых интерфейсах](Screenshot_1.png){ #fig:001 width=80% }

## Проверка подключения к Интернету

![Проверка соединения с Интернетом (ping)](Screenshot_2.png){ #fig:002 width=80% }

## Добавление дополнительного IP-адреса

![Добавление IP-адреса](Screenshot_3.png){ #fig:003 width=80% }

## Сравнение ip и ifconfig

![Вывод ifconfig](Screenshot_4.png){ #fig:004 width=80% }

## Просмотр активных портов

![Список портов TCP и UDP](Screenshot_5.png){ #fig:005 width=80% }

# Управление подключениями через nmcli

## Список подключений

![Список существующих подключений](Screenshot_6.png){ #fig:006 width=80% }

## Активация статического соединения

![Активация static и проверка IP](Screenshot_7.png){ #fig:007 width=80% }

## Переключение обратно на DHCP

![Активация dhcp-подключения](Screenshot_8.png){ #fig:008 width=80% }

## Настройка параметров профиля static

![Изменение параметров соединения](Screenshot_9.png){ #fig:009 width=80% }

## Редактирование в nmtui

![Настройки соединения static в nmtui](Screenshot_10.png){ #fig:010 width=80% }

## Профиль dhcp в nmtui

![Настройки dhcp в nmtui](Screenshot_11.png){ #fig:011 width=80% }

## Проверка в графическом интерфейсе

![Настройки static в GUI](Screenshot_12.png){ #fig:012 width=80% }

## Проверка в графическом интерфейсе

![Настройки dhcp в GUI](Screenshot_13.png){ #fig:013 width=80% }

# Итоги работы

## Вывод

В ходе лабораторной работы были освоены методы настройки сети в Linux.  
Рассмотрены способы конфигурирования сетевых параметров с помощью **ip**, **nmcli** и **nmtui**, а также управление профилями **NetworkManager**.  
Изучены процедуры добавления статических и динамических IP-адресов, настройки DNS и шлюзов, а также проверка состояния соединений и маршрутов.
