## **task 1:**
В каких состояниях могут находиться файлы в репозитории Git? Чем эти состояния отличаются друг от друга? Какой обычный workflow по переходу из одного состояния в другое?

---
Возможные состояния файла в GIT и их описания:
- Не под версионным контролем:  
  - **Untracked** (неотслеживаемый):  
  любые файлы в рабочем каталоге, которые не входили в последний снимок состояния и не подготовлены к коммиту (новый файл, не был добавлен для отслеживания командой `git add`)   
- Под версионным контролем (отслеживаемые):  
  - **Unmodified** (неизмененный):  
  в ранее закомиченный файл не вносились изменения (любой файл после коммита переходит в состояние *unmodified*)   
  - **Modified** (измененный):  
  отслеживаемый файл (который был ранее *unmodified*) был изменён в рабочем каталоге, но пока не проиндексирован
  - **Staged** (подготовленный к коммиту):  
  проиндексированные файлы, подготовлены к коммиту (`git add` переводит файлы с любым состоянием, кроме *unmodifed*, в состояние *staged*)

Обычный процесс перехода между состояниями (без учета специфических переходов):

- **Untracked** `./file` ->  
  -> `git add file` -> *Staged*
- **Unmodified** `./file` ->  
  -> `echo file edited >> file` -> *Modified*  
  -> `git rm --cached file; git commit -m 'removed file from repo'` -> *Untracked*  
- **Modified** `./file` ->  
  -> `git add file` -> *Staged*  
- **Staged** `./file` ->  
  -> `git commit -m 'staged file commited'` -> *Unmodified*

Источники:  
[Состояния файлов в Git](https://ru.hexlet.io/courses/intro_to_git/lessons/git-status/theory_unit)  
[The entire Pro Git book](https://git-scm.com/book/ru/v2)

## **task 2:**
Как переместить файл из состояния Staged в состояние Modified?

---
Переход *Staged* -> *Modified* осуществляется командой:  
`git reset HEAD file`

<details>
<summary>Пример</summary>
1. *Unmodified* -> *Modified*:  

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ echo foobar > file
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые не в индексе для коммита:
  (используйте «git add <файл>…», чтобы добавить файл в индекс)
  (используйте «git checkout -- <файл>…», чтобы отменить изменения
   в рабочем каталоге)

        изменено:      file

нет изменений добавленных для коммита
(используйте «git add» и/или «git commit -a»)
```

2. *Modified* -> *Staged*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ git add file
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые будут включены в коммит:
  (используйте «git reset HEAD <файл>…», чтобы убрать из индекса)

        изменено:      file

```

3. *Staged* -> *Modified*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ git reset HEAD file
Непроиндексированные изменения после сброса:
M       file
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые не в индексе для коммита:
  (используйте «git add <файл>…», чтобы добавить файл в индекс)
  (используйте «git checkout -- <файл>…», чтобы отменить изменения
   в рабочем каталоге)

        изменено:      file

нет изменений добавленных для коммита
(используйте «git add» и/или «git commit -a»)
```

</details>

## **task 3:**
Как переместить файл из состояния Staged в состояние Untracked?

---
Переход *Staged* -> *Untracked* осуществляется той же командой, что и *Staged* -> *Modified*:  
`git reset HEAD file`

<details>
<summary>Пример</summary>
1. *Void* -> *Untracked*:  

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ git status
На ветке master
нечего коммитить, нет изменений в рабочем каталоге
cbpi@ubuntu-vbox:~/gittest (master)$ > file2
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Неотслеживаемые файлы:
  (используйте «git add <файл>…», чтобы добавить в то, что будет включено в коммит)

        file2

ничего не добавлено в коммит, но есть неотслеживаемые файлы (используйте «git add», чтобы отслеживать их)
```

2. *Untracked* -> *Staged*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ git add file2
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые будут включены в коммит:
  (используйте «git reset HEAD <файл>…», чтобы убрать из индекса)

        новый файл:    file2

```

3. *Staged* -> *Untracked*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ git reset HEAD file2
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Неотслеживаемые файлы:
  (используйте «git add <файл>…», чтобы добавить в то, что будет включено в коммит)

        file2

ничего не добавлено в коммит, но есть неотслеживаемые файлы (используйте «git add», чтобы отслеживать их)
```

</details>

## **task 4:**
Как переместить файл из состояния Modified к предыдущему снимку?

---
Переход *Modified* -> *Unmodified (Last commit)* осуществляется командой:  
`git checkout -- file`

<details>
<summary>Пример</summary>
1. *Unmodified*:  

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ git log -p -1
commit 58f807ed48f3419cb63c6a6c6c180ccefa757435 (HEAD -> master)
Author: a-tikhomirov <andrey.tikhomirov.88@gmail.com>
Date:   Wed Jan 29 21:00:16 2020 +0300

    03 edited file

diff --git a/file b/file
index e69de29..323fae0 100644
--- a/file
+++ b/file
@@ -0,0 +1 @@
+foobar
cbpi@ubuntu-vbox:~/gittest (master)$ cat file
foobar
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
нечего коммитить, нет изменений в рабочем каталоге
```

2. *Unmodified* -> *Modified*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ echo different text > file
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые не в индексе для коммита:
  (используйте «git add <файл>…», чтобы добавить файл в индекс)
  (используйте «git checkout -- <файл>…», чтобы отменить изменения
   в рабочем каталоге)

        изменено:      file

нет изменений добавленных для коммита
(используйте «git add» и/или «git commit -a»)
cbpi@ubuntu-vbox:~/gittest (master)*$ cat file
different text
cbpi@ubuntu-vbox:~/gittest (master)*$ git diff
diff --git a/file b/file
index 323fae0..fd83c83 100644
--- a/file
+++ b/file
@@ -1 +1 @@
-foobar
+different text
```

3. *Modified* -> *Unmodified (Last commit)*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ git checkout -- file
cbpi@ubuntu-vbox:~/gittest (master)$ git status
На ветке master
нечего коммитить, нет изменений в рабочем каталоге
cbpi@ubuntu-vbox:~/gittest (master)$ cat file
foobar
```

</details>

Источники:  
[Lesson 4. Introduction to undoing things in git Intro version control git](https://www.earthdatascience.org/workshops/intro-version-control-git/undoing-things/)

## **task 5:**
Как переместить файл из состояния Commited в состояние Untracked (2 шага)?

---
Переход *Unmodified (commited)* -> *Untracked* осуществляется командами:  
```
git rm --cached file
git commit -m 'removed file from repo'
```

<details>
<summary>Пример</summary>
1. *Unmodified* -> *Deleted && Untracked*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ git ls-files
file
cbpi@ubuntu-vbox:~/gittest (master)$ git status
На ветке master
нечего коммитить, нет изменений в рабочем каталоге
cbpi@ubuntu-vbox:~/gittest (master)$ git rm --cached file
rm 'file'
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые будут включены в коммит:
  (используйте «git reset HEAD <файл>…», чтобы убрать из индекса)

        удалено:       file

Неотслеживаемые файлы:
  (используйте «git add <файл>…», чтобы добавить в то, что будет включено в коммит)

        file

```

2. *Deleted && Untracked* -> *Untracked*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ git commit -m '04 removed "file" from repo'
[master 9b672f4] 04 removed "file" from repo
 1 file changed, 1 deletion(-)
 delete mode 100644 file
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Неотслеживаемые файлы:
  (используйте «git add <файл>…», чтобы добавить в то, что будет включено в коммит)

        file

ничего не добавлено в коммит, но есть неотслеживаемые файлы (используйте «git add», чтобы отслеживать их)
```

</details>

Источники:  
[How can I delete a file from a Git repository](https://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-a-git-repository/2047477)  
[List files in local git repo](https://stackoverflow.com/questions/8533202/list-files-in-local-git-repo)

## **task 6:**
Как переместить файл из состояния Commited в состояние Staged?

---
Для переход *Unmodified (commited)* -> *Staged* есть два варианта:
1. Перевести файл в  *Untracked*, затем в *Staged*:  
```
git rm --cached file
git commit -m 'removed "file" from repo'
git add dile
```

2. Откатить локальный коммит на предыдущий (не рекомендуется) и заново добавить файл:  
```
git reset commit_index
git add file
```

<details>
<summary>Пример</summary>
- Вариант 1:  
1. *Unmodified* -> *Deleted && Untracked*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ git ls-files
file
cbpi@ubuntu-vbox:~/gittest (master)$ git status
На ветке master
нечего коммитить, нет изменений в рабочем каталоге
cbpi@ubuntu-vbox:~/gittest (master)$ git rm --cached file
rm 'file'
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые будут включены в коммит:
  (используйте «git reset HEAD <файл>…», чтобы убрать из индекса)

        удалено:       file

Неотслеживаемые файлы:
  (используйте «git add <файл>…», чтобы добавить в то, что будет включено в коммит)

        file

```

2. *Deleted && Untracked* -> *Untracked*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ git commit -m '06 removed "file" from repo'
[master fe86298] 06 removed "file" from repo
 1 file changed, 1 deletion(-)
 delete mode 100644 file
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Неотслеживаемые файлы:
  (используйте «git add <файл>…», чтобы добавить в то, что будет включено в коммит)

        file

ничего не добавлено в коммит, но есть неотслеживаемые файлы (используйте «git add», чтобы отслеживать их)
```

2. *Untracked* -> *Staged*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ git add file
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые будут включены в коммит:
  (используйте «git reset HEAD <файл>…», чтобы убрать из индекса)

        новый файл:    file

```

- Вариант 2:  
1. *Unmodified* -> *Untracked*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ git status
На ветке master
нечего коммитить, нет изменений в рабочем каталоге
cbpi@ubuntu-vbox:~/gittest (master)$ git log -2
commit aea33684860bc35f290f2207a9eb61e4e74f4739 (HEAD -> master)
Author: a-tikhomirov <andrey.tikhomirov.88@gmail.com>
Date:   Thu Jan 30 01:50:06 2020 +0300

    09 edited "file2"

commit aae8ef3580d2014240c2c07b98a9f9d2a7885e0b
Author: a-tikhomirov <andrey.tikhomirov.88@gmail.com>
Date:   Thu Jan 30 01:38:15 2020 +0300

    08 added "file2"

cbpi@ubuntu-vbox:~/gittest (master)$ git reset aae8ef35
Непроиндексированные изменения после сброса:
M       file2
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые не в индексе для коммита:
  (используйте «git add <файл>…», чтобы добавить файл в индекс)
  (используйте «git checkout -- <файл>…», чтобы отменить изменения
   в рабочем каталоге)

        изменено:      file2

нет изменений добавленных для коммита
(используйте «git add» и/или «git commit -a»)
```

2. *Untracked* -> *Staged*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ git add file2
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые будут включены в коммит:
  (используйте «git reset HEAD <файл>…», чтобы убрать из индекса)

        изменено:      file2

```

</details>

Источники:  
[Remove single file from staging or from commit](https://coderwall.com/p/zpkhyg/remove-single-file-from-staging-or-from-commit)

## **task 7:**
Как переместить файл из состояния Untracked в состояние Staged?

---
Переход *Untracked* -> *Staged* осуществляется командой:  
`git add file`

<details>
<summary>Пример</summary>
*Untracked* -> *Staged*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Неотслеживаемые файлы:
  (используйте «git add <файл>…», чтобы добавить в то, что будет включено в коммит)

        file

ничего не добавлено в коммит, но есть неотслеживаемые файлы (используйте «git add», чтобы отслеживать их)
cbpi@ubuntu-vbox:~/gittest (master)*$ git add file
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые будут включены в коммит:
  (используйте «git reset HEAD <файл>…», чтобы убрать из индекса)

        новый файл:    file

```

</details>

## **task 8:**
Как переместить файл из состояния Modified в состояние Commited напрямую?

---
Переход *Modified* -> *Commited* напрямую осуществляется командой:  
`git commit -am 'stage and commit'`

<details>
<summary>Пример</summary>
*Modified* -> *Commited* 

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ git status
На ветке master
нечего коммитить, нет изменений в рабочем каталоге
cbpi@ubuntu-vbox:~/gittest (master)$ echo some text >> file2
cbpi@ubuntu-vbox:~/gittest (master)*$ git status
На ветке master
Изменения, которые не в индексе для коммита:
  (используйте «git add <файл>…», чтобы добавить файл в индекс)
  (используйте «git checkout -- <файл>…», чтобы отменить изменения
   в рабочем каталоге)

        изменено:      file2

нет изменений добавленных для коммита
(используйте «git add» и/или «git commit -a»)
cbpi@ubuntu-vbox:~/gittest (master)*$ git commit -am '09 edited "file2"'
[master aea3368] 09 edited "file2"
 1 file changed, 1 insertion(+)
cbpi@ubuntu-vbox:~/gittest (master)$ git status
На ветке master
нечего коммитить, нет изменений в рабочем каталоге
cbpi@ubuntu-vbox:~/gittest (master)$ git log -1
commit aea33684860bc35f290f2207a9eb61e4e74f4739 (HEAD -> master)
Author: a-tikhomirov <andrey.tikhomirov.88@gmail.com>
Date:   Thu Jan 30 01:50:06 2020 +0300

    09 edited "file2"
```

</details>

## **task 9:**
Проделать цепочку:

-- В github настроить аутентификацию по ssh ключу. Защитить ветку мастер.  
-- Создать ветку local_feature локально.  
-- Поставить ее остлеживать удаленную ветку remote_feature, запушив изменения.  
-- Cменить отслеживание удаленной ветки remote_feature на удаленную local_feature.  
-- Удалить удаленную ветку remote_feature.  
-- Сделать изменения и запушить в удаленную local_feature. Создать Pull request и смержить в мастер.  
-- Из интерфейса гитхаба удалить удаленную ветку local_feature  
-- Из консоли удалить локальную ветку local_feature

---
1. Аутентификация по ssh настроена:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ ssh -T git@github.com
Hi a-tikhomirov! You've successfully authenticated, but GitHub does not provide shell access.
cbpi@ubuntu-vbox:~/gittest (master)$ git status
На ветке master
Ваша ветка обновлена в соответствии с «origin/master».

нечего коммитить, нет изменений в рабочем каталоге
cbpi@ubuntu-vbox:~/gittest (master)$ git branch -a
* master
  remotes/origin/master
cbpi@ubuntu-vbox:~/gittest (master)$ git log
commit f003933f8284b5db65656327d14a99f4d717c7ad (HEAD -> master, origin/master)
Author: a-tikhomirov <andrey.tikhomirov.88@gmail.com>
Date:   Thu Jan 30 04:45:07 2020 +0300

    master init commit
```

Ветка мастер защищена: `repo Settings -> Branches -> Add rule -> master; Require pull request reviews before merging -> Create`

2. Создание локальной ветки *local_feature*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ git checkout -b local_feature
Переключено на новую ветку «local_feature»
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git branch -vv
* local_feature f003933 master init commit
  master        f003933 [origin/master] master init commit
```

3. Установка отслеживания удаленной ветки *remote_feature* и `push` изменений:

- Создание удаленной ветки *remote_feature*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git checkout master
Переключено на ветку «master»
Ваша ветка обновлена в соответствии с «origin/master».
cbpi@ubuntu-vbox:~/gittest (master)$ git branch remote_feature
cbpi@ubuntu-vbox:~/gittest (master)$ git push origin remote_feature
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'remote_feature' on GitHub by visiting:
remote:      https://github.com/a-tikhomirov/git-test/pull/new/remote_feature
remote:
To github.com:a-tikhomirov/git-test.git
 * [new branch]      remote_feature -> remote_feature
cbpi@ubuntu-vbox:~/gittest (master)$ git branch -d remote_feature
Ветка remote_feature удалена (была f003933).
cbpi@ubuntu-vbox:~/gittest (master)$ git branch -a
  local_feature
* master
  remotes/origin/master
  remotes/origin/remote_feature
```

- Установка отслеживания и `push`:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ git checkout local_feature
Переключено на ветку «local_feature»
cbpi@ubuntu-vbox:~/gittest (local_feature)$ echo new feature > feature.txt
cbpi@ubuntu-vbox:~/gittest (local_feature)*$ git add feature.txt
cbpi@ubuntu-vbox:~/gittest (local_feature)*$ git commit -m 'local_feature added feature.txt'
[local_feature c9fd9f0] local_feature added feature.txt
 1 file changed, 1 insertion(+)
 create mode 100644 feature.txt
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git push origin HEAD:remote_feature
Подсчет объектов: 3, готово.
Delta compression using up to 4 threads.
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (3/3), 306 bytes | 306.00 KiB/s, готово.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:a-tikhomirov/git-test.git
   f003933..c9fd9f0  HEAD -> remote_feature
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git branch -vv
* local_feature c9fd9f0 [origin/remote_feature] local_feature added feature.txt
  master        f003933 [origin/master] master init commit
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git log -1
commit c9fd9f04ac857a5e1665d8598ceddefbbe5f7c17 (HEAD -> local_feature, origin/remote_feature)
Author: a-tikhomirov <andrey.tikhomirov.88@gmail.com>
Date:   Thu Jan 30 04:56:28 2020 +0300

    local_feature added feature.txt
```

4. Смена отслеживания удаленной ветки *remote_feature* на удаленную *local_feature*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git push --set-upstream origin local_feature
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'local_feature' on GitHub by visiting:
remote:      https://github.com/a-tikhomirov/git-test/pull/new/local_feature
remote:
To github.com:a-tikhomirov/git-test.git
 * [new branch]      local_feature -> local_feature
Ветка «local_feature» отслеживает внешнюю ветку «local_feature» из «origin».
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git branch -vv
* local_feature c9fd9f0 [origin/local_feature] local_feature added feature.txt
  master        f003933 [origin/master] master init commit
```

5. Удаление ветки `remote_feature`:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git branch -a
* local_feature
  master
  remotes/origin/local_feature
  remotes/origin/master
  remotes/origin/remote_feature
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git push origin --delete remote_feature
To github.com:a-tikhomirov/git-test.git
 - [deleted]         remote_feature
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git branch -a
* local_feature
  master
  remotes/origin/local_feature
  remotes/origin/master
```

6. Создание изменений и `push` в удаленную *local_feature*. Создание *Pull request* и `merge` в мастер.

- Создание изменений и `push` в удаленную *local_feature*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (local_feature)$ echo feture update >> feature.txt
cbpi@ubuntu-vbox:~/gittest (local_feature)*$ git commit -am 'local_feature updated feature'
[local_feature 9ae702f] local_feature updated feature
 1 file changed, 1 insertion(+)
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git push
Подсчет объектов: 3, готово.
Delta compression using up to 4 threads.
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (3/3), 318 bytes | 318.00 KiB/s, готово.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:a-tikhomirov/git-test.git
   c9fd9f0..9ae702f  local_feature -> local_feature
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git log -1
commit 9ae702f684f8759341e28ffb33af22827b8cfd73 (HEAD -> local_feature, origin/local_feature)
Author: a-tikhomirov <andrey.tikhomirov.88@gmail.com>
Date:   Thu Jan 30 05:13:29 2020 +0300

    local_feature updated feature
```

- Создание *Pull request* и `merge` в мастер.

Так как ветка мастер была защищена, а *github* не позволяет подтвердить собственный	 *pull request* должным образом - был создан новый аккаунт.
На новый аккаунт было отправлено приглашение в коллабораторы через `repo Settings -> Collaborators  -> Add Collaborator`  
*Pull request* подтвержден и объединен в ветку мастер. Проверка:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (local_feature)$ git checkout master
Переключено на ветку «master»
Ваша ветка обновлена в соответствии с «origin/master».
cbpi@ubuntu-vbox:~/gittest (master)$ git pull
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
Распаковка объектов: 100% (1/1), готово.
Из github.com:a-tikhomirov/git-test
   f003933..c35c562  master     -> origin/master
Обновление f003933..c35c562
Fast-forward
 feature.txt | 2 ++
 1 file changed, 2 insertions(+)
 create mode 100644 feature.txt
cbpi@ubuntu-vbox:~/gittest (master)$ git log -1
commit c35c56266a6b1c7a1ffcb683884a493714a12556 (HEAD -> master, origin/master)
Merge: f003933 9ae702f
Author: cbpi-nameless <60455951+cbpi-nameless@users.noreply.github.com>
Date:   Thu Jan 30 05:29:08 2020 +0300

    Merge pull request #1 from a-tikhomirov/local_feature

    Local feature
```

Ссылка на закрытый `pull request`: [Pull request closed by collaborator](https://github.com/a-tikhomirov/git-test/pull/1)

7. Удаленная ветка *local_feature* через интерфейс гитхаба удалена: [git-test repo branches](https://github.com/a-tikhomirov/git-test/branches)

8. Удаление локальной ветки *local_feature* из консоли.

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ git branch -D local_feature
Ветка local_feature удалена (была 9ae702f).
cbpi@ubuntu-vbox:~/gittest (master)$ git branch -a
* master
  remotes/origin/local_feature
  remotes/origin/master
cbpi@ubuntu-vbox:~/gittest (master)$ git remote prune origin
Очистка origin
URL: git@github.com:a-tikhomirov/git-test.git
 * [удалена] origin/local_feature
cbpi@ubuntu-vbox:~/gittest (master)$ git branch -a
* master
  remotes/origin/master
```

Источники:  
[Make an existing Git branch track a remote branch](https://stackoverflow.com/questions/520650/make-an-existing-git-branch-track-a-remote-branch)  
[Push a new local branch to a remote Git repository and track it too](https://www.freecodecamp.org/forum/t/push-a-new-local-branch-to-a-remote-git-repository-and-track-it-too/13222)  
[How To Set Upstream Branch on Git](https://devconnected.com/how-to-set-upstream-branch-on-git/#Why_are_upstream_branches_so_useful_in_Git)  
[cleaning up old remote git branches](https://stackoverflow.com/questions/3184555/cleaning-up-old-remote-git-branches)

## **task 10:**
Задание выполняется в одиночку (в этом случае нужно два разных аккаунта на github) либо в парах. Нужно создать репозиторий, другим аккаунтом (или другому участнику) сделать его форк, внести изменение, сделать Pull Request. Далее следует смержить Pull Request.

---
Репозиторий создан пользователем *a-tikhomirov*: [Git-test repo](https://github.com/a-tikhomirov/git-test)

1. Пользователем *cbpi-nameless* сделан `fork` и `clone`:

```ShellSession
dev1@ubuntu-vbox:~$ git clone git@github.com:cbpi-nameless/git-test.git
Клонирование в «git-test»…
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 10 (delta 0), reused 9 (delta 0), pack-reused 0
Получение объектов: 100% (10/10), готово.
dev1@ubuntu-vbox:~$ cd git-test/
dev1@ubuntu-vbox:~/git-test$ git status
На ветке master
Ваша ветка обновлена в соответствии с «origin/master».

нечего коммитить, нет изменений в рабочем каталоге
```

2. Отслеживание изменений оригинального репозитория:

```ShellSession
dev1@ubuntu-vbox:~/git-test$ git remote add upstream git@github.com:a-tikhomirov/git-test.git
dev1@ubuntu-vbox:~/git-test$ git fetch
Warning: Permanently added the RSA host key for IP address '140.82.118.4' to the list of known hosts.
```

3. Внесение изменений пользователем *cbpi-nameless*:

```ShellSession
dev1@ubuntu-vbox:~/git-test$ git checkout -b cbpi-feature
Переключено на новую ветку «cbpi-feature»
dev1@ubuntu-vbox:~/git-test$ echo feature from cbpi here >> feature.txt
dev1@ubuntu-vbox:~/git-test$ git add feature.txt
dev1@ubuntu-vbox:~/git-test$ git commit -m 'added my own new feature'
[cbpi-feature 32285bf] added my own new feature
 1 file changed, 1 insertion(+)
dev1@ubuntu-vbox:~/git-test$ git push origin cbpi-feature
Подсчет объектов: 3, готово.
Delta compression using up to 4 threads.
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (3/3), 323 bytes | 323.00 KiB/s, готово.
Total 3 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'cbpi-feature' on GitHub by visiting:
remote:      https://github.com/cbpi-nameless/git-test/pull/new/cbpi-feature
remote:
To github.com:cbpi-nameless/git-test.git
 * [new branch]      cbpi-feature -> cbpi-feature
dev1@ubuntu-vbox:~/git-test$ git log -1
commit 32285bf41271b676bfc841e7cfa0ccb02e048a21 (HEAD -> cbpi-feature, origin/cbpi-feature)
Author: cbpi-nameless <cbpi@mail.ru>
Date:   Thu Jan 30 06:16:11 2020 +0300

    added my own new feature
```

4. Пользователем *cbpi-nameless* сделан `pull request` в `forked` репозитории -> `pull request` появился в оригинальном репозитории.

5. Пользователем *a-tikhomirov* `pull request` был подтвержден и объединен в мастер:

- *cbpi-nameless*:

```ShellSession
dev1@ubuntu-vbox:~/git-test$ git checkout master
Переключено на ветку «master»
Ваша ветка обновлена в соответствии с «origin/master».
dev1@ubuntu-vbox:~/git-test$ git pull
Уже обновлено.
dev1@ubuntu-vbox:~/git-test$ git log -1
commit c35c56266a6b1c7a1ffcb683884a493714a12556 (HEAD -> master, origin/master, origin/HEAD)
Merge: f003933 9ae702f
Author: cbpi-nameless <60455951+cbpi-nameless@users.noreply.github.com>
Date:   Thu Jan 30 05:29:08 2020 +0300

    Merge pull request #1 from a-tikhomirov/local_feature

    Local feature
dev1@ubuntu-vbox:~/git-test$ git push --delete origin cbpi-feature
To github.com:cbpi-nameless/git-test.git
 - [deleted]         cbpi-feature
```

- *a-tikhomirov*:

```ShellSession
cbpi@ubuntu-vbox:~/gittest (master)$ git pull
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 3 (delta 0), pack-reused 0
Распаковка объектов: 100% (4/4), готово.
Из github.com:a-tikhomirov/git-test
   c35c562..77dcba4  master     -> origin/master
Обновление c35c562..77dcba4
Fast-forward
 feature.txt | 1 +
 1 file changed, 1 insertion(+)
cbpi@ubuntu-vbox:~/gittest (master)$ git log -p -2
commit 77dcba4cf8411fda9ee328c6bf4d5e3b6f128cf6 (HEAD -> master, origin/master)
Merge: c35c562 32285bf
Author: a-tikhomirov <57209702+a-tikhomirov@users.noreply.github.com>
Date:   Thu Jan 30 06:20:57 2020 +0300

    Merge pull request #2 from cbpi-nameless/cbpi-feature

    added my own new feature

commit 32285bf41271b676bfc841e7cfa0ccb02e048a21
Author: cbpi-nameless <cbpi@mail.ru>
Date:   Thu Jan 30 06:16:11 2020 +0300

    added my own new feature

diff --git a/feature.txt b/feature.txt
index d524199..fca48c8 100644
--- a/feature.txt
+++ b/feature.txt
@@ -1,2 +1,3 @@
 new feature
 feture update
+feature from cbpi here
```

Ссылка на закрытый `pull request`: [Pull request closed by author](https://github.com/a-tikhomirov/git-test/pull/2)