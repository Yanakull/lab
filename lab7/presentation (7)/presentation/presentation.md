---
## Front matter
lang: ru-RU
title: Лабораторная работа №7
subtitle: Управление журналами событий в системе
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

Получить навыки работы с журналами мониторинга различных событий в системе.

# Ход выполнения работы

## Мониторинг системных событий

![Мониторинг /var/log/messages](Screenshot_1.png){ #fig:001 width=70% }

## Ошибка авторизации su

![FAILED SU (to root)](Screenshot_2.png){ #fig:002 width=70% }

## Сообщения через logger

![Сообщение logger hello](Screenshot_3.png){ #fig:003 width=70% }

## Журнал secure

![Журнал /var/log/secure](Screenshot_4.png){ #fig:004 width=70% }

## Настройка rsyslog (Apache)

![Установка и запуск httpd](Screenshot_5.png){ #fig:005 width=70% }

## Журнал ошибок Apache

![Журнал ошибок httpd](Screenshot_6.png){ #fig:006 width=70% }

## Перенаправление логов Apache

![ErrorLog syslog:local1](Screenshot_7.png){ #fig:007 width=70% }

## Конфигурация rsyslog для Apache

![Файл /etc/rsyslog.d/httpd.conf](Screenshot_8.png){ #fig:008 width=70% }

## Отладочные сообщения

![debug.conf для rsyslog](Screenshot_9.png){ #fig:009 width=70% }

## Отладка через logger

![Daemon Debug Message](Screenshot_10.png){ #fig:010 width=70% }

## Использование journalctl

![Журнал с момента загрузки](Screenshot_11.png){ #fig:011 width=70% }

## Journalctl без пейджера

![--no-pager](Screenshot_12.png){ #fig:012 width=70% }

## Journalctl -f (реальное время)

![Режим реального времени](Screenshot_13.png){ #fig:013 width=70% }

## Фильтрация journalctl

![Параметры фильтрации](Screenshot_14.png){ #fig:014 width=70% }

## Фильтрация по UID=0

![UID=0](Screenshot_15.png){ #fig:015 width=70% }

## Последние строки журнала

![journalctl -n 20](Screenshot_16.png){ #fig:016 width=70% }

## Ошибки journalctl

![journalctl -p err](Screenshot_17.png){ #fig:017 width=70% }

## Журнал со вчерашнего дня

![--since yesterday](Screenshot_18.png){ #fig:018 width=70% }

## Ошибки со вчерашнего дня

![journalctl --since yesterday -p err](Screenshot_19.png){ #fig:019 width=70% }

## Подробный вывод journalctl

![journalctl -o verbose](Screenshot_20.png){ #fig:020 width=70% }


## Постоянный журнал journald

![Создание постоянного журнала](Screenshot_21.png){ #fig:021 width=70% }

# Итоги работы

## Вывод

В ходе работы были изучены возможности администрирования журналов в Linux.  
Рассмотрены способы просмотра системных сообщений с использованием **rsyslog** и **systemd-journald**, а также настройка постоянного хранения логов и фильтрация событий.
