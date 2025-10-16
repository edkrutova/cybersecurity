---
title: "Лабораторная работа №2"
subtitle: "Кибербезопасность предприятия"
author: "Крутова Екатерина Дмитриевна, Прасолов Валерий Сернеевич | НПИ-22"
lang: ru-RU
toc-title: "Содержание"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
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
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
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
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Показать этапы реализации атак и контрмер в сценарии компрометации научно-технической информации предприятия, определить уязвимости, продемонстрировать доказательства успешной эксплуатации и предложить практические способы устранения.

# Теоретическое введение

Легенда: защита корпоративного мессенджера
Конкуренты решили скомпрометировать деятельность Компании и нашли для этого исполнителя. Злоумышленник находит в Интернете сайт соответствующего предприятия и решает провести атаку на него с целью получения доступа к внутренним ресурсам. Проэксплуатировав обнаруженную на сайте уязвимость, нарушитель стремится захватить управление другими ресурсами защищаемой сети, в том числе, пытается закрепиться на почтовом сервере и продолжить атаку. Главная задача злоумышленника - получение доступа к переписке сотрудников компании, раскрытие учётных данных пользователей, зарегистрированных в приложении корпоративного мессенджера, с целью использования их для нанесения ущерба репутации конкурирующей компании. Квалификация нарушителя высокая. Он умеет использовать инструментарий для проведения атак, а также знает техники постэксплуатации.

Уязвимости и последствия:

1. WordPress-wpDiscuz (CVE-2020-24186) (критическая уязвимость в плагине wpDiscuz (версии 7.0 — 7.0.4), позволяющая неаутентифицированному пользователю загружать любые файлы через уязвимый AJAX-эндпоинт (включая PHP).) → Deface

2. Proxylogon (CVE 2021-27065) (при наличии аутентификации (или после обхода через другие уязвимости) злоумышленник мог записать произвольный файл на сервер Exchange и добиться RCE.) → Exchange China Chopper

3. Rocket.Chat (CVE-2021-22911, CVE-2022-0847) (уязвимость в Rocket.Chat — недостаточная санация входа, приводящая к NoSQL-инъекции в неаутентифицированном API, что могло позволить похищение токенов сброса пароля и захват админского аккаунта) → Meterpreter

# Выполнение лабораторной работы

Обнаружение атак (рис. @fig:001 - @fig:002)

![Серия атак](image/image1.png){#fig:001 width=90%}

![продолжение атаки](image/Screenshot_2.png){#fig:002 width=90%}

Уязвимость 1. WordPress-wpDiscuz (CVE-2020-24186) (рис. @fig:003 - @fig:005)

С помощью WP Activity Log можно проверить журнал и обнаружить время, дату, роль и IP-адрес пользователя, который внес изменения. По информации из журналов на сервере можно отыскать причину взлома и устранить возникшие уязвимости.

![Окно пользовательской активности в WordPress](image/Screenshot_6.png){#fig:003 width=90%}

Для устранения уязвимости мы деактивировали WordPress.

![Деактивация WordPress](image/Screenshot_27.png){#fig:004 width=90%}

Также для устранения этой уязвимости необходимо удалить meterpreter сессию.

![Соединения с машиной нарушителя](image/Screenshot_3.png){#fig:005 width=90%}

Последствие 1. Deface (рис. @fig:006)

Для нейтрализации данной полезной нагрузки необходимо сформировать резервную копию с помощью плагина Updraft Backup/Restore.

![Успешное выполнение восстановления](image/Screenshot_28.png){#fig:006 width=90%}

Уязвимость 2. Proxylogon (CVE 2021-27065) (рис. @fig:007 - @fig:011)

![Вредоносная нагрузка](image/Screenshot_10.png){#fig:007 width=90%}

Данная уязвимость является следствием неэффективного ограничения выбора расположения backup виртуальной директории автономных адресных книг. Одним из шагов для эксплуатации данной уязвимости является изменение параметров OAB. После успешной авторизации нарушитель открыл веб-интерфейс настройки «Exchange – ecp» (Exchange Control Panel), в «Servers – Virtual directories» (Рисунок 12) и изменил параметры виртуальной директории.

![Интерфейс /ecp с открытой вкладкой виртуальной директории](image/Screenshot_4.png){#fig:008 width=90%}

В нашей реализации файлом backdoor является web.aspx (Рисунок 19). Для доступа к системе нарушителем выбрана директория /ecp, в данной директории находится вредоносный файл backdoor, представляющий из себя aspx web-shell.

![Вредоносная нагрузка](image/Screenshot_5.png){#fig:009 width=90%}

Для устранение уязвимости мы ограничили доступ к указанной директории для запрета эксплуатации уязвимости:

![«IP Address and Domain Restrictions»](image/Screenshot_8.png){#fig:010 width=90%}

Индикатор устранения не изменился, для выполнения этого пункта также необходимо решить последствие 2.

Последствие 2. Exchange China Chopper (рис. @fig:011)

Для устранения полезной нагрузки мы удалили файл веб-оболочки по пути С:\Program Files\Microsoft\Exchange Server\V15\FrontEnd\HttpProxy\..\auth\; завершили все соединения между уязвимой машиной и нарушителем.

![Соединения с машиной нарушителя](image/Screenshot_3.png){#fig:011 width=90%}

Уязвимость 3. Rocket.Chat (CVE-2021-22911, CVE-2022-0847) (рис. @fig:012 - @fig:019)

Обнаружение уязвимости:

![События ИБ в сетевом сенсоре ViPNet IDS NS](image/Screenshot_25.png){#fig:012 width=90%}

Для закрытия уязвимости мы выполнили следующие шаги (рис. @fig:013 - @fig:018):

Для восстановления доступа к аккаунту администратора необходимо сбросить пароль. Письмо с инструкциями для сброса пароля мы открыли при помощи текстового редактора, прочитав файл /var/mail/admin (рис. @fig:001)

![/var/mail/admin](image/Screenshot_11.png){#fig:013 width=90%}

Изменив пароль, мы отредактировали файл конфигурации БД /etc/mongod.conf:

![/etc/mongod.conf](image/Screenshot_19.png){#fig:014 width=90%}

![Включение двухфакторной аутентификации для пользователей](image/Screenshot_14.png){#fig:015 width=90%}

![Подтверждение электронной почты](image/Screenshot_15.png){#fig:016 width=90%}

![Включение двухфакторной аутентификации](image/Screenshot_16.png){#fig:017 width=90%}

![Настройка обязательной двухфакторной аутентификации](image/Screenshot_17.png){#fig:018 width=90%}

![Вредоносная нагрузка](image/Screenshot_18.png){#fig:019 width=90%}

Последствие 3. Meterpreter (рис. @fig:020)

Данная полезная нагрузка заключается в получении нарушителем meterpreter-сессии с уязвимым сервером.
Ее можно обнаружить и устранить(рис. @fig:020)

![Соединения с машиной нарушителя](image/Screenshot_3.png){#fig:020 width=90%}

Заполненные инциденты (рис. @fig:021 - @fig:026):

![Уязвимость 1](image/Screenshot_20.png){#fig:021 width=90%}

![Последствие 1](image/Screenshot_21.png){#fig:022 width=90%}

![Уязвимость 3](image/Screenshot_22.png){#fig:023 width=90%}

![Уязвимость 2](image/Screenshot_23.png){#fig:024 width=90%}

![Последствие 2](image/Screenshot_24.png){#fig:025 width=90%}

![Последствие 3](image/Screenshot_26.png){#fig:026 width=90%}

# Вывод

В ходе выполнения данной лабораторной работы был изучен сценарий атаки на систему защиты научно‑технической информации предприятия и способы её нейтрализации. Было рассмотрено, как цепочка уязвимостей может привести к компрометации данных, и какие меры позволяют устранить последствия.

# Список литературы
