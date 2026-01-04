---
## Front matter
title: "Отчёт по лабораторной работе №11"
subtitle: "Управление загрузкой системы"
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

Получить навыки работы с загрузчиком системы GRUB2.

# Выполнение

## Модификация параметров GRUB2

1. В терминале получены права суперпользователя с помощью команды **su -**.  
   После этого открыт для редактирования файл `/etc/default/grub` командой **nano /etc/default/grub**.  

   ![Редактирование файла /etc/default/grub](Screenshot_1.png){ #fig:001 width=80% }

   В файле задан параметр `GRUB_TIMEOUT=10`, определяющий время отображения меню загрузки, а также активированы настройки `GRUB_ENABLE_BLSCFG=true` и `GRUB_DISABLE_RECOVERY=true`.

2. После внесения изменений была выполнена команда **grub2-mkconfig > /boot/grub2/grub.cfg** для пересоздания конфигурационного файла загрузчика.  
   Процесс завершился успешно, о чём свидетельствует сообщение *done*.  

   ![Пересоздание конфигурации GRUB2](Screenshot_2.png){ #fig:002 width=80% }

3. После перезагрузки системы появилось меню загрузчика **GRUB version 2.12**, где отображаются доступные записи для запуска ОС.  

   ![Меню загрузчика GRUB](Screenshot_3.png){ #fig:003 width=80% }

4. Для перехода в режим восстановления (rescue) при загрузке была выбрана активная запись и нажата клавиша **e** для редактирования параметров.  
   В конце строки, начинающейся с `linux ($root)/vmlinuz-...`, добавлен параметр `systemd.unit=rescue.target`.  

   ![Редактирование параметров загрузки — режим rescue](Screenshot_4.png){ #fig:004 width=80% }

5. После загрузки в режиме восстановления выполнена проверка состояния загруженных модулей командой **systemctl list-units**, что позволило убедиться в запуске базовой системной среды.  
   Также просмотрены текущие переменные окружения с помощью **systemctl show-environment**.  

   ![Список активных модулей и переменные среды в режиме rescue](Screenshot_5.png){ #fig:005 width=80% }

6. Аналогично, для перехода в аварийный режим (emergency) в строку загрузки ядра был добавлен параметр `systemd.unit=emergency.target`.  

   ![Редактирование параметров загрузки — режим emergency](Screenshot_6.png){ #fig:006 width=80% }

   После запуска системы в этом режиме команда **systemctl list-units** показала, что количество активных модулей сведено к минимуму, что подтверждает загрузку только критически необходимых служб.  

   ![Список активных модулей в режиме emergency](Screenshot_7.png){ #fig:007 width=80% }

7. Для сброса пароля **root** при загрузке в редакторе GRUB в конец строки ядра добавлен параметр `rd.break`, который останавливает процесс на этапе загрузки `initramfs`.  

   ![Редактирование параметров загрузки для сброса пароля root](Screenshot_8.png){ #fig:008 width=80% }

   После загрузки в режиме `initramfs` выполнены команды:  
   - **mount -o remount,rw /sysroot** — повторное монтирование корневой файловой системы с правами записи;  
   - **chroot /sysroot** — установка системного корня;  
   - **passwd** — изменение пароля пользователя root;  
   - **reboot** — перезагрузка системы.  

   ![Работа в режиме initramfs при сбросе пароля root](Screenshot_9.png){ #fig:009 width=80% }

# Контрольные вопросы

1. **Какой файл конфигурации следует изменить для применения общих изменений в GRUB2?**  
   /etc/default/grub  

2. **Как называется конфигурационный файл GRUB2, в котором вы применяете изменения для GRUB2?**  
   /boot/grub2/grub.cfg  

3. **После внесения изменений в конфигурацию GRUB2, какую команду вы должны выполнить, чтобы изменения сохранились и воспринялись при загрузке системы?**  
   grub2-mkconfig > /boot/grub2/grub.cfg  
   или  
   grub2-mkconfig -o /boot/grub2/grub.cfg  

# Заключение

В ходе выполнения работы были изучены принципы настройки и модификации загрузчика **GRUB2** в операционной системе Linux.  
Были выполнены действия по изменению параметров конфигурации, пересозданию файла загрузки и проверке работы системы в различных режимах — **rescue**, **emergency** и при сбросе пароля пользователя **root**.  
Полученные навыки позволяют администратору уверенно управлять процессом загрузки системы, устранять неполадки и восстанавливать доступ в случае потери пароля суперпользователя.
