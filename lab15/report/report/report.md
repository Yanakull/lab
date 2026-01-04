---
## Front matter
title: "Отчёт по лабораторной работе №15"
subtitle: "Управление логическими томами"
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

Получить навыки управления логическими томами.

# Выполнение

## Создание физического тома

1. С помощью `fdisk` выполнено разбиение диска **/dev/sdb**: создан основной раздел размером 300 МБ и установлен тип **LVM (8e)**.

   ![Создание раздела /dev/sdb1](Screenshot_1.png){ #fig:001 width=80% }

2. Таблица разделов обновлена с помощью `partprobe /dev/sdb`.

3. Создан физический том LVM на новом разделе.

4. Команда `pvs` подтвердила успешное создание физического тома:

## Создание группы томов и логического тома

1. Создана группа томов **vgdata** и добавлен физический том `/dev/sdb1`.

2. Проверка `vgs` показала успешное создание группы:

   ![Создание VG и проверка](Screenshot_2.png){ #fig:002 width=80% }

3. Создан логический том **lvdata**, использующий 50% доступного пространства группы томов.

4. Создана файловая система **ext4** на логическом томе:

   ![Создание ext4 на LV](Screenshot_3.png){ #fig:003 width=80% }

5. Создан каталог `/mnt/data` для монтирования.

6. В `/etc/fstab` добавлена строка для автомонтирования:

![Редактирование /etc/fstab](Screenshot_4.png){ #fig:004 width=80% }

7. После проверки файловая система была успешно смонтирована:

![Проверка монтирования](Screenshot_5.png){ #fig:005 width=80% }

## Изменение размера логических томов

1. В `fdisk` создан дополнительный раздел **/dev/sdb2** размером 300 МБ с типом **LVM (8e)**.

![Создание раздела /dev/sdb2](Screenshot_6.png){ #fig:006 width=80% }

2. Новый раздел преобразован в физический том и добавлен в группу томов **vgdata**.

3. Размер группы томов увеличился, что подтверждено выводом:

![Проверка VG после расширения](Screenshot_7.png){ #fig:007 width=80% }

4. Размер логического тома **lvdata** увеличен на 50% свободного пространства группы.

![Расширение LV и ФС](Screenshot_8.png){ #fig:008 width=80% }

5. Размер тома уменьшен на 50 МБ. Файловая система была уменьшена корректно:

![Итоговый размер тома](Screenshot_9.png){ #fig:009 width=80% }

#  Самостоятельная работа

## Создание логического тома и монтирование на /mnt/groups

1. На диске **/dev/sdc** были созданы два раздела:  
   — `/dev/sdc1` размером 600 МБ  
   — `/dev/sdc2` размером 450 МБ  
   Оба раздела имеют тип **Linux LVM (8e)**.

   ![Разметка диска /dev/sdc](Screenshot_10.png){ #fig:010 width=80% }

2. Созданы физические тома на новых разделах. После выполнения `pvcreate` оба PV успешно определились.

3. На основе физического тома `/dev/sdc1` создана группа томов **vggroup**.

4. На 100% свободного пространства группы создан логический том **lvgroup**.

5. Логический том был отформатирован в файловой системе **XFS**:

   ![Создание LV и файловой системы XFS](Screenshot_11.png){ #fig:011 width=80% }

6. Записи для постоянного монтирования были добавлены в `/etc/fstab`:

   ![Редактирование /etc/fstab](Screenshot_12.png){ #fig:012 width=80% }

7. После выполнения `mount -a` и проверки логический том успешно смонтирован в `/mnt/groups`:

   ![Проверка монтирования](Screenshot_13.png){ #fig:013 width=80% }

## Расширение логического тома на 150 МБ

1. Группа томов была расширена за счёт второго раздела `/dev/sdc2`:

![Расширение группы томов](Screenshot_14.png){ #fig:014 width=80% }

2. Логический том **lvgroup** увеличен на 100% свободного пространства (≈150 МБ):

Файловая система XFS была увеличена автоматически.

![Расширение LV и файловой системы XFS](Screenshot_15.png){ #fig:015 width=80% }

3. Итоговая проверка показала, что размер тома увеличен:

![Проверка конечного размера](Screenshot_16.png){ #fig:016 width=80% }

# Контрольные вопросы

1. **Какой тип раздела используется в разделе GUID для работы с LVM?**
   Тип раздела GUID для LVM: **8e00 (Linux LVM)**.

2. **Какой командой можно создать группу томов с именем vggroup, которая содержит физическое устройство /dev/sdb3 и использует физический экстент 4 MiB?**
   vgcreate **-s 4M** vggroup /dev/sdb3

3. **Какая команда показывает краткую сводку физических томов в вашей системе, а также группу томов, к которой они принадлежат?**
   pvs

4. **Что вам нужно сделать, чтобы добавить весь жёсткий диск /dev/sdd в группу томов группы?**
   Сначала создать физический том:
   pvcreate /dev/sdd
   Затем добавить его в группу томов:
   vgextend *имя_группы* /dev/sdd

5. **Какая команда позволяет вам создать логический том lvvol1 с размером 6 MiB?**
   lvcreate **-n lvvol1 -L 6M** *имя_группы*

6. **Какая команда позволяет вам добавить 100 МБ в логический том lvvol1, если пространство доступно?**
   lvextend **-L +100M** /dev/*группа*/lvvol1

7. **Каков первый шаг, чтобы добавить ещё 200 МБ в логический том, если требуемого пространства нет в группе томов?**
   Добавить новый физический том в группу томов (pvcreate → vgextend).

8. **Какую опцию нужно использовать с командой lvextend, чтобы также изменить размер файловой системы?**
   Использовать опцию **-r**.

9. **Как посмотреть, какие логические тома доступны?**
   lvs
   или
   lvdisplay

10. **Какую команду нужно использовать для проверки целостности файловой системы на /dev/vgdata/lvdata?**
    Для ext4:
    fsck.ext4 /dev/vgdata/lvdata
    или универсально:
    fsck /dev/vgdata/lvdata

# Заключение

В ходе работы были выполнены основные операции управления логическими томами в Linux: создание физических томов, групп томов и логических томов, их форматирование и монтирование. Отработаны приёмы изменения размеров томов и соответствующих файловых систем. Закреплены навыки использования утилит LVM и базовых инструментов администрирования дискового пространства.
