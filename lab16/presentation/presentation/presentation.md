---
## Front matter
lang: ru-RU
title: Лабораторная работа №16
subtitle: Программный RAID (mdadm)
author:
  - Руслан Алиев
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 06 декабря 2025

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

Освоить работу с RAID-массивами при помощи утилиты **mdadm** в Linux.

# Создание RAID 1

## Проверка дисков

![Проверка дисков /dev/sdd, /dev/sde, /dev/sdf](Screenshot_1.png){ width=70% }

## Создание разделов

![Создание разделов](Screenshot_2.png){ width=70% }

## Изменение типа разделов на RAID

![Типы разделов RAID](Screenshot_3.png){ width=70% }

## Состояние дисков

![Состояние дисков](Screenshot_4.png){ width=70% }

## Создание RAID 1

![Создание RAID1](Screenshot_5.png){ width=70% }

## Информация о массиве

![Информация о RAID](Screenshot_6.png){ width=70% }

## Создание файловой системы

![Создание файловой системы ext4](Screenshot_7.png){ width=70% }

## Монтирование RAID

![fstab и монтирование](Screenshot_8.png){ width=70% }

## Сбой и замена диска

![Сбой диска и добавление нового](Screenshot_9.png){ width=70% }

# RAID 1 с диском горячего резерва

## Создание массива RAID1

![Создание RAID1](Screenshot_11.png){ width=70% }

## Добавление hot spare

![Добавление hot spare](Screenshot_12.png){ width=70% }

## Сбой диска

![Сбой диска](Screenshot_13.png){ width=70% }

# Преобразование RAID1 → RAID5

## RAID1 перед конверсией

![Состояние RAID1](Screenshot_15.png){ width=70% }

## Переход к RAID5

![Изменение уровня RAID](Screenshot_16.png){ width=70% }

## RAID5 с двумя активными дисками

![RAID5 с hot spare](Screenshot_16.png){ width=70% }

## Добавление третьего диска

![RAID5 после расширения](Screenshot_17.png){ width=70% }

# Заключение

## Основной вывод

Изучены методы создания, контроля и преобразования программных RAID-массивов средствами **mdadm**, включая работу с зеркалированием, горячим резервом и переходом на RAID5. Получены практические навыки настройки отказоустойчивых хранилищ.
