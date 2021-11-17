# devops-netology

Доработка девятого вопроса

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