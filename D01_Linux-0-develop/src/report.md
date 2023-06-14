# Report


## Part 1. Установка ОС

- Установить Ubuntu 20.04 Server LTS без графического интерфейса. (Используем программу для виртуализации - VirtualBox)

![a](../screenshots/part1_1.PNG)

```bash
cat /etc/issue
```

## Part 2. Создание пользователя

- Создать пользователя, отличного от пользователя, который создавался при установке. Пользователь должен быть добавлен в группу `adm`

![b](../screenshots/part2_1.PNG)

![add](../screenshots/part2_2.PNG)

```bash
sudo adduser timyr
cat /etc/passwd
sudo usermod -aG adm timyr
members adm
```

## Part 3. Настройка сети ОС

- Задать название машины вида user-1 

![c](../screenshots/part3_1.PNG) 

```bash
hostnamectl
hostnamectl set-hostname user-1
hostnamectl
```

- Установить временную зону, соответствующую вашему текущему местоположению.

![d](../screenshots/part3_2.PNG)

```bash
timedatectl list-timezones | grep Kazan
timedatectl list-timezones | grep Moscow
sudo timedatectl set-timezone Europe/Moscow
timedatectl
```  

- Вывести названия сетевых интерфейсов с помощью консольной команды.

![e](../screenshots/part3_3.PNG)

```bash
ip -s link
``` 

Интерфейс lo (loopback) - виртуальный интерфейс, присутствующий по умолчанию в любом Linux. lo используется для того, чтобы компьютер мог обращаться к самому себе и имеет по умолчанию ip-адрес 127.0.0.1 на всех компьютерах.

- Используя консольную команду получить ip адрес устройства, на котором вы работаете, от DHCP сервера. 

![f](../screenshots/part3_4.PNG)

```bash
ip route
``` 

DHCP - Dynamic Host Configuration Protocol - протокол, использующийся для автоматического выставления различной конфигурации, в том числе IP-адресов.

- Определить и вывести на экран внешний ip-адрес шлюза (ip) и внутренний IP-адрес шлюза, он же ip-адрес по умолчанию (gw). 

![g](../screenshots/part3_5.PNG) 

Внешний

![h](../screenshots/part3_6.PNG) 

Внутренний

```bash
wget -O - -q icanhazip.com
ip route show | grep default
```


- Задать статичные (заданные вручную, а не полученные от DHCP сервера) настройки ip, gw, dns (использовать публичный DNS серверы, например 1.1.1.1 или 8.8.8.8).  

![i](../screenshots/part3_7.PNG)

```bash
sudo vim /etc/netplan/00-installer-config.yalm
sudo netplan apply
```
- Перезагрузить виртуальную машину. Убедиться, что статичные сетевые настройки (ip, gw, dns) соответствуют заданным в предыдущем пункте.  

![j](../screenshots/part3_8.PNG)

```bash
sudo shutdown -r now
ip a | grep "global enp0s3"
systemd-resolve --status | grep "DNS Servers"
ping 1.1.1.1
ping ya.ru
```

## Part 4. Обновление ОС

- Обновить системные пакеты до последней на момент выполнения задания версии.  

![k](../screenshots/part4_1.PNG)

```bash
sudo apt update
```

## Part 5. Использование команды **sudo**

- Разрешить пользователю, созданному в Part 2, выполнять команду sudo.

![l](../screenshots/part5_1.PNG)

```bash
sudo usermod -aG sudo timyr
hostname
su timyr
sudo hostnamectl set-hostname user-2
hostname
```

`sudo` — консольная команда выполняющая команду переданную ей как аргумент с правами суперпользователя (root). Запуск команды, которая создаёт файлы и директории из-под sudo, приводит к тому, что владельцем этих файлов становится пользователь root. Фактически все последующие обращения к этому файлу без sudo начнут выдавать ошибку об отсутствии прав доступа. 

## Part 6. Установка и настройка службы времени

- Настроить службу автоматической синхронизации времени.  

![m](../screenshots/part6_1.PNG)

```bash
timedatectl show
```

## Part 7. Установка и использование текстовых редакторов 

- Установить текстовые редакторы **VIM** (+ любые два по желанию **NANO**, **MCEDIT**, **JOE** и т.д.)

![n](../screenshots/part7_1.PNG)

```bash
sudo apt install nano
sudo apt install vim
sudo apt install mcedit
```

- Используя каждый из трех выбранных редакторов, создайте файл *test_X.txt*, где X -- название редактора, в котором создан файл. Напишите в нём свой никнейм, закройте файл с сохранением изменений.

![o](../screenshots/part7_2.PNG)

```bash
touch test_vim.txt
vim test_vim.txt
```

vim: `escape` + `:` + `w` + `q` + `enter` - для выхода и сохранения изменений

![p](../screenshots/part7_3.PNG)

```bash
touch test_nano.txt
nano test_nano.txt
```

nano: `ctrl` + `x` + `y` + `enter` - для выхода и сохранения изменений

![q](../screenshots/part7_4.PNG)

```bash
touch test_mcedit.txt
mcedit test_mcedit.txt
```

mcedit: `F2` + `enter` + `F10` - для выхода и сохранения изменений

- Используя каждый из трех выбранных редакторов, откройте файл на редактирование, отредактируйте файл, заменив никнейм на строку "21 School 21", закройте файл без сохранения изменений.

![r](../screenshots/part7_5.PNG)

```bash
vim test_vim.txt
```
vim: `escape` + `:` + `!` + `q` + `enter` - для выхода без сохранения

![s](../screenshots/part7_6.PNG)

```bash
nano test_nano.txt
```

nano: `ctrl` + `x` + `n` - для выхода без сохранения

![t](../screenshots/part7_7.PNG)

```bash
mcedit test_mcedit.txt
```

mcedit: `F10` + `No` - для выхода без сохранения

- Используя каждый из трех выбранных редакторов, отредактируйте файл ещё раз (по аналогии с предыдущим пунктом), а затем освойте функции поиска по содержимому файла (слово) и замены слова на любое другое.

![u](../screenshots/part7_8.PNG)

![v](../screenshots/part7_9.PNG)

```bash
vim test_vim.txt
```

vim: `escape` + `/` + `target` - для поиска слова

vim: `escape` + `%` + `s` + `target` + `/` + `replace target` + `enter` - для замены слова    

![w](../screenshots/part7_10.PNG)

![x](../screenshots/part7_11.PNG)

```bash
nano test_nano.txt
```
nano: `ctrl` + `w` + `target` + `enter` - для поиска слова

nano: `ctrl` + `/` + `target` + `enter` + `replace target` + `enter` + `y` - для замены слова   

![x](../screenshots/part7_12.PNG)

![y](../screenshots/part7_13.PNG)

```bash
mcedit test_mcedit.txt
```

mcedit: `F7` + `target` + `Ok` + `enter` - для поиска слова

mcedit: `F4` + `target` + `replace target` + `Ok` + `enter` - для замены слова   

## Part 8. Установка и базовая настройка сервиса **SSHD**

- Установить службу SSHd.

![z](../screenshots/part8_1.PNG)

```bash
sudo apt-get install openssh-server
```

- Добавить автостарт службы при загрузке системы.  

![aa](../screenshots/part8_2.PNG)

```bash
sudo systemctl enable ssh
```

![ab](../screenshots/part8_3.PNG)

```bash
systemctl list-unit-files --type=service --state=enabled
```

- Перенастроить службу SSHd на порт 2022.

![ac](../screenshots/part8_4.PNG)

```bash
sudo nano /etc/ssh/sshd_config
```

- Используя команду ps, показать наличие процесса sshd. Для этого к команде нужно подобрать ключи.

![ad](../screenshots/part8_5.PNG)

```bash
ps -aux | grep sshd
```

`--output--`

`UID` - польтзователь, от имени которого запущен процесс
`PID` - инденфикатор процесса
`PPID` - индентификатор родительского процесса
`C` - процент времени CPU, используемое процессом
`SZ` - размера процесса в памяти
`RSS` - реальный размер процесса в памяти
`PSR` - ядро процессора, на котором выполняется процесс
`STIME` - время запуска процесса
`TTY` - терминал, из которого запущен процесс
`TIME` - общее время процессора, затраченного на выполнение процесса
`CMD` - команда запуска процесса

`--input--`

`ps`выводит список текущих процессов на сервере
`a` выбрать все процессы, кроме фоновых.
`u` выбрать процессы пользователя.
`x` выводит список всех процессов, принадлежащих вам (тот же EUID, что и ps), или выводит список всех процессов при использовании вместе с опцией `a`

- Перезагрузить систему.

![ae](../screenshots/part8_6.PNG)

```bash
sudo shutdown -r now
netstat -tan
```

`--output--`

`Proto` - протокол используемый сокетом
`Recv-Q` - количество байтов не переданных пользователем
`Send-Q` - количество байтов не подтвержденных удаленным хостом
`Local Address` - локальный адрес и номер порта локальной сети
`Foreign Address` - локальный адрес и номер порта удаленной сети
`State` - состояние сокета
`0.0.0.0` означает, что подключение может быть выполнено с/на любой адрес
`LISTEN` готовность к установке соединения

`--input--`

`netstat` утилита командной строки, выводящая на дисплей состояние TCP-соединений
`a` отображает все активные соединения и порты TCP и UDP, через которые прослушивается компьютер.
`n` отображает активные TCP-соединения, однако адреса и номера портов выражаются численно, и не предпринимается никаких попыток определить имена.
`t` отображает только TCP-соединения.

## Part 9. Установка и использование утилит **top**, **htop**

- Установить и запустить утилиты top и htop.

![af](../screenshots/part9_1.PNG)

![ag](../screenshots/part9_2.PNG)

```bash
sudo apt install top
sudo apt install htop
top
htop
```

- uptime

![ah](../screenshots/part9_3.PNG)

```bash
top
```

- количество авторизованных пользователей

![ai](../screenshots/part9_4.PNG)

```bash
top
```

- общую загрузку системы

![aj](../screenshots/part9_5.PNG)

```bash
top
```
- общее количество процессов

![ak](../screenshots/part9_6.PNG)

```bash
top
```

- загрузку cpu

![al](../screenshots/part9_7.PNG)
```bash
top
```

- загрузку памяти

![am](../screenshots/part9_8.PNG)
```bash
top
```

- pid процесса занимающего больше всего памяти

![an](../screenshots/part9_9.PNG)

```bash
top
```

- pid процесса, занимающего больше всего процессорного времени

![ao](../screenshots/part9_10.PNG)

```bash
top
```

- отсортированному по PID, PERCENT_CPU, PERCENT_MEM, TIME

![ap](../screenshots/part9_11.PNG)
`PID`
![aq](../screenshots/part9_12.PNG)
`PERCENT_CPU`
![ar](../screenshots/part9_13.PNG)
`PERCENT_MEM`
![as](../screenshots/part9_14.PNG)
`TIME`

```bash
htop
```

- отфильтрованному для процесса sshd

![at](../screenshots/part9_15.PNG)

```bash
htop
```

- с процессом syslog, найденным, используя поиск

![au](../screenshots/part9_16.PNG)

```bash
htop
```

- с добавленным выводом hostname, clock и uptime

![av](../screenshots/part9_17.PNG)

```bash
htop
```

## Part 10. Использование утилиты **fdisk**

- Запустить команду fdisk -l.

![aw](../screenshots/part10_1.PNG)

```bash
fdisk -l
free -h
```

Название: `/dev/sda`
Размер: `4.15Gib`
Количество секторов: `8687304`
Размер swap: `0`

## Part 11. Использование утилиты **df** 

- Запустить команду df.  

![aw](../screenshots/part11_1.PNG)

```bash
df
```

- размер раздела

![ax](../screenshots/part11_2.PNG)

```bash
df /
```

- размер занятого пространства

![ay](../screenshots/part11_3.PNG)

```bash
df /
```

- размер свободного пространства

![az](../screenshots/part11_4.PNG)

```bash
df /
```

- процент использования

![ba](../screenshots/part11_5.PNG)

```bash
df /
```

- Определить и написать в отчёт единицу измерения в выводе. 

![bb](../screenshots/part11_6.PNG)

Единица измерения: `Kb`

```bash
df /
```

- Запустить команду df -Th.

![bc](../screenshots/part11_7.PNG)

```bash
df -Th
```

- размер раздела

![bd](../screenshots/part11_8.PNG)

```bash
df -Th /
```

- размер занятого пространства

![be](../screenshots/part11_9.PNG)

```bash
df -Th /
```

- размер свободного пространства

![bf](../screenshots/part11_10.PNG)

```bash
df -Th /
```

- процент использования

![bg](../screenshots/part11_11.PNG)

```bash
df -Th /
```

- Определить и написать в отчёт тип файловой системы для раздела.

![bh](../screenshots/part11_12.PNG)

```bash
df -Th /
```

Тип файловой системы для раздела: `ext4`

## Part 12. Использование утилиты **du**

- Запустить команду du.

![bh](../screenshots/part12_1.PNG)

```bash
du
```

- Вывести размер папок /home, /var, /var/log (в байтах, в человекочитаемом виде)

![bh](../screenshots/part12_2.PNG)

```bash
sudo du -sh -b /home
sudo du -sh -b /var
sudo du -sh -b /var/log
```

- Вывести размер всего содержимого в /var/log (не общее, а каждого вложенного элемента, используя *)

![bh](../screenshots/part12_3.PNG)

```bash
sudo du -sh -b * /var/log
```

## Part 13. Установка и использование утилиты **ncdu**

- Установить утилиту ncdu.

![bi](../screenshots/part13_1.PNG)

```bash
sudo apt install ncdu
ncdu
```

- Вывести размер папок /home, /var, /var/log.

![bj](../screenshots/part13_1.PNG)

```bash
sudo apt install ncdu
ncdu
```

![bk](../screenshots/part13_2.PNG)

`home`

```bash
sudo apt install ncdu
ncdu
```

![bl](../screenshots/part13_3.PNG)

`var`

```bash
sudo apt install ncdu
ncdu
```

![bm](../screenshots/part13_4.PNG)

`var/log`

```bash
sudo apt install ncdu
ncdu
```

## Part 14. Работа с системными журналами

- Открыть для просмотра `/var/log/dmesg`

![bn](../screenshots/part14_1.PNG)

```bash
cat /var/log/dmesg
```

- Открыть для просмотра `/var/log/syslog`

![bo](../screenshots/part14_2.PNG)

```bash
cat /var/log/syslog
```

- Открыть для просмотра `/var/log/auth.log`

![bp](../screenshots/part14_3.PNG)

```bash
cat /var/log/auth.log
```

![bq](../screenshots/part14_4.PNG)

```bash
cat /var/log/auth.log
```
`time: 00:36:58`
`user: argoniaz`
`method: LOGIN`

![br](../screenshots/part14_5.PNG)

```bash
sudo systemctl restart ssh
```

![bs](../screenshots/part14_6.PNG)

```bash
cat /var/log/syslog
```

## Part 15. Использование планировщика заданий **CRON**

- Используя планировщик заданий, запустите команду uptime через каждые 2 минуты.


![bt](../screenshots/part15_1.PNG)

```bash
crontab -e
```

![bw](../screenshots/part15_2.PNG)

```bash
cat /var/log/syslog
```

- Удалите все задания из планировщика заданий.

![bx](../screenshots/part15_3.PNG)

```bash
crontab -l
crontab -r
crontab -l
```




















