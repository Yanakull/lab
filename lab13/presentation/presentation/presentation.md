---
## Front matter
lang: ru-RU
title: Лабораторная работа №13
subtitle: Фильтр пакетов (firewalld)
author:
  - Руслан Алиев
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 7 ноября 2025

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

Получить навыки настройки фильтрации пакетов и межсетевого экрана с помощью **firewalld** и утилиты **firewall-cmd**.

# Ход выполнения работы

## Получение зоны по умолчанию

![Получение зоны по умолчанию](Screenshot_1.png){ #fig:001 width=70% }

## Просмотр зон и служб

![Параметры зоны public](Screenshot_2.png){ #fig:002 width=70% }

## Добавление vnc-server (временно)

![Добавление и исчезновение службы vnc-server](Screenshot_3.png){ #fig:003 width=70% }

## Добавление vnc-server (постоянно)

![Добавление vnc-server на постоянной основе](Screenshot_4.png){ #fig:004 width=70% }

## Добавление порта 2022/tcp

![Добавление порта 2022/tcp](Screenshot_5.png){ #fig:005 width=70% }

## Firewall-config: службы

![Включение служб в графическом интерфейсе](Screenshot_6.png){ #fig:006 width=70% }

## Firewall-config: порты

![Добавление портов 2022/tcp и 2022/udp](Screenshot_7.png){ #fig:007 width=70% }

## Применение изменений

![Применение всех изменений](Screenshot_8.png){ #fig:008 width=70% }

## Итоговая конфигурация зоны

![Итоговая конфигурация зоны public](Screenshot_9.png){ #fig:009 width=70% }

# Итоги работы

## Вывод

В ходе лабораторной работы были изучены механизмы настройки брандмауэра в Linux с помощью **firewalld** и **firewall-cmd**.  
Проверены методы управления службами и портами, а также работа с зонами безопасности как в терминале, так и через GUI-интерфейс **firewall-config**.  
Полученные навыки важны для обеспечения базовой сетевой безопасности в Linux-средах.
