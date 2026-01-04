---
## Front matter
lang: ru-RU
title: Лабораторная работа №5
subtitle: Управление системными службами
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

Получить навыки управления системными службами операционной системы посредством **systemd**.

# Ход выполнения работы

## Управление сервисами

![Проверка статуса службы vsftpd](Screenshot_1.png){ #fig:001 width=70% }

## Управление сервисами

![Запуск и проверка статуса vsftpd](Screenshot_2.png){ #fig:002 width=70% }

## Управление сервисами

![Добавление службы vsftpd в автозапуск](Screenshot_3.png){ #fig:003 width=70% }

## Управление сервисами

![Отключение автозапуска vsftpd](Screenshot_4.png){ #fig:004 width=70% }

## Управление сервисами

![Символические ссылки сервисов](Screenshot_5.png){ #fig:005 width=70% }

## Управление сервисами

![Обратные зависимости vsftpd](Screenshot_6.png){ #fig:006 width=70% }

## Конфликты юнитов

![Установка iptables](Screenshot_7.png){ #fig:007 width=70% }

## Конфликты юнитов

![Статус firewalld и iptables](Screenshot_8.png){ #fig:008 width=70% }

## Конфликты юнитов

![Конфликт запуска firewalld и iptables](Screenshot_9.png){ #fig:009 width=70% }

## Конфликты юнитов

![Конфликты в firewalld.service](Screenshot_10.png){ #fig:010 width=70% }

## Конфликты юнитов

![Содержимое iptables.service](Screenshot_11.png){ #fig:011 width=70% }

## Конфликты юнитов

![Маскирование iptables](Screenshot_12.png){ #fig:012 width=70% }

## Изолируемые цели

![Список изолируемых целей](Screenshot_13.png){ #fig:013 width=70% }

## Изолируемые цели

![Перевод системы в rescue.target](Screenshot_14.png){ #fig:014 width=70% }

## Цель по умолчанию

![Текущая цель по умолчанию](Screenshot_15.png){ #fig:015 width=70% }

## Цель по умолчанию

![Возврат графического режима](Screenshot_16.png){ #fig:016 width=70% }

# Итоги работы

## Вывод

В ходе работы были изучены основные приёмы управления службами и целями в **systemd**.  
Было рассмотрено, как запускать и останавливать сервисы, включать и отключать их из автозагрузки, а также анализировать зависимости между юнитами.  
Особое внимание уделялось работе с изолируемыми целями и настройке цели по умолчанию при загрузке системы.  
Кроме того, были исследованы конфликты юнитов (например, между `firewalld` и `iptables`) и способы их разрешения.  
