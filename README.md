ДЗ 1. Изучение средств защиты ОС GNU/Linux

Делал на виртуальной машине в VirtualBox, система Fedora Server 44 без графики. Всё выполнял в консоли от своего пользователя gleb_adm, где нужны права root, использовал sudo. Мой номер студента 372260.

Пункт 1.1. Создал пользователя user1 с UID 1234 командой sudo useradd -u 1234 user1. Проверил через id user1, там uid=1234. Результат видно в файле passwd.

Пункт 1.1. Создал группу students командой sudo groupadd students и добавил в неё пользователя командой sudo usermod -aG students user1. Проверил через id user1, в списке групп появилась students. Результат видно в файле group.

Пункт 1.1. Задал пользователю пароль через sudo passwd user1 и настроил смену пароля раз в 90 дней командой sudo chage -M 90 user1. Проверил через sudo chage -l user1, максимальный срок стал 90 дней. Результат видно в файле shadow.

Пункт 2.1. Нашёл все файлы с битом set-UID командой sudo find / -perm -4000 -type f 2>/dev/null и сохранил результат в файл suid-files.out. В списке есть passwd, sudo, su, mount и другие.

Пункт 2.2. Запустил passwd в фоне, он висел и ждал ввода пароля. В это время через ps -eo pid,ruid,euid,user,cmd и awk отобрал процессы, у которых реальный UID от 1000 и выше, а эффективный равен 0. Нашёлся процесс passwd с RUID 1000 и EUID 0. Результат в файле processes.out.

Пункт 3.1. Взял утилиту cat и операцию чтения файла /etc/shadow. Файл /etc/passwd из примера может прочитать любой пользователь, поэтому выбрал shadow, его читает только root. Проверил, что обычный cat /etc/shadow выдаёт Permission denied.

Пункт 3.2. Скопировал cat к себе в домашнюю папку как mycat, сделал владельцем root командой sudo chown root:root ~/mycat и поставил бит set-UID командой sudo chmod u+s ~/mycat. Сначала делал chown, потому что он сбрасывает бит set-UID. После этого ~/mycat /etc/shadow читает файл без sudo. Права и владельца видно в stat.out, там 4755 и root.

Пункт 4.1. Взял утилиту chown и операцию смены владельца своего файла на другого пользователя. Проверил, что обычный chown user1 ~/test.txt выдаёт Operation not permitted.

Пункт 4.2. Скопировал chown как mychown и выдал ему только одну привилегию командой sudo setcap cap_chown+ep ~/mychown. После этого ~/mychown user1 ~/test.txt сработал без sudo, ls -l показал, что владелец файла теперь user1. Привилегию видно в getcap.out, сам файл в stat.out.

Пункт 5. Через sudo visudo добавил в /etc/sudoers строку user1 ALL=(root) /usr/bin/date, /usr/bin/timedatectl. Проверил через sudo -l -U user1, правило видно. Потом зашёл под user1 и поменял время через sudo date -s, это сработало, а sudo cat /etc/shadow ему запрещён. После проверки вернул точное время через chronyc makestep. Результат в файле sudoers.

Пункт 6.1. Вывел в консоль свой номер командой echo и сделал скриншот окна виртуальной машины, файл mephi-screenshot.png.

Пункт 6.2. Сохранил stat для /home/user1, mycat и mychown в файл stat.out, вывод getcap в getcap.out, историю команд в history.out. Скопировал /etc/passwd, /etc/shadow, /etc/group и /etc/sudoers.

Пункт 7. Выложил файлы на GitHub прямо из виртуальной машины через git, вход сделал через gh auth login. Проверил, что репозиторий открывается в режиме инкогнито.

В history.out есть опечатки и неудачные команды, например groupadd без sudo. Оставил как есть, это реальная история работы.
