---
lang: ru-RU
title: Лабораторная работа №2
subtitle: Управление пользователями и группами
author:
  - Руслан Алиев
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 15 декабря 2025

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

Получение практических навыков управления учётными записями пользователей и групп  
в операционной системе Linux.

# Ход выполнения работы

## Определение текущего пользователя


![Определение текущего пользователя](Screenshot_1.png){ width=70% }

## Переключение на root

![Работа под пользователем root](Screenshot_1.png){ width=70% }

## Просмотр конфигурации sudo

![Файл /etc/sudoers](Screenshot_2.png){ width=70% }

## Пользователь alice

![Создание пользователя alice](Screenshot_3.png){ width=70% }

## Пользователь bob

![Создание пользователя bob](Screenshot_4.png){ width=70% }

## Файл login.defs

![Редактирование login.defs](Screenshot_5.png){ width=70% }

## Каталог /etc/skel

![Каталог /etc/skel](Screenshot_6.png){ width=70% }

## Создание и проверка

![Домашний каталог carol](Screenshot_7.png){ width=70% }

## Управление паролем

![Параметры пароля carol](Screenshot_8.png){ width=70% }

## Создание групп

![Проверка групп](Screenshot_9.png){ width=70% }

# Заключение

## Вывод

В ходе лабораторной работы были получены практические навыки администрирования пользователей и групп в Linux.  
Освоены механизмы разграничения прав доступа, настройки sudo, управления паролями и группами, что является основой администрирования многопользовательских систем.
