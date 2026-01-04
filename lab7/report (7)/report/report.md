---
## Front matter
title: "Отчёт по лабораторной работе №7"
subtitle: "Управление журналами событий в системе"
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

Получить навыки работы с журналами мониторинга различных событий в системе.

# Выполнение

## Мониторинг журнала системных событий в реальном времени

1. В трёх отдельных вкладках терминала получены права администратора командой **su -**.  
   Во второй вкладке выполнен запуск мониторинга системных сообщений в реальном времени командой **tail -f /var/log/messages**.  

   ![Мониторинг /var/log/messages в реальном времени — начало вывода](Screenshot_1.png){ #fig:001 width=80% }

2. В третьей вкладке выполнено возвращение к обычному пользователю (закрытие сеанса root через **Ctrl + d**).  
   Затем предпринята попытка получения прав суперпользователя с неверным паролем.  
   В мониторинге появилось сообщение `FAILED SU (to root) raliev on pts/2`.  

   ![Сообщения об аварийных дампах и FAILED SU](Screenshot_2.png){ #fig:002 width=80% }

3. В третьей вкладке введена команда **logger hello**.  
   В мониторинге во второй вкладке появилось сообщение `hello`.  

   ![Появление сообщения от logger в журнале](Screenshot_3.png){ #fig:003 width=80% }

4. Мониторинг был остановлен (**Ctrl + c**), затем выведены последние 20 строк файла `/var/log/secure`.  
   В выводе отображаются записи PAM и события авторизации, включая неудачную попытку su.  

   ![Вывод последних строк /var/log/secure с записями PAM и su](Screenshot_4.png){ #fig:004 width=80% }

## Изменение правил rsyslog.conf

1. В первой вкладке терминала установлен Apache при помощи команды **dnf -y install httpd**.  
   После завершения установки служба была запущена и добавлена в автозагрузку:  
   **systemctl start httpd**  
   **systemctl enable httpd**  

   ![Установка и запуск httpd](Screenshot_5.png){ #fig:005 width=80% }

2. Во второй вкладке выполнен просмотр журнала ошибок веб-сервера:  
   **tail -f /var/log/httpd/error_log**.  
   В журнале отобразились сообщения о запуске Apache, активации SELinux, конфигурации и запуске процесса httpd.  

   ![Журнал ошибок httpd](Screenshot_6.png){ #fig:006 width=80% }

3. В третьей вкладке открыт файл конфигурации `/etc/httpd/conf/httpd.conf`.  
   В его конец добавлена строка:  
   `ErrorLog syslog:local1`  
   Это позволяет перенаправлять сообщения об ошибках веб-сервера в системный журнал через объект **local1**.  

   ![Добавление строки ErrorLog syslog:local1](Screenshot_7.png){ #fig:007 width=80% }

4. В каталоге `/etc/rsyslog.d` создан новый файл **httpd.conf**, в который добавлена строка:  
   `local1.* -/var/log/httpd-error.log`  
   Таким образом, сообщения от веб-сервиса будут сохраняться в отдельном файле `/var/log/httpd-error.log`.  

   ![Файл конфигурации httpd.conf для rsyslog](Screenshot_8.png){ #fig:008 width=80% }

5. Для применения изменений выполнена перезагрузка сервисов:  
   **systemctl restart rsyslog.service**  
   **systemctl restart httpd**  

   После этого сообщения веб-службы стали фиксироваться в `/var/log/httpd-error.log`.

6. В каталоге `/etc/rsyslog.d` создан новый файл **debug.conf** для отладочных сообщений. В него добавлена строка:  
   `*.debug /var/log/messages-debug`  

   ![Создание файлов конфигурации rsyslog](Screenshot_9.png){ #fig:009 width=80% }

7. После перезапуска rsyslog командой **systemctl restart rsyslog.service** был запущен мониторинг отладочных сообщений:  
   **tail -f /var/log/messages-debug**  

8. В другой вкладке введена команда:  
   **logger -p daemon.debug "Daemon Debug Message"**  
   В результате сообщение появилось в выводе мониторинга отладочных событий.  

   ![Сообщение отладки в журнале](Screenshot_10.png){ #fig:010 width=80% }

# Выполнение

## Использование journalctl

1. Во второй вкладке терминала просмотрен журнал с событиями с момента последнего запуска системы командой **journalctl**.  
   На экране выводится последовательность сообщений ядра и служб, начиная с момента загрузки системы.  

   ![Просмотр журнала с момента загрузки](Screenshot_11.png){ #fig:011 width=80% }

2. Для просмотра содержимого журнала без использования постраничного режима применена команда **journalctl --no-pager**.  

   ![Вывод журнала без пейджера](Screenshot_12.png){ #fig:012 width=80% }

3. Запуск журнала в режиме реального времени осуществлён с помощью команды **journalctl -f**.  
   В выводе отобразились текущие события, включая ошибки приложений.  

   ![Режим реального времени в journalctl](Screenshot_13.png){ #fig:013 width=80% }

4. Для ознакомления с параметрами фильтрации после ввода команды **journalctl** дважды нажата клавиша **Tab**.  
   На экране появился список возможных ключей фильтрации.  

   ![Вывод списка параметров фильтрации](Screenshot_14.png){ #fig:014 width=80% }

5. Для отображения событий, зафиксированных от имени пользователя с UID=0, применена команда **journalctl _UID=0**.  
   На экране отображены сообщения, относящиеся к процессам, выполняемым от имени root.  

   ![Журнал по UID=0](Screenshot_15.png){ #fig:015 width=80% }

6. Для отображения последних 20 строк журнала использовалась команда **journalctl -n 20**.  
   В выводе зафиксированы свежие события, включая сообщения ядра и systemd.  

   ![Последние 20 строк журнала](Screenshot_16.png){ #fig:016 width=80% }

7. Для просмотра сообщений только с уровнем приоритета "ошибка" использовалась команда **journalctl -p err**.  
   В журнале отобразились ошибки графического драйвера, аудиосистемы и PAM.  

   ![Просмотр сообщений с уровнем error](Screenshot_17.png){ #fig:017 width=80% }

8. Для вывода всех сообщений со вчерашнего дня введена команда **journalctl --since yesterday**.  
   Показаны системные события, начиная с момента загрузки ядра.  

   ![Журнал со вчерашнего дня](Screenshot_18.png){ #fig:018 width=80% }

9. Для отображения сообщений уровня error со вчерашнего дня использована команда **journalctl --since yesterday -p err**.  
   На экране видно совмещение временного фильтра и уровня приоритета.  

   ![Ошибки со вчерашнего дня](Screenshot_19.png){ #fig:019 width=80% }

10. Для получения детальной информации применена команда **journalctl -o verbose**.  
    Журнал отобразил сообщения с дополнительными параметрами, включая идентификаторы процессов, устройств и подсистем.  

    ![Детализированный вывод журнала](Screenshot_20.png){ #fig:020 width=80% }

11. Для просмотра дополнительной информации о модуле **sshd** использована команда **journalctl _SYSTEMD_UNIT=sshd.service**.  
    В журнале зафиксированы строки о запуске службы и открытии порта 22.  

## Постоянный журнал journald

1. В терминале получены полномочия администратора.  
2. Создан каталог для хранения постоянных записей журнала:  
   **mkdir -p /var/log/journal**  
3. Изменены права доступа для каталога `/var/log/journal`, чтобы служба journald могла записывать туда информацию:  
   **chown root:systemd-journal /var/log/journal**  
   **chmod 2755 /var/log/journal**  
4. Для применения изменений отправлен сигнал службе journald:  
   **killall -USR1 systemd-journald**  
   Это позволило обойтись без перезагрузки системы.  
5. После включения постоянного хранения журнал просмотрен с момента последней перезагрузки командой:  
   **journalctl -b**  

   ![Создание постоянного журнала journald](Screenshot_21.png){ #fig:021 width=80% }

# Заключение

В ходе работы были изучены возможности администрирования журналов в Linux.  
Были рассмотрены способы просмотра системных сообщений с использованием **rsyslog** и **systemd-journald**, а также различия между ними.  

# Контрольные вопросы

1. **Какой файл используется для настройки rsyslogd?**  
   /etc/rsyslog.conf  

2. **В каком файле журнала rsyslogd содержатся сообщения, связанные с аутентификацией?**  
   /var/log/secure  

3. **Если вы ничего не настроите, то сколько времени потребуется для ротации файлов журналов?**  
   По умолчанию ротация выполняется раз в неделю (weekly).  

4. **Какую строку следует добавить в конфигурацию для записи всех сообщений с приоритетом info в файл /var/log/messages.info?**  
   *.info /var/log/messages.info  

5. **Какая команда позволяет вам видеть сообщения журнала в режиме реального времени?**  
   tail -f /var/log/messages  
   или  
   journalctl -f  

6. **Какая команда позволяет вам видеть все сообщения журнала, которые были написаны для PID 1 между 9:00 и 15:00?**  
   journalctl _PID=1 --since "09:00" --until "15:00"  

7. **Какая команда позволяет вам видеть сообщения journald после последней перезагрузки системы?**  
   journalctl -b  

8. **Какая процедура позволяет сделать журнал journald постоянным?**  
   - Создать каталог /var/log/journal  
   - Назначить права доступа:  
     chown root:systemd-journal /var/log/journal  
     chmod 2755 /var/log/journal  
   - Перезапустить journald сигналом:  
     killall -USR1 systemd-journald  
