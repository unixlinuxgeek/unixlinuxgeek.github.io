# 14 полезных команд и подсказок Linux.

Linux является операционной системой, которая питает Интернет. Для разработчика программного обеспечения важно хотя бы иметь представление о том, как работает Linux и как его использовать. В этой статье вы познакомитесь с командной строкой Linux.
Прежде чем мы продолжим, я предполагаю, что вы используете bash. Это интерпретатор команд, который поставляется по умолчанию со всеми основными дистрибутивами. Если вы не изменили его вручную, это должен быть bash.

# 1. !!

Сколько раз это случилось с вами? После ввода и запуска длинной команды вы обнаружите, что забыли добавить sudo в начале. Ну, вы можете просто ввести sudo !! и командная строка заменит !! с последней командой, которую вы пытались запустить:

```bash
askar@dell-Inspiron:~$ apt update
Reading package lists... Done
E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
E: Unable to lock directory /var/lib/apt/lists/
W: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (13: Permission denied)
W: Problem unlinking the file /var/cache/apt/srcpkgcache.bin - RemoveCaches (13: Permission denied)
askar@dell-Inspiron:~$ sudo !!
sudo apt update
[sudo] password for askar:
Hit:1 http://kz.archive.ubuntu.com/ubuntu focal InRelease
Get:2 http://kz.archive.ubuntu.com/ubuntu focal-updates InRelease [111 kB]
Get:3 http://dl.google.com/linux/chrome/deb stable InRelease [1,811 B]
Get:4 http://security.ubuntu.com/ubuntu focal-security InRelease [107 kB]
Get:5 http://dl.google.com/linux/chrome/deb stable/main amd64 Packages [1,148 B]
Get:6 http://kz.archive.ubuntu.com/ubuntu focal-backports InRelease [98.3 kB]
Get:7 http://kz.archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages [312 kB]
Get:8 http://kz.archive.ubuntu.com/ubuntu focal-updates/main i386 Packages [187 kB]
Get:9 http://kz.archive.ubuntu.com/ubuntu focal-updates/main Translation-en [116 kB]
Get:10 http://kz.archive.ubuntu.com/ubuntu focal-updates/main amd64 DEP-11 Metadata [196 kB]
Get:11 http://kz.archive.ubuntu.com/ubuntu focal-updates/main amd64 c-n-f Metadata [7,756 B]
Get:12 http://kz.archive.ubuntu.com/ubuntu focal-updates/restricted amd64 Packages [29.2 kB]
Get:13 http://kz.archive.ubuntu.com/ubuntu focal-updates/restricted Translation-en [7,732 B]
Get:14 http://kz.archive.ubuntu.com/ubuntu focal-updates/universe i386 Packages [77.5 kB]
Get:15 http://security.ubuntu.com/ubuntu focal-security/main amd64 Packages [148 kB]
Get:16 http://kz.archive.ubuntu.com/ubuntu focal-updates/universe amd64 Packages [142 kB]
Get:17 http://kz.archive.ubuntu.com/ubuntu focal-updates/universe Translation-en [71.8 kB]
Get:18 http://kz.archive.ubuntu.com/ubuntu focal-updates/universe amd64 DEP-11 Metadata [177 kB]
Get:19 http://kz.archive.ubuntu.com/ubuntu focal-updates/universe DEP-11 64x64 Icons [159 kB]
Get:20 http://kz.archive.ubuntu.com/ubuntu focal-updates/universe amd64 c-n-f Metadata [4,840 B]
Get:21 http://kz.archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 DEP-11 Metadata [2,468 B]
Get:22 http://kz.archive.ubuntu.com/ubuntu focal-backports/universe amd64 DEP-11 Metadata [1,972 B]
Get:23 http://security.ubuntu.com/ubuntu focal-security/main i386 Packages [55.9 kB]
Get:24 http://security.ubuntu.com/ubuntu focal-security/main Translation-en [52.0 kB]
Get:25 http://security.ubuntu.com/ubuntu focal-security/main amd64 DEP-11 Metadata [21.2 kB]
Get:26 http://security.ubuntu.com/ubuntu focal-security/main amd64 c-n-f Metadata [3,432 B]
Get:27 http://security.ubuntu.com/ubuntu focal-security/restricted amd64 Packages [29.2 kB]
Get:28 http://security.ubuntu.com/ubuntu focal-security/restricted Translation-en [7,732 B]
Get:29 http://security.ubuntu.com/ubuntu focal-security/universe i386 Packages [18.0 kB]
Get:30 http://security.ubuntu.com/ubuntu focal-security/universe amd64 Packages [42.8 kB]
Get:31 http://security.ubuntu.com/ubuntu focal-security/universe amd64 DEP-11 Metadata [35.8 kB]
Get:32 http://security.ubuntu.com/ubuntu focal-security/universe amd64 c-n-f Metadata [1,768 B]
Fetched 2,228 kB in 4s (538 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
46 packages can be upgraded. Run 'apt list --upgradable' to see them.
askar@dell-Inspiron:~$
```

# 2. Возвращение назад

Все знают, что вы можете перейти в каталог с помощью cd. Но почти никто не знает, что с помощью cd - вы можете вернуться к предыдущему каталогу:

```bash
askar@dell-Inspiron:~$ pwd
/home/askar
askar@dell-Inspiron:~$ cd /
askar@dell-Inspiron:/$ pwd
/
askar@dell-Inspiron:/$ cd -
/home/askar
askar@dell-Inspiron:~$ pwd
/home/askar
askar@dell-Inspiron:~$
```

# 3. Домашний каталог

Вы, вероятно, знаете, что ~ это ярлык для вашей домашней папки. Но есть хитрость, о которой мало кто знает: если вы введете cd без чего-либо после этого, он все равно перенесет вас в ваш домашний каталог:

```sh
askar@dell-Inspiron:/usr/bin$ pwd
/usr/bin
askar@dell-Inspiron:/usr/bin$ cd
askar@dell-Inspiron:~$ pwd
/home/askar
askar@dell-Inspiron:~$
```

# 4. Поиск

Широко известно, что вы можете просматривать историю с помощью стрелок. Разработчики настолько ленивы, что мы бы предпочли нажать стрелку вверх 15 раз, чтобы узнать, что у нас где-то было. Но это может быть достигнуто намного проще с функцией обратного поиска. Нажмите Ctrl + R и начните вводить команду, обратная поисковая заливка найдет ближайшее совпадение в вашей недавней истории:

```sh
(reverse-i-search)`systemctl': sudo systemctl status docker.service
```

# 5. Повторно используйте аргумент

Еще один удобный трюк - это !\$. Он будет заменен аргументами предыдущей команды. Это полезно, например, когда вы создаете папку и хотите перейти в нее:

```sh
askar@dell-Inspiron:~/Temp$ mkdir temp
askar@dell-Inspiron:~/Temp$ cd !$
cd temp
askar@dell-Inspiron:~/Temp/temp$ pwd
/home/askar/Temp/temp
askar@dell-Inspiron:~/Temp/temp$
```

# 6. Копирование и вставка

Вы, вероятно, заметили, что Ctrl + C и Ctrl + V не работают как обычно в терминале Linux. Чаще всего их заменяют Ctrl + Shift + C и Ctrl + Shit + V. Это потому, что Ctrl + C уже зарезервирован для завершения текущей запущенной программы.

# 7. Авторизация в SSH без пароля

Если вы часто входите на определенный сервер SSH, это может раздражать необходимость каждый раз вводить пароль. Вы можете пропустить его, если ваш хост и сервер обмениваются сертификатами.
Во-первых, вы должны сгенерировать один. Запустите команду ssh-keygen. Это создает пару секретного / открытого ключа и сохраняет ее в ~ / .ssh / id_rsa. Теперь вам нужно скопировать открытый ключ на сервер с помощью этой команды: ssh-copy-id [email protected] \_host. Вам будет предложено ввести пароль для сервера, и открытый ключ будет скопирован. Теперь вы можете войти на этот сервер без пароля от этой конкретной системы.
Примечание. Этот метод не менее безопасен, чем обычная аутентификация. Это может быть даже более безопасно, если ваша локальная система защищена. Если вы не скомпрометируете закрытый ключ, вы не сможете войти в SSH.

# 8. Держите вашу программу работающей в фоновом режиме

Если вы запустите программу в терминале, она будет уничтожена, как только вы завершите сеанс терминала. Чтобы предотвратить это и сохранить работу программы, используйте команду nohup - она означает «не вешать трубку».
Например, чтобы передать файлы на сервер и с сервера с помощью scp, при этом будучи уверенным, что передача продолжится, даже если вы случайно закроете окно терминала, используйте эту команду:

```sh
nohup scp /big-file.rar [email protected]:~/big-file.rar
```

nohup также создает файл с именем nohup.out, чтобы сохранить выходные данные команды.

# 9. Ответьте Да

Если вы пишете сценарии bash для автоматизации определенных задач, вы можете быть разочарованы тем, что вводите yes для каждой команды, которую вы запускаете. Чтобы пропустить это и ответить «да» на любую команду, добавьте yes |, например:

```sh
yes | apt-get update
```

Если вы хотите ответить «нет», добавьте yes no |.

# 10. Войти как Root

Это не лучшая практика, но иногда нет выбора. Однако лучший вариант это использовать sudo su. Команда su регистрирует вас как root, sudo будет выполнять как root. Следовательно, вам не нужен пароль root для этого. Более того, в некоторых дистрибутивах пароль root отключен, поэтому это единственный вариант:

```sh
askar@dell-Inspiron:~$ whoami
askar
askar@dell-Inspiron:~$ sudo su
[sudo] password for askar:
root@dell-Inspiron:/home/askar# whoami
root
root@dell-Inspiron:/home/askar#

```

# 11. Выход

Самый быстрый способ выхода из SSH, SFTP, root или из сеанса терминала - это сочетание клавиш Ctrl + D. Это удобно, когда вы обрабатываете много SSH-соединений или не можете ввести exit.

# 12 Shred

Если вы цените конфиденциальность, это для вас. Команда rm широко используется для удаления файлов, но она не удаляет их полностью. Даже после удаления можно извлечь данные с помощью специального программного обеспечения. Чтобы полностью удалить файл и заполнить его пространство нулями, используйте команду shred. Используйте это так: shred -zvu <имя файла>.

# 13. Список пользователей

Если у вас есть проблемы с конфиденциальностью, вы можете проверить, кто вошел в систему в любой момент времени. Вы можете использовать команду w, чтобы получить список всех пользователей, которые в данный момент находятся в системе. Кроме того, вы можете написать сценарий, который будет запускать эту команду по расписанию и отправить вам электронное письмо, если что-то не так.

# 14. Системная информация

Чтобы красиво показать информацию о вашей системе, установите и используйте команду screenfetch:

``````sh
askar@dell-Inspiron:~/Temp/temp$ screenfetch
                          ./+o+-       askar@dell-Inspiron
                  yyyyy- -yyyyyy+      OS: Ubuntu 20.04 focal
               ://+//////-yyyyyyo      Kernel: x86_64 Linux 5.4.0-40-generic
           .++ .:/++++++/-.+sss/`      Uptime: 1h 16m
         .:++o:  /++++++++/:--:/-      Packages: 2073
        o:+o+:++.`..```.-/oo+++++/     Shell: bash 5.0.17
       .:+o:+o/.          `+sssoo+/    Resolution: 1920x1080
  .++/+:+oo+o:`             /sssooo.   DE: GNOME 3.36.3
 /+++//+:`oo+o               /::--:.   WM: Mutter
 \+/+o+++`o++o               ++////.   WM Theme: Adwaita
  .++.o+++oo+:`             /dddhhh.   GTK Theme: Yaru-dark [GTK2/3]
       .+.o+oo:.          `oddhhhh+    Icon Theme: Yaru
        \+.++o+o``-````.:ohdhhhhh+     Font: Ubuntu 11
         `:o+++ `ohhhhhhhhyo++os:      Disk: 52G / 177G (31%)
           .o:`.syhhhhhhh/.oo++o`      CPU: Intel Core i5-7300HQ @ 4x 3.5GHz [59.0°C]
               /osyyyyyyo++ooo+++/     GPU: GeForce GTX 1050
                   ````` +oo+++o\:     RAM: 4469MiB / 7607MiB
                          `oo++.
askar@dell-Inspiron:~/Temp/temp$

``````
