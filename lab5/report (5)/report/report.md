---
## Front matter
title: "Отчёт по лабораторной работе №5"
subtitle: "Управление системными службами"
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

Получить навыки управления системными службами операционной системы посредством systemd.

# Выполнение

## Управление сервисами

1. Сначала был получен доступ администратора с помощью команды **su -**.  
   Проверка статуса службы **vsftpd** показала, что она отсутствует в системе, так как пакет ещё не установлен.  

   ![Проверка статуса службы vsftpd до установки](Screenshot_1.png){ #fig:001 width=80% }

2. Для установки службы **Very Secure FTP (vsftpd)** использовалась команда **dnf -y install vsftpd**.  
   После загрузки и распаковки пакета служба стала доступна в системе.  

3. Для запуска службы применялась команда **systemctl start vsftpd**.  
   Повторная проверка статуса показала, что сервис находится в состоянии *active (running)*, но при этом автозапуск отключён (`disabled`).  

   ![Запуск и проверка статуса vsftpd](Screenshot_2.png){ #fig:002 width=80% }

4. Для добавления службы в автозапуск была выполнена команда **systemctl enable vsftpd**.  
   Статус изменился: теперь сервис не только работает, но и имеет состояние `enabled`, что гарантирует запуск при старте ОС.  

   ![Добавление службы vsftpd в автозапуск](Screenshot_3.png){ #fig:003 width=80% }

5. Для проверки отключения автозапуска была использована команда **systemctl disable vsftpd**.  
   Статус показал, что служба продолжает работать (*active*), но её автоматический запуск снова выключен (`disabled`).  

   ![Отключение автозапуска службы vsftpd](Screenshot_4.png){ #fig:004 width=80% }

6. Для просмотра списка сервисов, запускаемых при переходе в состояние **multi-user.target**, применялась команда **ls /etc/systemd/system/multi-user.target.wants/**.  
   В выводе отсутствовала ссылка на `vsftpd.service`.  

   После повторного включения автозапуска (**systemctl enable vsftpd**) символическая ссылка появилась, что подтверждает корректную настройку.  

   ![Символические ссылки сервисов в multi-user.target](Screenshot_5.png){ #fig:005 width=80% }

7. Для анализа зависимостей юнита использовалась команда **systemctl list-dependencies vsftpd**.  
   Она показала, что служба привязана к **multi-user.target** и, как следствие, к **graphical.target**.  

8. Для отображения обратных зависимостей была применена команда **systemctl list-dependencies vsftpd --reverse**.  
   В результате видно, что запуск `vsftpd` осуществляется в рамках **multi-user.target** и **graphical.target**.  

   ![Обратные зависимости vsftpd](Screenshot_6.png){ #fig:006 width=80% }

## Конфликты юнитов

1. Сначала были получены полномочия администратора. Затем выполнена установка пакета **iptables** с помощью команды **dnf -y install iptables\***.  
   В систему были установлены дополнительные пакеты: `iptables-devel`, `iptables-nft-services`, `iptables-utils`, а также обновлены библиотеки `iptables-libs` и `iptables-nft`.  

   ![Установка iptables](Screenshot_7.png){ #fig:007 width=80% }

2. Проверка состояния служб **firewalld** и **iptables** показала:  
   - `firewalld.service` работает (*active, running*) и включён в автозапуск (`enabled`),  
   - `iptables.service` неактивен (*inactive, dead*) и автозапуск для него отключён (`disabled`).  

   ![Статус firewalld и iptables](Screenshot_8.png){ #fig:008 width=80% }

3. При запуске обеих служб наблюдается конфликт:  
   - после старта **firewalld** запуск **iptables** останавливается,  
   - после старта **iptables** останавливается **firewalld**.  
   Это подтверждает, что данные сервисы не могут функционировать одновременно.  

   ![Конфликт запуска firewalld и iptables](Screenshot_9.png){ #fig:009 width=80% }

4. Содержимое файла юнита **firewalld.service** показывает, что данный сервис имеет явное указание на конфликт с `iptables.service`, `ip6tables.service`, `ebtables.service` и `ipset.service`.  
   Это означает, что при старте **firewalld** данные сервисы будут автоматически деактивированы.  

   ![Конфликты в firewalld.service](Screenshot_10.png){ #fig:010 width=80% }

5. Содержимое файла юнита **iptables.service** показывает отсутствие явных конфликтов.  
   Однако он настроен как сервис типа *oneshot* с возможностью перезапуска правил, и при старте `firewalld` его работа блокируется.  

   ![Содержимое iptables.service](Screenshot_11.png){ #fig:011 width=80% }

6. Для исключения конфликтов была выгружена служба **iptables** (**systemctl stop iptables**) и запущен **firewalld**.  
   Затем для предотвращения случайного запуска службы iptables применена команда **systemctl mask iptables**, которая создала символическую ссылку на `/dev/null`.  

   ![Маскирование iptables](Screenshot_12.png){ #fig:012 width=80% }

7. Попытка запустить или добавить **iptables** в автозагрузку показала сообщение об ошибке:  
   - запуск невозможен, так как сервис замаскирован,  
   - добавление в автозагрузку также не выполняется по той же причине.  

   Это гарантирует, что iptables не будет случайно активирован и не вызовет конфликт с firewalld.  

## Изолируемые цели

1. Сначала были получены полномочия администратора.  
   Для поиска целей, которые могут быть изолированы, использовалась команда **grep Isolate \*.target** в каталоге `/usr/lib/systemd/system`.  
   В результате отобразился список целей, содержащих строку `AllowIsolate=yes`, например: `multi-user.target`, `graphical.target`, `rescue.target`, `reboot.target` и другие.  

   ![Список изолируемых целей](Screenshot_13.png){ #fig:013 width=80% }

2. Операционная система была переведена в режим восстановления с помощью команды **systemctl isolate rescue.target**.  
   После этого для входа потребовалось ввести пароль суперпользователя.  

   ![Перевод системы в rescue.target](Screenshot_14.png){ #fig:014 width=80% }

3. Для перезапуска системы была выполнена команда **systemctl isolate reboot.target**, что инициировало процесс перезагрузки ОС.  

## Цель по умолчанию

1. После получения полномочий администратора была проверена текущая цель по умолчанию с помощью команды **systemctl get-default**.  
   По умолчанию была установлена графическая цель (**graphical.target**).  

   ![Текущая цель по умолчанию](Screenshot_15.png){ #fig:015 width=80% }

2. Для изменения цели по умолчанию и запуска системы в текстовом режиме использовалась команда **systemctl set-default multi-user.target**.  
   В каталоге `/etc/systemd/system/` была создана символическая ссылка на соответствующий юнит. После перезагрузки система загрузилась в режиме без графического интерфейса.  

3. Для возврата загрузки в графическом режиме использовалась команда **systemctl set-default graphical.target**.  
   После перезагрузки система вновь загружалась с графическим интерфейсом.  

   ![Возврат графического режима по умолчанию](Screenshot_16.png){ #fig:016 width=80% }

# Заключение

В ходе работы были изучены основные приёмы управления службами и целями в **systemd**.  
Было рассмотрено, как запускать и останавливать сервисы, включать и отключать их из автозагрузки, а также анализировать зависимости между юнитами.  
Особое внимание уделялось работе с изолируемыми целями (runlevels в терминах SystemV init) и настройке цели по умолчанию при загрузке системы.  
Кроме того, были исследованы конфликты юнитов (например, между `firewalld` и `iptables`) и способы их разрешения.  

# Контрольные вопросы

1. **Что такое юнит (unit)? Приведите примеры.**  
   Юнит — это объект управления systemd, который описывает сервис, сокет, точку монтирования, устройство, цель и т.д.  
   Примеры: `sshd.service`, `network.target`, `tmp.mount`, `rescue.target`.

2. **Какая команда позволяет вам убедиться, что цель больше не входит в список автоматического запуска при загрузке системы?**  
   systemctl disable имя_цели  

3. **Какую команду вы должны использовать для отображения всех сервисных юнитов, которые в настоящее время загружены?**  
   systemctl list-units --type=service  

4. **Как создать потребность (wants) в сервисе?**  
   systemctl enable имя_сервиса  
   (при этом создаётся символическая ссылка в каталог `*.wants/` на соответствующий юнит).

5. **Как переключить текущее состояние на цель восстановления (rescue target)?**  
   systemctl isolate rescue.target  

6. **Поясните причину получения сообщения о том, что цель не может быть изолирована.**  
   Цель не может быть изолирована, если в её юнит-файле отсутствует параметр `AllowIsolate=yes`.  

7. **Вы хотите отключить службу systemd, но, прежде чем сделать это, вы хотите узнать, какие другие юниты зависят от этой службы. Какую команду вы бы использовали?**  
   systemctl list-dependencies имя_сервиса --reverse  
