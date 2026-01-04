---
lang: ru-RU
title: Лабораторная работа №15
subtitle: Управление логическими томами (LVM)
author:
  - Руслан Алиев
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 25 ноября 2025

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

Освоение создания, настройки и управления логическими томами LVM в Linux.

# Ход выполнения работы

## Создание физического тома

![Создание раздела /dev/sdb1](Screenshot_1.png){width=70%}

## Создание LVM PV

![Физический том /dev/sdb1](Screenshot_2.png){width=70%}

## Создание логического тома lvdata

![Создание ext4 на LV](Screenshot_3.png){width=70%}

## Настройка fstab

![fstab запись для lvdata](Screenshot_4.png){width=70%}

## Проверка монтирования

![Монтирование /mnt/data](Screenshot_5.png){width=70%}

## Новый раздел /dev/sdb2

![Создание /dev/sdb2](Screenshot_6.png){width=70%}

## Расширение группы томов

![Расширение VG](Screenshot_7.png){width=70%}

## Увеличение lvdata

![Увеличение LV и ФС](Screenshot_8.png){width=70%}

## Уменьшение lvdata

![Итоговый размер тома](Screenshot_9.png){width=70%}

# Самостоятельная работа

## Создание разделов /dev/sdc1 и /dev/sdc2

![Разметка диска /dev/sdc](Screenshot_10.png){width=70%}

## Создание vggroup и lvgroup

![Создание LV и XFS](Screenshot_11.png){width=70%}

## Монтирование /mnt/groups

![fstab для /mnt/groups](Screenshot_12.png){width=70%}

## Проверка монтирования

![Монтирование /mnt/groups](Screenshot_13.png){width=70%}

## Добавление PV /dev/sdc2

![Добавление PV](Screenshot_14.png){width=70%}

## Увеличение lvgroup и файловой системы

![Расширение XFS](Screenshot_15.png){width=70%}

## Итоговое состояние

![Итоговый размер](Screenshot_16.png){width=70%}

# Итоги работы

## Вывод

Изучены основные механизмы LVM: создание PV, VG и LV, изменение размеров томов и файловых систем, а также автоматическое монтирование через **fstab**. Получены практические навыки администрирования дискового пространства.
