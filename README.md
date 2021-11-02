# devops-netology

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