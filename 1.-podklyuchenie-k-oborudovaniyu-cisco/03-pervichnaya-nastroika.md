# Первичная настройка

Управление через консоль доступно сразу, а вот для телнета нужно установить пароль. Как это сделать?

Обратимся к PT. Начнём с создания маршрутизатора: выбираем его на панели внизу и переносим на рабочее пространство. Даём какое-нибудь название:

![c2811](http://img-fotki.yandex.ru/get/4526/83739833.f/0_7c481_16c83eff_XL.jpg)

Что бы вы делали, если бы это был самый взаправдашний железный маршрутизатор? Взяли бы консольный кабель и подключились им в него и в компьютер. То же самое сделаем и тут:

![Packet tracer console](http://img-fotki.yandex.ru/get/3/83739833.10/0_7c4a6_b8cd01b3_XL.jpg)

![Packet tracer console](http://img-fotki.yandex.ru/get/3008/83739833.10/0_7c4aa_e740e023_XL.jpg)

Кликом по компьютеру вызываем окно настройки, в котором нас интересует вкладка Desktop. Далее выбираем Terminal, где нам даётся выбор параметров

![](http://img-fotki.yandex.ru/get/4/83739833.10/0_7c4a8_b45c05a0_XL.jpg)

Впрочем, все параметры по умолчанию нас устраивают, и менять их особо смысла нет. Если в энергонезависимой памяти устройства отсутствует конфигурационный файл \(startup-config\), а так оно и будет при первом включении нового железа, нас встретит Initial Configuration Dialog prompt:

![cisco interface](http://img-fotki.yandex.ru/get/4424/83739833.10/0_7c4a9_139a7ae4_XL.jpg)

Вкратце, это такой визард, позволяющий шаг за шагом настроить основные параметры устройства \(hostname, пароли, интерфейсы\). Но это неинтересно, поэтому отвечаем **no** и видим приглашение

> Router&gt;

Это стандартное совершенно для любой линейки cisco приглашение, которое характеризует **пользовательский режим**, в котором можно просматривать некоторую статистику и проводить самые простые операции вроде пинга. Ввод знака вопроса покажет список доступных команд:

Грубо говоря, это режим для сетевого оператора, инженера первой линии техподдержки, чтобы он ничего там не повредил, не напортачил и лишнего не узнал.  
Гораздо большие возможности предоставляет режим с говорящим названием **привилегированный**. Попасть в него можно, введя команду **&gt;enable**. Теперь приглашение выглядит так:

![](http://img-fotki.yandex.ru/get/4424/83739833.f/0_7c483_e700580b_XL.jpg)

> Router\#

Здесь список операций гораздо обширнее, например, можно выполнить одну из наиболее часто используемых команд, демонстрирующую текущие настройки устройства ака “конфиг” **\#show running-config**. В привилегированном режиме вы можете просмотреть всю информацию об устройстве.

## Полезные команды

Прежде, чем приступать к настройке, упомянем несколько полезностей при работе с cisco CLI, которые могут сильно упростить жизнь:  
— Все команды в консоли можно сокращать. Главное, чтобы сокращение однозначно указывало на команду. Например, **show running-config** сокращается до **sh run**. Почему не до **s r**? Потому, что **s** \(в пользовательском режиме\) может означать как команду **show**, так и команду **ssh**, и мы получим сообщение об ошибке % Ambiguous command: «s r» \(неоднозначная команда\).  
— Используйте клавишу **Tab** и знак вопроса. По нажатию Tab сокращенная команда дописывается до полной, а знак вопроса, следующий за командой, выводит список дальнейших возможностей и небольшую справку по ним \(попробуйте сами в PT\).  
— Используйте горячие клавиши в консоли:

**Ctrl+A** — Передвинуть курсор на начало строки  
**Ctrl+E** — Передвинуть курсор на конец строки  
**Курсорные Up, Down** — Перемещение по истории команд  
**Ctrl+W** — Стереть предыдущее слово  
**Ctrl+U** — Стереть всю линию  
**Ctrl+C** — Выход из режима конфигурирования  
**Ctrl+Z** — Применить текущую команду и выйти из режима конфигурирования  
**Ctrl+Shift+6** — Остановка длительных процессов \(так называемый escape sequence\)

— Используйте фильтрацию вывода команды. Бывает, что команда выводит много информации, в которой нужно долго копаться, чтобы найти определённое слово, например.  
Облегчаем работу с помощью фильтрации: после команды ставим \|, пишем вид фильтрации и, собственно, искомое слово\(или его часть\). Виды фильтрации \(ака модификаторы вывода\):

**begin** — вывод всех строк, начиная с той, где нашлось слово,  
**section** — вывод секций конфигурационного файла, в которых встречается слово,  
**include** — вывод строк, где встречается слово,  
**exclude** — вывод строк, где НЕ встречается слово.

Но вернемся к режимам. Третий главный режим, наряду с пользовательским и привилегированным: **режим глобальной конфигурации**. Как понятно из названия, он позволяет нам вносить изменения в настройки устройства. Активируется командой **\#configure terminal** из привилегированного режима и демонстрирует такое приглашение:

> Router\(config\)\#

В режиме глобальной конфигурации не выполняются довольно нужные порой команды других режимов \(тот же show running-config, ping, etc.\). Но есть такая полезная штука, как **do**. Благодаря ей мы можем, не выходя из режима конфигурирования, выполнять эти самые команды, просто добавляя перед ними do. Примерно так:

> Router\(config\)\#do show running-config
