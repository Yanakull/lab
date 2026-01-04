---
## Front matter
title: "Отчёт по лабораторной работе №8"
subtitle: "Планировщики событий"
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

Получение навыков работы с планировщиками событий cron и at.

# Выполнение

## Работа с планировщиком cron

1. В терминале был выполнен переход в режим суперпользователя с помощью команды **su -**.  
   Затем с помощью команды **systemctl status crond -l** был проверен статус службы **crond**, отвечающей за выполнение задач по расписанию.  

   ![Статус службы crond](Screenshot_1.png){ #fig:001 width=80% }

   Служба **crond** находится в активном состоянии (*active (running)*), что подтверждает корректную работу планировщика задач.

2. Далее было просмотрено содержимое файла конфигурации **/etc/crontab**, в котором заданы глобальные параметры расписаний.  

   ![Содержимое файла /etc/crontab](Screenshot_2.png){ #fig:002 width=80% }

   В файле определены переменные окружения:
   - `SHELL=/bin/bash` — оболочка для выполнения заданий;
   - `PATH=/sbin:/bin:/usr/sbin:/usr/bin` — путь к исполняемым файлам;
   - `MAILTO=root` — адрес получателя уведомлений.

   Также приведён шаблон структуры задания:
   ```* * * * * user-name command ``` 
   где пять звёздочек означают соответственно: минуту, час, день месяца, месяц и день недели.

3. Для редактирования личного расписания root использована команда **crontab -e**.  
   В файл добавлена строка:  
   ```*/1 * * * * logger This message is written from root cron```  

   ![Создание задания в crontab](Screenshot_3.png){ #fig:003 width=80% }

   **Пояснение синтаксиса:**  
   - `*/1` — выполнение каждую минуту;  
   - `*` — любое значение часов, дней и месяцев;  
   - команда `logger` записывает сообщение в системный журнал `/var/log/messages`.

4. После сохранения изменений команда **crontab -l** подтвердила наличие записи в расписании.  
   Через несколько минут проверка с помощью **grep written /var/log/messages** показала, что сообщения действительно создаются каждую минуту.  

   ![Результат выполнения задания cron](Screenshot_4.png){ #fig:004 width=80% }

5. Затем задание было изменено на:  
   0 */1 * * 1-5 logger This message is written from root cron  

   ![Изменённая запись в crontab](Screenshot_5.png){ #fig:005 width=80% }

   **Пояснение синтаксиса:**  
   - `0 */1` — выполнение в начале каждого часа;  
   - `1-5` — с понедельника по пятницу;  
   - команда `logger` создаёт запись в журнале.

6. Далее был создан сценарий в каталоге **/etc/cron.hourly** с содержимым:  
   
   ```
   #!/bin/sh  
   logger This message is written at $(date)  
   ```
   
   ![Создание сценария eachhour](Screenshot_6.png){ #fig:006 width=80% }

   Файл был сделан исполняемым командой **chmod +x eachhour**, что позволяет выполнять его каждый час автоматически.

7. В каталоге **/etc/cron.d** создан файл **eachhour** со следующим содержимым:  
   11 * * * * root logger This message is written from /etc/cron.d  

   ![Файл расписания в /etc/cron.d](Screenshot_7.png){ #fig:007 width=80% }

   **Пояснение синтаксиса:**  
   - `11 * * * *` — выполнение на 11-й минуте каждого часа;  
   - `root` — пользователь, от имени которого выполняется команда;  
   - `logger` записывает сообщение в системный журнал.

## Работа с планировщиком at

1. Для одноразового запуска задач используется служба **atd**.  
   Её статус был проверен с помощью **systemctl status atd**.  

   ![Статус службы atd](Screenshot_8.png){ #fig:008 width=80% }

   Служба находится в активном состоянии (*active (running)*), что позволяет использовать утилиту **at**.

2. Командой **at 18:58** было запланировано выполнение задачи **logger message from at** на указанное время.  
   Проверка списка заданий командой **atq** подтвердила наличие одного активного задания.

3. После наступления времени выполнения команда **grep 'from at' /var/log/messages** показала, что сообщение было успешно записано в системный журнал.  

   ![Результат выполнения задания at](Screenshot_9.png){ #fig:009 width=80% }

# Контрольные вопросы

1. **Как настроить задание cron, чтобы оно выполнялось раз в 2 недели?**  
   Использовать комбинацию запуска каждое воскресенье с интервалом 14 дней:  
   0 0 */14 * * команда  

   или, если требуется более точный контроль, можно добавить условие через `date` в скрипте:  
   0 0 * * 0 [ $(($(date +%U) % 2)) -eq 0 ] && команда  

2. **Как настроить задание cron, чтобы оно выполнялось 1-го и 15-го числа каждого месяца в 2 часа ночи?**  
   0 2 1,15 * * команда  

3. **Как настроить задание cron, чтобы оно выполнялось каждые 2 минуты каждый день?**  
   */2 * * * * команда  

4. **Как настроить задание cron, чтобы оно выполнялось 19 сентября ежегодно?**  
   0 0 19 9 * команда  

5. **Как настроить задание cron, чтобы оно выполнялось каждый четверг сентября ежегодно?**  
   0 0 * 9 4 команда  

6. **Какая команда позволяет вам назначить задание cron для пользователя alice? Приведите подтверждающий пример.**  
   crontab -u alice -e  
   Пример:  
   crontab -u alice -e  
   и добавить строку:  
   0 9 * * * echo "Доброе утро, Alice" >> /home/alice/cron.log  

7. **Как указать, что пользователю bob никогда не разрешено назначать задания через cron? Приведите подтверждающий пример.**  
   Добавить имя пользователя **bob** в файл **/etc/cron.deny**:  
   echo "bob" >> /etc/cron.deny  

   После этого при попытке выполнить **crontab -e** от имени bob будет выдан отказ в доступе.

8. **Вам нужно убедиться, что задание выполняется каждый день, даже если сервер во время выполнения временно недоступен. Как это сделать?**  
   Использовать планировщик **anacron**, который выполняет пропущенные задания после восстановления работы системы.  
   Необходимо добавить задание в файл **/etc/anacrontab**.  
   Пример строки:  
   1 5 cron.daily nice run-parts /etc/cron.daily  

9. **Какая команда позволяет узнать, запланированы ли какие-либо задания на выполнение планировщиком atd?**  
   atq  


# Заключение

В ходе работы были изучены принципы планирования заданий в Linux с использованием планировщиков **cron** и **at**.  
Было рассмотрено создание периодических заданий, настройка расписаний различной сложности, а также работа с системными каталогами **/etc/cron.hourly** и **/etc/cron.d**.  
Практически подтверждена корректная работа службы **crond**, обеспечивающей выполнение заданий по расписанию, и службы **atd**, отвечающей за одноразовые отложенные задания.  
