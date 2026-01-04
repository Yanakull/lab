---
## Front matter
lang: ru-RU
title: Лабораторная работа №9
subtitle: Управление SELinux
author:
  - Руслан Алиев
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 16 октября 2025

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

Получить навыки работы с контекстом безопасности и политиками **SELinux** в Linux.

# Ход выполнения работы

## Проверка состояния SELinux

![Вывод команды sestatus -v](Screenshot_1.png){ #fig:001 width=70% }

## Изменение режима работы SELinux

![Изменение режима SELinux на Permissive](Screenshot_2.png){ #fig:002 width=70% }

## Отключение SELinux

![Отключение SELinux в файле конфигурации](Screenshot_3.png){ #fig:003 width=70% }

## Ошибка при попытке включения после отключения

![Попытка изменить режим при отключённом SELinux](Screenshot_4.png){ #fig:004 width=70% }

## Повторное включение SELinux

![Включение SELinux в конфигурации](Screenshot_5.png){ #fig:005 width=70% }

## Восстановление меток SELinux

![Автоматическое восстановление меток SELinux](Screenshot_6.png){ #fig:006 width=70% }

## Проверка работы после перезапуска

![Проверка состояния SELinux после включения](Screenshot_7.png){ #fig:007 width=70% }

## Просмотр и изменение контекста файла

![Автоматическое восстановление контекста безопасности при загрузке](Screenshot_8.png){ #fig:008 width=70% }

## Изменение конфигурации Apache

![Изменение файла конфигурации Apache](Screenshot_9.png){ #fig:009 width=70% }

## Тестовая страница Apache по умолчанию

![Стандартная тестовая страница Apache](Screenshot_10.png){ #fig:010 width=70% }

## Применение контекста httpd_sys_content_t

![Применение нового контекста безопасности к каталогу /web](Screenshot_11.png){ #fig:011 width=70% }

## Отображение пользовательской страницы

![Отображение пользовательской страницы веб-сервера](Screenshot_12.png){ #fig:012 width=70% }

## Проверка и изменение состояния ftpd_anon_write

![Просмотр и изменение переключателя ftpd_anon_write](Screenshot_13.png){ #fig:013 width=70% }

# Итоги работы

## Вывод

В ходе лабораторной работы были изучены режимы работы и механизмы **SELinux**, методы настройки контекстов безопасности, восстановления меток, а также принципы взаимодействия SELinux с веб- и FTP-службами.  
Получены практические навыки администрирования системы безопасности SELinux в Linux.
