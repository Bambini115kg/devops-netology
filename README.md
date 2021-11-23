# devops-netology

Домашнее задание по лекции "Файловые системы"

2.
hardlink это ссылка на тот же самый файл и имеет тот же inode то права будут одни и теже

3.
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk
sdc                    8:32   0  2.5G  0 disk

4. 
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
└─sdb2                 8:18   0  511M  0 part
sdc                    8:32   0  2.5G  0 disk

5. 
vagrant@vagrant:~$ sudo sfdisk -d /dev/sdb | sudo sfdisk /dev/sdc
Checking that no-one is using this disk right now ... OK

Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Created a new DOS disklabel with disk identifier 0x7028bfd6.
/dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
/dev/sdc2: Created a new partition 2 of type 'Linux' and of size 511 MiB.
/dev/sdc3: Done.

New situation:
Disklabel type: dos
Disk identifier: 0x7028bfd6

Device     Boot   Start     End Sectors  Size Id Type
/dev/sdc1          2048 4196351 4194304    2G 83 Linux
/dev/sdc2       4196352 5242879 1046528  511M 83 Linux

The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

6-7 вопрос
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
│ └─md0                9:0    0    2G  0 raid1
└─sdb2                 8:18   0  511M  0 part
  └─md1                9:1    0 1018M  0 raid0
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
│ └─md0                9:0    0    2G  0 raid1
└─sdc2                 8:34   0  511M  0 part
  └─md1                9:1    0 1018M  0 raid0

8.
vagrant@vagrant:~$ sudo pvcreate /dev/md1 /dev/md0
  Physical volume "/dev/md1" successfully created.
  Physical volume "/dev/md0" successfully created.

9.
vagrant@vagrant:~$ sudo vgcreate vg1 /dev/md1 /dev/md0
  Volume group "vg1" successfully created

10.
vagrant@vagrant:~$ sudo lvcreate -L 100M vg1 /dev/md1
  Logical volume "lvol0" created.

11.
vagrant@vagrant:~$ sudo mkfs.ext4 /dev/vg1/lvol0
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 25600 4k blocks and 25600 inodes

Allocating group tables: done
Writing inode tables: done
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done

12.
vagrant@vagrant:~$ mkdir /tmp/new
vagrant@vagrant:~$ sudo mount /dev/vg1/lvol0 /tmp/new

13.
vagrant@vagrant:~$ sudo wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.
--2021-11-23 09:07:06--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 22474564 (21M) [application/octet-stream]
Saving to: ‘/tmp/new/test.gz.’

/tmp/new/test.gz.             100%[=================================================>]  21.43M  10.1MB/s    in 2.1s

2021-11-23 09:07:09 (10.1 MB/s) - ‘/tmp/new/test.gz.’ saved [22474564/22474564]

14.
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
│ └─md0                9:0    0    2G  0 raid1
└─sdb2                 8:18   0  511M  0 part
  └─md1                9:1    0 1018M  0 raid0
    └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/new
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
│ └─md0                9:0    0    2G  0 raid1
└─sdc2                 8:34   0  511M  0 part
  └─md1                9:1    0 1018M  0 raid0
    └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/new

15. 
vagrant@vagrant:/$ sudo gzip -t /tmp/new/test.gz.
vagrant@vagrant:/$ echo $?
0

16.
vagrant@vagrant:/$ sudo pvmove /dev/md1
  /dev/md1: Moved: 16.00%
  /dev/md1: Moved: 100.00%

17.
vagrant@vagrant:/$ sudo mdadm /dev/md0 --fail /dev/sdb1
mdadm: set /dev/sdb1 faulty in /dev/md0

18.
vagrant@vagrant:/$ sudo mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Tue Nov 23 08:27:00 2021
        Raid Level : raid1
        Array Size : 2094080 (2045.00 MiB 2144.34 MB)
     Used Dev Size : 2094080 (2045.00 MiB 2144.34 MB)
      Raid Devices : 2
     Total Devices : 2
       Persistence : Superblock is persistent

       Update Time : Tue Nov 23 09:47:24 2021
             State : clean, degraded
    Active Devices : 1
   Working Devices : 1
    Failed Devices : 1
     Spare Devices : 0

Consistency Policy : resync

              Name : vagrant:0  (local to host vagrant)
              UUID : 530482a9:b90294ce:24ce65df:1923836f
            Events : 19

    Number   Major   Minor   RaidDevice State
       -       0        0        0      removed
       1       8       33        1      active sync   /dev/sdc1

       0       8       17        -      faulty   /dev/sdb1

19.
vagrant@vagrant:/$ sudo gzip -t /tmp/new/test.gz.
vagrant@vagrant:/$ echo $?
0

Домашнее задание по лекции "Работа в терминале (лекция 2)"

1. CD -внутренняя команда. Это логично, если надо менять указатели на дериктори, то лучше делать это используя внутреннюю команду.
2. Может такая grep 123 test -c ?
3. pstree -c . Родитель systemd
4. Думаю, как то так должно выглядеть ls -l \root 2>/dev/pts/1
5. Создаем файл vim test, записываю строку new line, сохраняю. Потом создаею файл test_out и в качестве ответа на вопрос,
можно выполнить cat <test>test_out
6. Не совсем уверен в ответе, но я запустил два терминала. Посмотрел TTY у каждого (0 и 1 получилось), на нулевом выполнил
echo "Hello, World!" > /dev/pts/1  на первом наблюдал вывод на экран Hello, World!
7. bash 5>&1 - создастся дискриптор 5 и перенаправит его в первый дискриптор. Если выполнить echo netology > /proc/$$/fd/5, то 
получим вывод netology. Не знаю как выразить правильно, но у нас получилось, что пятый дискриптор и есть первый, поэтому у нас такой результат.
8. ls -l /root 9>&2 2>&1 1>&9 |grep denied -c 
9.Выведет переменные окружение. Также можем увидеть список всех наших переменных окружения, используя команды env или printenv
10. 
/proc/<PID>/cmdline - полный путь до исполняемого файла процесса [PID] 
/proc/<PID>/exe - содержит ссылку до файла запущенного для процесса [PID]
11. 4.2
12.Не знаю как точно объяснить, но система считает, что мне уже выделен TTY в первой сессии и зачем мне еще один TTY? Я так понимаю,
можно принудительно запустить еще один TTY с помощью -t (ssh -t localhost 'tty')
13. Не знаю, отвечать на вопрос или нет (в задании вроде как не надо), но алгоритм такой: 
kill -STOP <PID>
disown (имя процесса)
screen
reptyr $ (pgrep имя процесса)
14. Команда tee в Linux считывает стандартный ввод и записывает его одновременно в стандартный вывод и в 
один или несколько подготовленных файлов. В команде echo string | sudo tee /root/new_file , мы пытаемся сделать вывод в каталог
администратора, что требует определенные права, которые нам даёт команда sudo


Доработка девятого вопроса

Я так и знал, что ошибка закралась не только у меня.
Вы писали, что ошибка в Задании 9, а в девятом про фигурныве скобки речь и я
прям нормально покапался в мануалах и гугле, даже возможный вариант проверки файла на наличие написал, ибо
еще прочитал про find )))

Так вот, вы хотели сказать Задание 11.
Я посмотрел вашу подсказку + man и выяснил, что такая конструкция возвращает true ,если каталог (tmp)
существует. Двйоные скобки - это улучшенная версия одинарных и позволяет не использовать кавычки.
Верно?


1.На 257 строке, описывается назначение фигурных скобок, они призваны группировать команды.
2.На строке 1091 описано расширение скобок, то, что надо в задании 10.
3.На разных строчках, есть описание того, как с помощью скобок можно экранировать (отделять) имена параметров
4.Скорее всего можно использовать при написании скрипта, например заключить тело цикла в скобки (не уверен, но думаю, так)

По поводу вашего предложения "Посмотрите как проверить существование файла или папки в bash."
Я не знаю, как) Возможно, можно попробовать найти файл через find, например так -  find .-name bash{1..6}.txt

Домашнее задание по лекции "Работа в терминале (лекция 1)"

5. RAM 1024Мб, 2 процессора,SATA 64Gb, VGA 4Mb
6. Поправить строчку v.memory = 1024 в vagrantfile
8.1 HISTFILESIZE  . 721 line
8.2 gnoreboth — заменяет(можно использовать вместо) опции ignorespace и ignoredups в настройках ведения истории команд
9. ОТвет на вопрос не нашел. Не знаю, что такое "сценарии использования". Комбинация {}, часто встречается в мануале 
(например 257,1091 строка)
10. touch new-file-{1..100001}.txt . Не уверен, но может не дать создать 300000 файлов из-за ограничений некого стека, который надо увеличить.
11. [[ -d /tmp ]] - не уверен, но должна очищать временную папку
12. 
- создал папку /tmp/new_path_directory
- из папки /usr/bin скопировал файл *bash в созданную папку с помощью команды cp
- вывел на экран содержимое переменной PATH (echo $PATH), скопировал в буфер результат, потом его закинул в блокнот и вписал перед первой 
выводимой строкой (у меня, кстати, выводится /user/bin)  :/tmp/new_path_directory/
-  потом ввёл такую команду export PATH=моя обновленная строка с путями
в итоге, при вводе type -a bash, получилось вот так:
bash is /tmp/new_path_directory/bash
bash is /usr/bin/bash
bash is /bin/bash
13. Команда at позволяет запланировать разовое задание в определенное время, 
а batch запланировать это разовые задачи





Домашнее задание к занятию «2.4. Инструменты Git»

1. commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Thu Jun 18 10:29:58 2020 -0400
    Update CHANGELOG.md
Нвшёл через git log aefead . Первый коммит наш.

2. 85024d310 (tag: v0.12.23) Нашел таким же способом, как в первом вопросе. 

3. 56cd7859e05c36c06b56d013b55a252d0bb7e158
   9ea88f22fc6269854151c571162c5bcf958bee2b
Нашёл подный хеш коммита b8d720, затем воспользовался git show хеш^1. Повторил с цифрами 2,3 и на третьем выдало, что 
неизвестная ревизия, поэтому только два родителя у нашего мержкоммита.
4. 
b14b74c4939dcab573326f4e3ee2a62e23e12f89 [Website] vmc provider links
3f235065b9347a758efadc92295b540ee0a5e26e Update CHANGELOG.md
6ae64e247b332925b872447e9ce869657281c2bf registry: Fix panic when server is unreachable
5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 website: Remove links to the getting started guide's old location
06275647e2b53d97d4f0a19a0fec11f6d69820b5 Update CHANGELOG.md
d5f9411f5108260320064349b757f55c09bc4b80 command: Fix bug when using terraform login on Windows
4b6d06cc5dcb78af637bbb19c198faff37a066ed Update CHANGELOG.md
dd01a35078f040ca984cdd349f18d0b67e486c35 Update CHANGELOG.md

Воспользовался git log --pretty=format:"%H %s" v0.12.23..v0.12.24
5.
commit 8c928e83589d90a031f811fae52a81be7153e82f
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Thu Apr 2 18:04:39 2020 -0700
 Нашёл файл, в которой была искомая функция
 с помощью git grep --function-context --heading "func providerSource" , потом нашёл все коммиты с этим файлом,
посмотрел самый нижний в git, увидел функцию, значит оно.
Но потом вопсользовался вот такой конструкцией  git log -SproviderSource'(' --oneline и там появился еще один коммит
18dd1bb4d6134b9f8e15b9cea7fae0c878d0a8ba
Думаю, что первый коммит, как ответ на вопрос, правильный. 

6. 
35a058fb3 main: configure credentials from the CLI config file
c0b176109 prevent log output during init
8364383c3 Push plugin discovery down into command package

Тут просто git log -SglobalPluginDirs --oneline

7. Martin Atkins - автор

нашел все коммиты - git log -SsynchronizedWriters --oneline
взял первый и посмотрел через git show 

	













Задание №1 – Ветвление, merge и rebase.

1. Создал каталог branching в main mkdir branching
2. Создал два файла в этом катлоге vim merge.sh, vim rebase.sh
3. Создал коммит git commit -m "prepare for merge and rebase"
4. Создал ветку git switch git-merge
5. Отредактировал файл merge.sh, создал коммит с описанием "merge: @ instead *" и запушил всё в репозиторий
6. Ещё раз отредактировал файл merge.sh, создал коммитс описанием "merge: use shift" и запушил всё в репозиторий
7. Вернулся в main
8. Изменил rebase.sh, закоммител с описанием "echo =====" и запушил в репозиторий git push origin main
9. По условиям задачи нашёл нужный коммит, переключился на него и сделал ветку git-rebase
10. Изменил rebase.sh по образцу , закоммител с описанием "git-rebase 1", запушил в репозиторий
11. Поменял строку "Parameter: $param" на "Next parameter: $param" в файле rebase.sh, закоммител с описанием "git-rebase 2", запушил в репозиторий
12. Перешел в ветку main, выполнил git merge git-merge
13. Перешел в ветку git-rebase, выполнил git rebase -i, в открывшемся редакторе добавил fixup слева от последнего коммита, сохранил и вышел
14. Получил ошибку, зашёл в редактирование файла rebase.sh, оставил нужные строки, сохранил и вышел
15. Выполнил git add rebase.sh и git rebase --continue
16. Получил ошибку, зашёл в редактирование файла rebase.sh, оставил нужные строки, сохранил и вышел
17. Выполнил git add rebase.sh и git rebase --continue
18. Выполнил git push -u origin git-rebase
19. Получил ошибку, выполнил git push -u origin git-rebase -f
20. Перешёл в main и смержил ветку git merge git-rebase




Редактирую файл
fix

1. **/.terraform/* - игнорируем всё во вложенных каталогах выше уровня и файлы, находящиеся в каталогах уровня ниже каталога .terraform
2. *.tfstate - игнорируем все файлы с расширением tfstate в каталоге terraform
3. *.tfstate.* - игнирируем файлы с любым именем и расширением .tfstate.любое второе расширением (например .gz) в каталоге terraform
4. crash.log - игнорируем этот файл в каталоге terraform
5. *.tfvars - игнорируем все файлы с расширением tfvars в каталоге terraform
6. override.tf - игнорируем этот файл в каталоге terraform
7. override.tf.json - игнорируем этот файл в каталоге terraform
8. *_override.tf - игнорируем все файлы с именем начинающиеся на любое количество символволов 
   с нижним подчеркиванием в конце _ , например vasya_override.tf в каталоге terraform
9. *_override.tf.json - игнорируем все файлы с именем начинающиеся на любое количество символволов 
   с нижним подчеркиванием в конце _ , например vasya_override.tf.json в каталоге terraform
10. .terraformrc - игнорировать весь каталог .terraformc
11. terraform.rc - игнорируем этот файл в каталоге terraform