---
## Front matter
title: "Отчёт по лабораторной работе №9"
subtitle: "Управление SELinux"
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

Получить навыки работы с контекстом безопасности и политиками SELinux.

# Выполнение

## Управление режимами SELinux

1. Для начала был запущен терминал и получены права суперпользователя с помощью команды **su -**.  
   Затем выполнена команда **sestatus -v** для просмотра текущей информации о состоянии SELinux.  

   ![Вывод команды sestatus -v](Screenshot_1.png){ #fig:001 width=80% }

   Из вывода команды видно:  
   - **SELinux status: enabled** — система SELinux включена.  
   - **Current mode: enforcing** — включён принудительный режим, при котором политики безопасности строго соблюдаются.  
   - **Mode from config file: enforcing** — данный режим также задан в конфигурационном файле.  
   - **Policy MLS status: enabled** — используется многоуровневая модель безопасности (MLS).  
   - **Loaded policy name: targeted** — применяется политика *targeted*, ограничивающая доступ для определённых служб.  
   - В разделе *Process contexts* указаны контексты процессов, таких как `init` и `sshd`.  
   - В разделе *File contexts* отображаются метки безопасности системных файлов (`/etc/passwd`, `/etc/shadow`, `/bin/bash` и др.).  

2. Для проверки текущего режима SELinux введена команда **getenforce**.  
   Результат показал, что режим — *Enforcing* (принудительный).  

3. С помощью команды **setenforce 0** режим SELinux был изменён на *Permissive* (разрешающий).  
   После повторного выполнения команды **getenforce** система подтвердила, что теперь SELinux работает в разрешающем режиме.  

   ![Изменение режима SELinux на Permissive](Screenshot_2.png){ #fig:002 width=80% }

4. Затем был открыт файл конфигурации `/etc/sysconfig/selinux` с помощью редактора **nano**.  
   Параметр **SELINUX** был изменён на `disabled`, что полностью отключает SELinux после перезагрузки системы.  

   ![Отключение SELinux в файле конфигурации](Screenshot_3.png){ #fig:003 width=80% }

5. После перезагрузки системы команда **getenforce** показала, что SELinux отключён (*Disabled*).  
   Попытка включить режим принудительного исполнения (**setenforce 1**) завершилась ошибкой, поскольку при полностью отключённом SELinux изменение режима невозможно без перезагрузки.  

   ![Попытка изменить режим при отключённом SELinux](Screenshot_4.png){ #fig:004 width=80% }

6. Для повторного включения SELinux значение параметра **SELINUX** в конфигурационном файле было изменено на `enforcing`.  

   ![Включение SELinux в конфигурации](Screenshot_5.png){ #fig:005 width=80% }

7. После перезагрузки система выполнила процедуру **relabeling** — восстановление меток SELinux для всех файлов.  
   Процесс сопровождается предупреждением о необходимости переназначения контекстов и может занять некоторое время.  

   ![Автоматическое восстановление меток SELinux](Screenshot_6.png){ #fig:006 width=80% }

8. После завершения восстановления и повторного запуска системы была снова выполнена команда **sestatus -v**.  
   Из вывода видно, что SELinux работает в режиме **Enforcing**.  

   ![Проверка состояния SELinux после включения](Screenshot_7.png){ #fig:007 width=80% }

## Использование restorecon для восстановления контекста безопасности

1. Для начала были получены права суперпользователя.  
   Затем с помощью команды **ls -Z /etc/hosts** был просмотрен контекст безопасности файла `/etc/hosts`.  
   Метка контекста имела значение **net_conf_t**, что соответствует сетевым конфигурационным файлам.  

2. Файл `/etc/hosts` был скопирован в домашний каталог (**cp /etc/hosts ~/**), после чего его контекст изменился на **admin_home_t**, что характерно для файлов, созданных пользователем в домашней директории.  

3. Затем файл был перемещён обратно в каталог `/etc` (**mv ~/hosts /etc**), и его контекст остался **admin_home_t**, что является некорректным для данного пути.  

4. Для исправления контекста использована команда **restorecon -v /etc/hosts**.  
   В результате контекст был возвращён к правильному значению **net_conf_t**, что подтверждает корректную работу утилиты `restorecon`.  

5. Для массового исправления контекста безопасности на файловой системе была создана метка **/.autorelabel**, после чего выполнена перезагрузка.  
   Во время загрузки система автоматически выполнила процедуру перемаркировки файловой системы.  

   ![Автоматическое восстановление контекста безопасности при загрузке](Screenshot_8.png){ #fig:008 width=80% }

## Настройка контекста безопасности для нестандартного расположения файлов веб-сервера

1. В терминале были получены права суперпользователя и установлены необходимые пакеты **httpd** и **lynx** для развертывания и проверки работы веб-сервера.  
   После этого был создан каталог `/web` для размещения пользовательских веб-файлов.  

2. В каталоге `/web` был создан файл **index.html** с содержимым:  
   *Welcome to my web-server*.  

3. В конфигурационном файле Apache `/etc/httpd/conf/httpd.conf` были внесены изменения:  
   - строка `DocumentRoot "/var/www/html"` была закомментирована;  
   - добавлена новая строка `DocumentRoot "/web"`;  
   - добавлен блок конфигурации для каталога `/web`, разрешающий доступ к содержимому.  

   ![Изменение файла конфигурации Apache](Screenshot_9.png){ #fig:009 width=80% }

4. После сохранения изменений служба Apache была запущена и добавлена в автозагрузку.  
   При первом обращении к веб-серверу через текстовый браузер **lynx** по адресу `http://localhost` отобразилась стандартная тестовая страница Apache — это указывает, что SELinux заблокировал доступ к новому каталогу `/web`.  

   ![Стандартная тестовая страница Apache](Screenshot_10.png){ #fig:010 width=80% }

5. Для корректной работы сервера в контексте безопасности SELinux был добавлен новый контекст для каталога `/web`:  
   `semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"`  
   Затем команда **restorecon -R -v /web** применила метки к каталогу и файлам.  

   ![Применение нового контекста безопасности к каталогу /web](Screenshot_11.png){ #fig:011 width=80% }

6. После обновления контекста и повторного обращения к веб-серверу через **lynx** по адресу `http://localhost` отобразилась пользовательская страница с надписью:  
   *Welcome to my web-server*.  
   Это подтвердило, что SELinux разрешает веб-серверу доступ к каталогу `/web` с правильной меткой безопасности.  

   ![Отображение пользовательской страницы веб-сервера](Screenshot_12.png){ #fig:012 width=80% }

## Работа с переключателями SELinux

1. В терминале с правами суперпользователя был просмотрен список переключателей SELinux, связанных с FTP-службой, с помощью команды:  
   **getsebool -a | grep ftp**  
   В результате обнаружен переключатель **ftpd_anon_write**, значение которого по умолчанию — *off*.  

2. Для получения подробной информации о переключателях, связанных с анонимным доступом FTP, использована команда:  
   **semanage boolean -l | grep ftpd_anon**,  
   которая показывает текущее состояние и назначение каждого параметра.  

3. Для разрешения анонимной записи был включён переключатель **ftpd_anon_write**:  
   - временно, командой **setsebool ftpd_anon_write on**;  
   - затем — постоянно, с помощью **setsebool -P ftpd_anon_write on**.  

4. После изменения параметров повторная проверка командой **getsebool ftpd_anon_write** и **semanage boolean -l | grep ftpd_anon** показала, что оба состояния переключателя (*временное* и *постоянное*) установлены в значение **on**.  

   ![Просмотр и изменение переключателя ftpd_anon_write](Screenshot_13.png){ #fig:013 width=80% }

# Контрольные вопросы

1. **Вы хотите временно поставить SELinux в разрешающем режиме. Какую команду вы используете?**  
   setenforce 0  

2. **Вам нужен список всех доступных переключателей SELinux. Какую команду вы используете?**  
   getsebool -a  

3. **Каково имя пакета, который требуется установить для получения легко читаемых сообщений журнала SELinux в журнале аудита?**  
   setroubleshoot  

4. **Какие команды вам нужно выполнить, чтобы применить тип контекста httpd_sys_content_t к каталогу /web?**  
   semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"  
   restorecon -R -v /web  

5. **Какой файл вам нужно изменить, если вы хотите полностью отключить SELinux?**  
   /etc/sysconfig/selinux  

6. **Где SELinux регистрирует все свои сообщения?**  
   /var/log/audit/audit.log  

7. **Вы не знаете, какие типы контекстов доступны для службы ftp. Какая команда позволяет получить более конкретную информацию?**  
   semanage fcontext -l | grep ftp  

8. **Ваш сервис работает не так, как ожидалось, и вы хотите узнать, связано ли это с SELinux или чем-то ещё. Какой самый простой способ узнать?**  
   временно перевести SELinux в разрешающий режим командой **setenforce 0** и проверить работу сервиса  


# Заключение

В ходе лабораторной работы были изучены механизмы функционирования системы безопасности **SELinux** и её взаимодействие с сервисами операционной системы.  
Были рассмотрены различные режимы работы SELinux — **Enforcing**, **Permissive** и **Disabled**, а также способы их переключения.  
Изучены принципы настройки контекстов безопасности и восстановления меток при помощи утилит **restorecon** и **semanage**.  
