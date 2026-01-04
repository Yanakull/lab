---
## Front matter
lang: ru-RU
title: Лабораторная работа №4
subtitle: Работа с программными пакетами
author:
  - Руслан Алиев
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 15 декабря 2025

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

Получение практических навыков работы с репозиториями  
и менеджерами пакетов **dnf** и **rpm**  
в операционной системе Linux.

# Ход выполнения работы

## Переход в режим суперпользователя

![Каталог /etc/yum.repos.d и файл rocky.repo](Screenshot_1.png){ width=70% }

## Просмотр подключённых репозиториев

![Список репозиториев](Screenshot_2.png){ width=70% }

## Поиск пакетов

![Результат поиска пакетов user](Screenshot_3.png){ width=70% }

## Работа с пакетом nmap

![Информация о пакете nmap](Screenshot_4.png){ width=70% }

## Установка и удаление nmap

![Установка пакета nmap](Screenshot_5.png){ width=70% }

## Группы пакетов

![Список групп пакетов](Screenshot_7.png){ width=70% }

## Установка и удаление группы

![Установка группы RPM Development Tools](Screenshot_8.png){ width=70% }

## История dnf

![История использования dnf](Screenshot_9.png){ width=70% }

## Работа с пакетом lynx

![Загрузка пакета lynx](Screenshot_10.png){ width=70% }

## Анализ пакета lynx

![Документация lynx](Screenshot_14.png){ width=70% }

## Работа с пакетом dnsmasq

![Пакет dnsmasq](Screenshot_17.png){ width=70% }

## Анализ dnsmasq

![Файлы пакета dnsmasq](Screenshot_19.png){ width=70% }

# Итоги работы

## Вывод

В ходе лабораторной работы были изучены:
- принципы работы с репозиториями;
- установка и удаление пакетов и групп пакетов;
- анализ rpm-пакетов, их файлов, документации и скриптов.

Полученные навыки позволяют эффективно администрировать программное обеспечение  
в Linux-системах.
