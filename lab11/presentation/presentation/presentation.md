---
## Front matter
lang: ru-RU
title: Лабораторная работа №11
subtitle: Управление загрузкой системы
author:
  - Руслан Алиев
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

Получить навыки работы с загрузчиком системы **GRUB2** и научиться управлять параметрами загрузки, а также выполнять восстановление доступа через консольные режимы.

# Ход выполнения работы

## Модификация параметров GRUB2

![Редактирование файла /etc/default/grub](Screenshot_1.png){ #fig:001 width=70% }

## Пересоздание конфигурации GRUB2

![Пересоздание конфигурации GRUB2](Screenshot_2.png){ #fig:002 width=70% }

## Меню загрузчика GRUB

![Меню загрузчика GRUB](Screenshot_3.png){ #fig:003 width=70% }

## Режим восстановления (rescue)

![Редактирование параметров загрузки — режим rescue](Screenshot_4.png){ #fig:004 width=70% }

## Режим восстановления (rescue)

![Список активных модулей и переменные среды в режиме rescue](Screenshot_5.png){ #fig:005 width=70% }

## Аварийный режим (emergency)

![Редактирование параметров загрузки — режим emergency](Screenshot_6.png){ #fig:006 width=70% }

## Аварийный режим (emergency)

![Список активных модулей в режиме emergency](Screenshot_7.png){ #fig:007 width=70% }

## Сброс пароля root

![Редактирование параметров загрузки для сброса пароля root](Screenshot_8.png){ #fig:008 width=70% }

## Сброс пароля root

![Работа в режиме initramfs при сбросе пароля root](Screenshot_9.png){ #fig:009 width=70% }

# Итоги работы

## Вывод

В ходе работы были изучены принципы настройки и модификации загрузчика **GRUB2** в Linux.  
Были выполнены действия по изменению конфигурационных параметров, пересозданию файла загрузки и проверке работы системы в режимах **rescue**, **emergency**, а также при сбросе пароля **root**.  
Полученные навыки позволяют администратору уверенно управлять процессом загрузки системы и восстанавливать доступ при сбоях.
