# Git 

## Что такое git

 **Git** (читается как «гит») — это система контроля версий, которая помогает отслеживать историю изменений в файлах. Git используют программисты для совместной работы над проектами.

 ## Подготовка репозитория 

 Обычно вы получаете репозиторий Git одним из двух способов:

1. Вы можете взять локальный каталог, который в настоящее время не находится под версионным контролем, и превратить его в репозиторий Git, либо

2. Вы можете клонировать существующий репозиторий Git из любого места.

В обоих случаях вы получите готовый к работе Git репозиторий на вашем компьютере.

    Создание репозитория в существующем каталоге
Если у вас уже есть проект в каталоге, который не находится под версионным контролем Git, то для начала нужно перейти в него. Если вы не делали этого раньше, то для разных операционных систем это выглядит по-разному:
 
 для Windows:

    $ cd C:/Users/user/my_project
    
     а затем выполните команду:

$ git init

## Создание «сохранений» 

(тут и add, и status, и commit)
 
## конфликтик 


## Перемещение между сохранениями 

(обязательно про возврат в конце на конец ветки master)

Команда git checkout позволяет переключаться между последними коммитами (если упрощенно) веток:

checkout some-other-branch

Создаёт ветку, в которую и произойдет переключение:

git checkout -b some-other-new-branch

Если в текущей ветке были какие-то изменения по сравнению с последним коммитом в ветке(HEAD), то команда откажется производить переключение, дабы не потерять произведенную работу. Проигнорировать этот факт позволяет ключ -f:

git checkout -f some-other-branch

В случае, когда изменения надо все же сохранить, следует использовать ключ -m. Тогда команда перед переключением попробует залить изменения в текущую ветку и, после разрешения возможных конфликтов переключиться в новую:

git checkout -m some-other-branch

Вернуть файл (или просто вытащить из прошлого коммита) позволяет команда вида:

git checkout somefile

Возвращает somefile к состоянию последнего коммита:

git checkout somefile

Возвращает somefile к состоянию на два коммита назад по ветке:

git checkout HEAD~2 somefile

git merge — слияние веток

## Журнал изменений (git log)

Иногда требуется получить информацию об истории коммитов; коммитах, изменивших отдельный файл; коммитах за определенный отрезок времени и так далее. Для этих целей используется команда git log.

Простейший пример использования, в котором приводится короткая справка по всем коммитам, коснувшимся активной в настоящий момент ветки (о ветках и ветвлении подробно узнать можно ниже, в разделе «Ветвления и слияния»):

git log

Получает подробную информацию о каждом в виде патчей по файлам из коммитов можно, добавив ключ 

-p (или -u):

git log -p

Статистика изменения файлов, вроде числа измененных файлов, внесенных в них строк, удаленных файлов вызывается ключом --stat:

git log --stat

За информацию по созданиям, переименованиям и правам доступа файлов отвечает ключ --summary:

git log --summary

Чтобы просмотреть историю отдельного файла, достаточно указать в виде параметра его имя (кстати, в моей старой версии git этот способ не срабатывает, обязательно добавлять " — " перед «README»):

git log README

или, если версия git не совсем свежая:

git log — README

Далее приводится только более современный вариант синтаксиса. Возможно указывать время, начиная в определенного момента («weeks», «days», «hours», «s» и так далее):

git log --since=«1 day 2 hours» README

git log --since=«2 hours» README

изменения, касающиеся отдельной папки:

git log --since=«2 hours» dir/

Можно отталкиваться от тегов.
Все коммиты, начиная с тега v1:

git log v1...

Все коммиты, включающие изменения файла README, начиная с тега v1:

git log v1... README

Все коммиты, включающие изменения файла README, начиная с тега v1 и заканчивая тегом v2:

git log v1..v2 README

Интересные возможности по формату вывода команды предоставляет ключ --pretty.
Выводит на каждый из коммитов по строчке, состоящей из хэша (здесь — уникального идентификатора каждого коммита, подробней — дальше):

git log --pretty=oneline

Лаконичная информация о коммитах, приводятся только автор и комментарий:

git log --pretty=short

Более полная информация о коммитах, с именем автора, комментарием, датой создания и внесения коммита:

git log --pretty=full/fuller

В принципе, формат вывода можно определить самостоятельно:

git log --pretty=format:'FORMAT'

Определение формата можно поискать в разделе по 
git log из Git Community Book или справке. Красивый ASCII-граф коммитов выводится с использованием ключа --graph.

## Ветки в git

Работа с ветками — очень легкая процедура в git, все необходимые механизмы сконцентрированы в одной команде.

Просто перечисляет существующие ветки, отметив активную:

git branch

Создаёт новую ветку new-branch:

git branch new-branch

Переименовывает ветку:

git branch -m new-name-branch

Показывывает те ветки, среди предков которых есть определенный коммит:

git branch --contains v1.2

Показывает коммит ответвления ветки 

new-name-branch от ветки master:

git merge-base master new-name-branch

## Слияние веток и решение конфликтов

Слияние веток, в отличие от обычной практики централизованных систем, в git происходит практически каждый день. Естественно, что имеется удобный интерфейс к популярной операции.

Пытается объединить текующую ветку и ветку 
new-feature:

git merge new-feature

В случае возникновения конфликтов коммита не происходит, а по проблемным файлам расставляются специальные метки а-ля svn; сами же файлы отмечаются в индексе как «не соединенные» (unmerged). До тех пор пока проблемы не будут решены, коммит совершить будет нельзя.
Например, конфликт возник в файле TROUBLE, что можно увидеть в git status.

Произошла неудачная попытка слияния:

git merge experiment

Смотрим на проблемные места:

git status

Разрешаем проблемы:

edit TROUBLE

Индексируем наши изменения, тем самым снимая метки:

git add .

Совершаем коммит слияния:

git commit

Вот и все, ничего сложного. Если в процессе 
разрешения вы передумали разрешать конфликт, достаточно набрать (это вернёт обе ветки в исходные состояния):

git reset --hard HEAD

Если же коммит слияния был совершен, используем команду:

git reset --hard ORIG_HEAD

## Удаление веток конфликт
 
 Удаляет ветку, если та была залита (merged) с разрешением возможных конфликтов в текущую:

git branch -d new-branch

Удаляет ветку в любом случае:

git branch -D new-branch

## работа с ветками 

### просмотр списка веток 
 Для того что бы просмотреть список веток нужно использовать команду git branch 
 
 


