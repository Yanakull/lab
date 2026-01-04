---
## Front matter
title: "Отчёт по лабораторной работе №13"
subtitle: "Фильтр пакетов"
author: "Руслан Алиев"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true
toc-depth: 2
lof: true
lot: true
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
    - spelling=modern
    - babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы

Получить навыки настройки пакетного фильтра в Linux.

## Настройка брандмауэра с помощью firewall-cmd

1. Для начала выполнен переход в режим суперпользователя.  
   Определена зона по умолчанию, которая оказалась `public`.

   ![Получение зоны по умолчанию](Screenshot_1.png){ #fig:001 width=80% }

2. Команда получения доступных зон вывела перечень: `block`, `dmz`, `external`, `home`, `internal`, `nm-shared`, `public`, `trusted`, `work`.

3. Просмотр списка доступных служб показал большое количество доступных сервисов, включая `ssh`, `http`, `ftp`, `cockpit`, `vnc-server` и другие.

4. В текущей активной зоне отображены службы `cockpit`, `dhcpv6-client`, `ssh`.

5. При сравнении команд `--list-all` и `--list-all --zone=public` вывод оказался одинаковым, так как `public` — зона по умолчанию.

   ![Параметры зоны public](Screenshot_2.png){ #fig:002 width=80% }

6. Временное добавление службы `vnc-server` позволило ей отобразиться в активной конфигурации зоны.

7. После перезапуска службы firewalld внесённые изменения исчезли. Это произошло потому, что добавление было временным и не сохранялось в конфигурации.

   ![Добавление и исчезновение службы vnc-server](Screenshot_3.png){ #fig:003 width=80% }

8. Повторное добавление `vnc-server` с ключом `--permanent` сохранило изменения в постоянной конфигурации. Однако, в конфигурации времени выполнения они пока не отразились.

9. После перезагрузки конфигурации командой `--reload` служба `vnc-server` появилась в активной конфигурации.

   ![Добавление vnc-server на постоянной основе](Screenshot_4.png){ #fig:004 width=80% }

10. В конфигурацию добавлен порт `2022/tcp` с постоянной фиксацией изменений и последующей перезагрузкой.  
    Порт отобразился в списке как `2022/tcp`.

    ![Добавление порта 2022/tcp](Screenshot_5.png){ #fig:005 width=80% }

## Использование графического интерфейса firewall-config

1. В GUI-интерфейсе firewall-config была выбрана конфигурация Permanent.  
   В зоне `public` активированы службы `ftp`, `http`, `https`.

   ![Включение служб в графическом интерфейсе](Screenshot_6.png){ #fig:006 width=80% }

2. На вкладке **Ports** добавлены порты `2022/tcp` и `2022/udp`.

   ![Добавление портов 2022/tcp и 2022/udp](Screenshot_7.png){ #fig:007 width=80% }

3. После перезагрузки конфигурации все внесённые изменения вступили в силу и отобразились в списке:  
   службы — `ftp`, `http`, `https`, порты — `2022/tcp`, `2022/udp`.

   ![Применение всех изменений](Screenshot_8.png){ #fig:008 width=80% }

## Добавление служб в рамках самостоятельной работы

1. Служба `telnet` была добавлена через CLI на постоянной основе.  
   После перезагрузки firewalld она появилась в конфигурации.

2. Через графический интерфейс были включены службы `imap`, `pop3`, `smtp`.

   ![Итоговая конфигурация зоны public](Screenshot_9.png){ #fig:009 width=80% }


# Контрольные вопросы

1. **Какая служба должна быть запущена перед началом работы с менеджером конфигурации брандмауэра firewall-config?**  
   firewalld

2. **Какая команда позволяет добавить UDP-порт 2355 в конфигурацию брандмауэра в зоне по умолчанию?**  
   firewall-cmd --add-port=2355/udp --permanent

3. **Какая команда позволяет показать всю конфигурацию брандмауэра во всех зонах?**  
   firewall-cmd --list-all-zones

4. **Какая команда позволяет удалить службу vnc-server из текущей конфигурации брандмауэра?**  
   firewall-cmd --remove-service=vnc-server

5. **Какая команда firewall-cmd позволяет активировать новую конфигурацию, добавленную опцией --permanent?**  
   firewall-cmd --reload

6. **Какой параметр firewall-cmd позволяет проверить, что новая конфигурация была добавлена в текущую зону и теперь активна?**  
   firewall-cmd --list-all

7. **Какая команда позволяет добавить интерфейс eno1 в зону public?**  
   firewall-cmd --zone=public --change-interface=eno1 --permanent

8. **Если добавить новый интерфейс в конфигурацию брандмауэра, пока не указана зона, в какую зону он будет добавлен?**  
   В зону по умолчанию (default zone), чаще всего — public

# Заключение

В рамках выполненной работы была изучена система управления сетевыми подключениями с использованием службы **firewalld** и утилиты **firewall-cmd**.  
Рассматривались текущие параметры зон, активные службы и способы изменения конфигурации брандмауэра как временно, так и на постоянной основе.  
Были отработаны команды добавления и удаления служб и портов, перезагрузка конфигурации и работа с интерфейсами.  
