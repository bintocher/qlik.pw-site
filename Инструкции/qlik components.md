# Qlikview-Components. Фреймворк для разработчиков

## Предыстория
Совсем недавно, наверно в течении последних 3-4 недель я познакомился с замечательным фреймворком для Qlik разработки и теперь "не могу просто без него жить".
Это первая, ознакомительная часть, с функциями которые лично я использую уже сейчас. Да, я еще не со всеми функциями разобрался, но вместе мы их осилим.

Забегая немного вперед, скажу, я тоже внёс свою лепту в разработку этих функций и добавил новую: [Qvc.DropTable()](https://github.com/RobWunderlich/Qlikview-Components/pull/82);

А также, если у вас есть функции которые как вы считаете будут полезны и захотите добавить их в эти компоненты - пожалуйста, лично я буду очень рад. Я вообще по своей сути являюсь сторонником всеобщей популяризации Qlik, в том числе это относится к open-source решениям.

## Что это и зачем? И почему это нужно всем
Основным разработчиком данных компонент является Rob Wunderlich ([Профиль на Github.com][2])

[Qlik компоненты][3] разложены по полочкам в отдельные функции и каждая содержится в отдельном [qvs-файле][1]. Первый релиз состоялся в 2015 году и с тех пор периодически пополняется. Есть небольшие баджи(шилды, иконки, кому как нравится) с текущим состоянием. На момент написания статьи количество скачиваний уже перевалило за 30к раз.

[![GitHub version](https://img.shields.io/github/release/RobWunderlich/Qlikview-Components.svg)](https://github.com/RobWunderlich/Qlikview-Components/releases/latest)
[![GitHub Release Date](https://img.shields.io/github/release-date/RobWunderlich/Qlikview-Components.svg)](https://github.com/RobWunderlich/Qlikview-Components/releases/latest)
[![GitHub download](https://img.shields.io/github/downloads/RobWunderlich/Qlikview-Components/total.svg)](https://github.com/RobWunderlich/Qlikview-Components/releases/latest)
[![GitHub stars](https://img.shields.io/github/stars/RobWunderlich/Qlikview-Components.svg)](https://github.com/RobWunderlich/Qlikview-Components/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/RobWunderlich/Qlikview-Components.svg)](https://github.com/RobWunderlich/Qlikview-Components/issues)
[![GitHub contributors](https://img.shields.io/github/contributors/RobWunderlich/Qlikview-Components.svg)](https://github.com/RobWunderlich/Qlikview-Components/graphs/contributors)
[![GitHub license](https://img.shields.io/github/license/RobWunderlich/Qlikview-Components.svg)](https://github.com/RobWunderlich/Qlikview-Components/blob/master/LICENSE)

На данный момент список функций обширен, и состоит из файлов: Qvc_AsOfTable.qvs ; Qvc_Calendar.qvs ; Qvc_Cleanup.qvs ; Qvc_ColorTheme.qvs ; Qvc_DataLineage.qvs ; Qvc_DbExtract.qvs ; Qvc_DropTable.qvs ; Qvc_EmptyQvd.qvs ; Qvc_Error.qvs ; Qvc_ExpandInterval.qvs ; Qvc_ExportModel.qvs ; Qvc_ExportTables.qvs ; Qvc_Global.qvs ; Qvc_Icons.qvs ; Qvc_Incremental_Reload.qvs ; Qvc_InspectTable.qvs ; Qvc_JoinGenericTables.qvs ; Qvc_LinkTable.qvs ; Qvc_ListDirectories.qvs ; Qvc_ListFiles.qvs ; Qvc_Log.qvs ; Qvc_PopulateVariables.qvs ; Qvc_QvcAvailableUpdate.qvs ; Qvc_SegmentedStore.qvs ; Qvc_TableStats.qvs ; Qvc_Utility.qvs ; Qvc_WildMap.qvs ; 

Так же есть и другие функции которые используются внутри этих, и часть из них можете использовать и вы.

Сегодня я бы хотел остановится и разобрать несколько из них, и вообще, описать как же начать ими пользоваться.

## Как начать пользоваться?

Самый простой вариант, это сделать Include= прямо из интернета. Например так:

```
    $(Must_Include='https://raw.githubusercontent.com/RobWunderlich/Qlikview-Components/master/Qvc_Runtime/Qvc.qvs');
```
Напомню, что отличия Must_Include от Include состоит в том, что в первом случае, если файл не будет найден - скрипт покажет ошибку и прекратит дальнейшее выполнение. Во втором случае - он просто пропустит это и продолжит выполнение дальше.

Итак, мы сделали инклуд компонент, давайте теперь разберем пару функций.

### Qvc.DropTable
Эту функцию я добавил в общие компоненты в ввиду того что устал постоянно делать проверки на наличие тех или иных таблиц в памяти, в момент выполнения скриптов.

Что делает функция - просто удаляет таблицу из памяти.

Синтаксис использования:
```
CALL Qvc.DropTable('Имя таблицы в одинарных кавычках');
```
Что под капотом? Производится проверка имеет ли предоставленная в функцию таблица - номер, если да, то таблица дропается, если нет - то ничего не происходит
```
SUB Qvc.DropTable (_qvctemp.tablename)
/**
@source Qvc_DropTable.qvs
Drop table if it exists.
Not need to make any checks before drop table.
- if noofrows('tab') > 0 then drop end if 
- if tablenumber('tab') > 0 then drop end if
- etc...
    
@syntax CALL Qvc.DropTable('tablename');

@param 1 String. The table name
*/
// Check table exists
IF NOT ISNULL (TableNumber('$(_qvctemp.tablename)')) THEN
	// If not null then table exist, drop it
	DROP TABLE '$(_qvctemp.tablename)';
END IF

SET _qvctemp.tablename=; //Delete variable
	
END SUB
```

### Qvc_Cleanup
Эту функцию нужно использовать всегда, если вы подключаете qlik-компоненты, иначе у вас останется куча ненужных переменных

Еще одна главная особенность этой функции в том, что она возвращает в переменную **Qvc.Global.v.ScriptDuration** значение о продолжительности выполнения скрипта.

На итого, я бы рекомендовал использовать include компонентов... затем ваш скрипт и в конце выполнение этой функции.

Синтаксис вызова функции:
```
CALL Qvc.Cleanup;
```

### Qvc.Log
Еще одна полезная функция, которая позволяет вести дополнительные логи выполнения скрипта в отдельные файлы.

Эта функция имеет не только параметры на вход, но и дополнительные переменные, которые можно перенастраивать в разных приложениях, чтобы получить именно тот результат который необходим.
Итак, синтаксис:
```
SUB Qvc.Log ('ваше сообщение', _level);
/*
Первый параметр - это само сообщение
Второй - это тип записи, возможные варианты: 'INFO', 'WARNING', 'ERROR'
*/
```

Опциональные переменные, которые вам помогут манипулировать логированием:

Qvc.Log.v.LogTable - строка . Имя таблицы в которой хранятся лог-записи, по умолчанию = 'Qvc.LogTable'.
Qvc.Log.v.LogField - строка . Название поля, которое будет содержать в себе сообщение, по-умолчанию = 'Qvc.LogMessage'.
Qvc.Log.v.LogLevelField - строка. Название поля, которое содержит в себе тип записи (INFO, ERROR, WARNING), по-умолчанию = '$(Qvc.Log.v.LogField)_Level'.
Qvc.Log.v.LogFileName - строка. Название внешнего файла для хранения лога сообщений, по-умолчанию имеет вид: 'НазваниеПриложения_log.txt'. Например, если наш QVW-файл называется : 'Загрузка данных', то имя файла-лога будет таким : 'Загрузка данных_log.txt'
Qvc.Log.v.WriteLogFile - -1/0 (true/false). Если -1, то запись в файл будет производится, если 0 - то не будет, по-умолчанию = 0
Qvc.Log.v.WriteToQvLog - -1/0 (true/false). Если -1, то запись лога будет производится в лог-файл который создает при выполнении сам Qlik, если 0 - то не будет, по-умолчанию = 0
Qvc.Log.v.KeepDays - число. Количество дней, которое указывает на длительность хранения лог-записей. Т.е. если у нас запись была сделана больше чем это значение дней назад, то она будет стёрта. По умолчанию стоит значение = 0, которое означает что при каждом выполнении файл будет перезаписываться.

P.S. Лично я для себя изменил эти параметры в отдельном файле, Include которого делаю после подгрузки Qvc.qvs.

Вообще, я в скором времени опишу про структуру каталогов, которые я использую и почему, но это отдельная статья.

На сим всё, всем happy qlik'inga.


[1]:https://github.com/RobWunderlich/Qlikview-Components/tree/master/QVC_Source
[2]:https://github.com/RobWunderlich
[3]:https://github.com/RobWunderlich/Qlikview-Components