# Теория разработки Qlik. Статья 3. Извлекаем скрипты из QVW и выбираем GIT

Это третья часть Теории разработки Qlik из ...

## Список будущих статей - Теории разработки Qlik

- [x] Статья 1: [Инструменты и как я разрабатываю скрипты, VSCode (и дополнения к нему), Sublime](https://qlik.pw/teorija-razrabotki-qlik-instrumenty-i-kak-2_515/)
- [x] Статья 2: [Разбираем структуру каталогов, волшебные Include](https://qlik.pw/teorija-razrabotki-qlik-razbiraem-struktu_529/)
- [x] Статья 3: [Извлекаем скрипты из QVW и выбираем GIT](https://qlik.pw/teorija-razrabotki-qlik-izvlekaem-skript_535/)
- [ ] Статья 4: Пользовательская документация, автоматическое обновление её на сервере и копаем accesspoint (QV)
- [ ] Статья 5: Пользуемся GIT внутри VSCode и как это отражается на продакшн-сервере QlikView
- [ ] Статья 6: Используем функции из QVC (QlikView-Components), ускорение загрузки данных если есть Where ... 

Если вам показалось что-либо непонятным, неточным, если упущен момент дзен или просто появились вопросы, приглашаю:
* Чат-группа: [https://tele.click/joinchat/IuANZRHjPUf4rvdMF0OgIA](https://tele.click/joinchat/IuANZRHjPUf4rvdMF0OgIA)
* Информационный канал: [https://tele.click/qlikd](https://tele.click/qlikd)
* Можно писать в личку: [https://tele.click/Chernov](https://tele.click/Chernov)

## GIT

Я не хочу особо объяснять, как работать c git (и не спрашивайте почему не mercurial), потому что есть великое множество учебников, в т.ч. видео-уроков по которым вы легко освоите работу с ним. [тык](https://githowto.com/ru), [тык](https://habr.com/ru/post/322424/) или сразу в гугл: [тык](https://www.google.com/search?q=git+уроки)

Но очень надеюсь, что вы уделите минут 30-60 на видео-уроки по гиту, для общего развития - в дальнейшем это вам будет помогать. И да не случись с вами *git reset --hard origin/master*

## Gitlab.com vs Github.com

Я на протяжении нескольких лет использовал [gitlab.com](https://gitlab.com) только по одной причине - потому что у него есть возможность создавать неограниченное количество скрытых репозиториев, но совсем недавно ситуация изменилась, ведь [github.com](https://github.com) тоже предложил эту [услугу](https://github.blog/2019-01-07-new-year-new-github/).

Сейчас, по факту, не имеет значения где вы начнёте собирать ваши скрытые репозитории со скриптами - на **gitlab** или на **github**. Лично я бы порекомендовал всё же **github**, а я скорее всего так и останусь сидеть на **gitlab**. Несмотря на то что экспорт репозиториев и перезаливка их на один из ресурсов займет от силы полчаса вашего времени.

Ах да, безопасность все дела... тогда я бы порекомендовал смотреть в сторону своего [gitlab](https://github.com/gitlabhq/gitlabhq). Я дома для тестов разворачивал его на raspberry pi 3 b+. Но так же можете попробовать например и [Gitea](https://gitea.io/en-us/)(написан на Go) или [Gogs](https://gogs.io/)(так же написан на Go), есть и другие: gitblit, kallithea, phabricator и т.д., оставлю это на ваш выбор.

От себя скажу, очень и очень большой плюс в локальном git - это хранение файлов неограниченных размеров (ну или тем объёмом, которое предоставят сис. админы на дисковой подсистеме). Будь моя воля - я бы хранил в git терабайт 50, с историей)) шутка, нет конечно! В общем, remote или local - решайте сами.

И еще, **github** - сейчас является частью Microsoft, а к ним у меня нелюбовь с детства, но к сожалению, приходится мирится. Но как только Qlik будет на *nix-системах, я сразу перепрыгну туда без оглядки (debian, centos, redhat, ubuntu, arch, mint - без разницы) - главное сбежать от MS. В то же время **gitlab** - разработан парой Украинских парней, и у меня нет оснований им не доверять, тем более что он open-source для своего сервера.


## Извлекаем скрипты из QVW через VBS
Когда я только начал переходить на другой принцип разработки, мне понадобился этот скрипт.

Если вы читали прошлую статью, то в ней я описывал принципы хранения скриптов в отдельном каталоге, но как на этот метод перейти если у вас уже созданы десятки/сотни приложений... есть выход!

Для начала я напишу скрипт, а потом мы его разложим по полочкам.

Файл обязательно должен быть сохранен в формате UTF-8. Формат UTF-8 with BOM - будет выдавать ошибку!

### Скрипт файла VBS

```vbs
Dim objShell
Dim objStartFolder
Dim script_path
Dim script_path_bkp
Dim objFolder
Dim colFiles

Set objShell = WScript.CreateObject( "WScript.Shell" )
Set objFSO = CreateObject("Scripting.FileSystemObject")

'get vbscript folder
objStartFolder = objFSO.GetParentFolderName(wscript.ScriptFullName) 
Set objFolder = objFSO.GetFolder(objStartFolder)

'create a folder to place extracted qlikview scripts
script_path_bkp = objStartFolder & "\backup_scripts_qlikview"
If Not objFSO.FolderExists(script_path_bkp) Then
    objFSO.CreateFolder script_path_bkp
End If

'get files in folder
Set colFiles = objFolder.Files

For Each objFile in colFiles
    
    'check if file is qlikview extension
    if UCase(Right(objFile.Name, 3)) = "QVW" then

        script_path = Left(objFile.Path, Len(objFile.Path) - 4) & "-prj"
        
        'create -prj folder
        If Not objFSO.FolderExists(script_path) Then
            objFSO.CreateFolder script_path
        End If
    
        'open and saves the qlikview document
        Set MyApp = CreateObject("QlikTech.QlikView")
        Set MyDoc = MyApp.OpenDocEx(objFile.Path, 2 , false , "" , "", "" , false)
        WScript.Sleep 2000
        MyDoc.Save
        WScript.Sleep 2000

        'extract script file form -prj folder
        If objFSO.FileExists(script_path & "\LoadScript.txt") Then 
            objFSO.CopyFile script_path & "\LoadScript.txt", script_path_bkp & "\" & left(objFile.Name, len(objFile.Name) - 4) & ".qvs", True
        End If 

        MyDoc.GetApplication.Quit
        
        'remove -prj folder
        If objFSO.FolderExists(script_path) Then  
            objFSO.DeleteFolder script_path
        End If 

    end if
Next

WScript.Echo "End of Backup, check folder: " & script_path_bkp
```

### Разбор строк VBS-скрипта

Определяем названия используемых перменных
``` vbs
Dim objStartFolder
Dim script_path
Dim script_path_bkp
Dim objFolder
Dim colFiles
```

Создаем объект для работы с файловой системой
``` vbs
Set objFSO = CreateObject("Scripting.FileSystemObject")
```

Получаем текущий каталог и создаем переменную для работы с ним
``` vbs
objStartFolder = objFSO.GetParentFolderName(wscript.ScriptFullName) 
Set objFolder = objFSO.GetFolder(objStartFolder)
```

Создаем каталог для хранения файлов-скриптов, если его не существует
``` vbs
script_path_bkp = objStartFolder & "\backup_scripts_qlikview"
If Not objFSO.FolderExists(script_path_bkp) Then
    objFSO.CreateFolder script_path_bkp
End If
```

Получаем список имен всех файлов в нашем каталоге и запускаем по ним цикл
``` vbs
Set colFiles = objFolder.Files
For Each objFile in colFiles
```

Если расширение файла = 'QVW' тогда....
``` vbs
if UCase(Right(objFile.Name, 3)) = "QVW" then
```

Определяем название для нового каталога от имени текущего файла, проверяем есть ли такой каталог, и создаем его в случае отсутствия
``` vbs
script_path = Left(objFile.Path, Len(objFile.Path) - 4) & "-prj"

If Not objFSO.FolderExists(script_path) Then
    objFSO.CreateFolder script_path
End If
```
Создаем новый объект операционной системы - QlikView, и открываем его с параметрами.

Доступные параметры для функции OpenDocEx: 
* 1 - DocName
* 2 - AccessMode
* 3 - Allow dialog
* 4 - Username
* 5 - Password
* 6 - Serial
* 7 - NoData
* 8 - UseDocStartDate

``` vbs
Set MyApp = CreateObject("QlikTech.QlikView")
Set MyDoc = MyApp.OpenDocEx(objFile.Path, 2 , false , "" , "", "" , false)
```

Ждём 2 секунды, сохраняем документ, ждём 2 секунды
``` vbs
WScript.Sleep 2000
MyDoc.Save
WScript.Sleep 2000
```

Проверяем, создался ли скрипт приложения, если да, то сохраняем его в наш каталог для хранения скриптов.

Имя создаваемого файла будет = [имя приложения].qvs
``` vbs
If objFSO.FileExists(script_path & "\LoadScript.txt") Then 
    objFSO.CopyFile script_path & "\LoadScript.txt", script_path_bkp & "\" & left(objFile.Name, len(objFile.Name) - 4) & ".qvs", True
End If 
```
Закрываем программу

``` vbs
MyDoc.GetApplication.Quit
```
Удаляем каталог приложения
``` vbs
If objFSO.FolderExists(script_path) Then  
    objFSO.DeleteFolder script_path
End If 
```
Конец условия на проверку расширения имени файла и продолжение цикла по файлам
``` vbs
    end if
Next
```
Выводим сообщение о том, что стоит проверить каталог скриптов, и полный путь к этому каталогу
``` vbs
WScript.Echo "End of Backup, check folder: " & script_path_bkp
```


## Как использовать скрипт

Помещаем вышеуказанный текст в "любое_имя".vbs, затем этот файл закидываем в каталог с вашими QVW файлами и запускаем.

Если не изменять начальные параметры, то у вас будет создан каталог *backup_scripts_qlikview* в котором и будут находится все qvs-файлы из ваших приложений.

## Как изменить скрипты во всех QVW на один? Include=...

Как вы помните, я рекомендовал поместить все qvs файлы в один каталог - как это сделать я объяснил чуть выше. - Но как сделать чтобы у нас при этом заменились текущие скрипты в приложениях на новые?

Я попробовал доделать vbs-скрипт, но у меня, скажем честно, через раз получалось это...

Как мне кажется, даже если бы я доработал скрипт, и выложил бы его здесь - я бы не рекомендовал его использовать "здесь и сейчас", т.к. могут вылезти любые конфликты, которые не очень скажутся на вашем PRODUCTION.

Посему, я рекомендую, вручную изменять только нужные вам приложения, дабы убедится в том, что всё это действительно работает.

## Заключение 

Всем Qlik