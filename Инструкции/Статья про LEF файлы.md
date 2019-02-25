# Qlik. Описание LEF-файла, что содержит в себе лицензия?

Что означает TOKENS? Что такое TIMELIMIT? Как считать лицензии?
Возможных вариантов не мало, попробуем разобраться во всех и по мере получения новой информации данная статья будет обновляться.

## Общее описание ключа

Первая строка всегда содержит в себе [Серийный номер] лицензии, и состоит из 16-ти цифр

Последняя строка всегда содержит в себе [ключ], и может состоять из букв англ. алфавита и цифр, имеет 5 секций по 4 символа, разделяются дефисом. Пример: A6AA-B7BB-C3CC-RR1R-0200

Остальные строки отвечают требованиям : Название атрибута ; Значение атрибута ; Дата начала ; Дата окончания. Все значения разделяются точкой с запятой ";"

## Атрибуты в ключах
### PRODUCTLEVEL;

Содержит в себе цифровое значение того продукта, который можно активировать данным LEF-файлом.
Может иметь ограничения по времени, а может и не иметь.
Возможные значения:
3 - Персональная лицензия для QlikView на 1 пользователя ;
6 - Возможно, это персональная лицензия для NPrinting Designer ;
10 - QlikView Server ;
20 - QlikView Expressor ;
30 - QlikView Publisher ;
40 - неизвестно ;
43 - OCX Developer license ;
50 - Qlik Sense ;
55 - неизвестно ;
60 - NPrinting, 16-ой версии ;
65 - NPrinting, 17-ой версии ;
70 - Qlik Core ;
75 - Qlik DataMarket ;
80-85 - SAP коннектор ;
90 - GeoAnalytics ;
91 - GeoAnalytics ;

### NUMBER_OF_XS;
Количество разрешенных QlikView Publisher серверов в одном кластере. 
Число
### UNCAPPED;
Не имеет ограничений по CAL, полный безлимит по пользователям.
No CAL limitation
YES/NO
### TIMELIMIT;
Содежит в себе как минимум время окончания действия ключа. По наступленнии указанной даты - ключ перестает работать
Так же может содержать время начала действия лицензии
### BELONGS_TO или BELONGS TO;
Для кого был создан ключ, или кто автор ключа
### PURPOSE;
Судя по всему, это просто описание ключа
### NUMBER_OF_ENGINES;
Для NPrinting
Число
### A64;
YES/NO
### ABDI;
Qlik Associative Big Data Index
YES/NO
### ANALYZER_TIME_UNIT;
Число
### ANALYZER_TIME;
Число 
### ANALYZER;
Количество лицензий типа Аналитик. 
Число
### BLACKLISTED;
YES/NO
### CLOUD_SERVICE;
YES/NO
### CLOUD_SERVICES;
YES/NO
### CORE_TIME_UNIT;
Число
### CORE_TIME;
Число
### CUSTOMER;
Для DataMarket
Число
### DATAMARKET_PACKAGE;
Название раздела из DataMarket, всегда есть дата начала и дата окончания. 
Возможные значения:
- ESSENTIALS_EVALUATION
- ESSENTIALS
- EVAL_ESSENTIALS
- FINANCIAL_REPORTS
- OEM_ESSENTIALS
- STOCKS_AND_INDICES
- TEST_BUNDLE
- TEST
- WORLD_CURRENCIES
- WORLD_WEATHER
### DISABLE_ANNOTATIONS;
YES/NO
### DISABLE_DATA_REDUCTION;
YES/NO
### DISABLE_DMS;
Используется для QlikView Small Business Edition
YES/NO
### DISABLE_SESSION_COLLABORATION;
YES/NO
### DYNAMIC_UPDATE;
Использовалось до QlikView 11 версии.
YES/NO
### ELASTIC;
YES/NO
### EMBED_LICENSE;
YES/NO/*
### EMBED_OEM_PRODUCT_ID;
YES/NO
### EMBEDDED_PRODUCTLEVEL;
Значение те же что и у PRODUCTLEVEL
### ESSENTIAL_NR_OF_SEATS;
Для DataMarket, всегда есть дата начала и дата окончания
### ESSENTIALS_NR_OF_SEATS;
Для DataMarket, всегда есть дата начала и дата окончания
### FACEBOOKFANPAGES;
Для веб-коннектора
### FACEBOOKINSIGHTS;
Для веб-коннектора
### FINANCIAL_REPORTS_NR_OF_SEATS;
Для DataMarket
Число
### FORCE_AUTHENTICATION;
YES/NO
### FREE_DATA;
YES/NO
### FREE;
YES/NO
### GEO_SERVER;
YES/NO
### GEOANALYTICS;
YES/NO
### GEOCODING;
Число
### GEOPLUS;
YES/NO
### GOOGLEADSENSE;
Для веб-коннектора
YES/NO
### GOOGLEADWORDS;
Для веб-коннектора
YES/NO
### GOOGLEANALYTICS;
Для веб-коннектора
YES/NO
### GOOGLEBIGQUERY;
Для веб-коннектора
YES/NO
### GOOGLEDFP;
Для веб-коннектора
YES/NO
### GOOGLESEARCHCONSOLE;
Для веб-коннектора
YES/NO
### IA64;
Использовалось до QlikView версии 7
YES/NO
### IGNORE_TOKENS;
YES/NO
### JIRA;
Для веб-коннектора
YES/NO
### LICENSE_LEASE;
Если указано YES, то сервер не выдает лицензии пользователям, очевидно что тогда QlikView Desktop не будет лицензироваться при обращениях к QlikView Server
YES/NO
### MAILCHIMP;
Для веб-коннектора
YES/NO
### MONGODB;
Для веб-коннектора
YES/NO
### MSCRM;
Для веб-коннектора
YES/NO
### NAMED_CAL_BY_MACHINE_ID;
Для QlikView Server
### NO_OF_CORES;
Для Qlik Sense, ограничение на количество ядер
Число
### NO_OF_ENGINES;
Для NPrinting 16ой версии (PRODUCTLEVEL;60)
### NUMBER_OF_APPS;
Для Qlik Sense, скорее всего максимальное значение количества публикуемых приложений
### NUMBER_OF_CLUSTER_NODES;
Для QlikView Server, количество серверов которое может находится в одном кластере
### NUMBER_OF_CPUS;
Количество ядер которое может использовать QlikView Server
### NUMBER_OF_DEVELOPERS;
Количество разработчиков для NPrinting
### NUMBER_OF_DOCUMENTS;
### NUMBER_OF_SEATS;
Используется в Qlik DataMarket

### NUMBER_OF_DOCUMENTCALS;
Ипользуется в QlikView Server. Количество документарных лицензий
Возможность просматривать только 1 документ одним пользователем в течении 28 дней
Число
### NUMBER_OF_SESSIONCALS;
Ипользуется в QlikView Server
Limit on Session CALs
### NUMBER_OF_USAGECALS;
Ипользуется в QlikView Server
Возможность открыть 1 документ, на 1 сессию (1 час) одним пользователем. При открытии - тратится 1 лицензия на 28 дней.
Если пользователь смотрит 1 документ например утром и этот же документ вечером - то используется 2 лицензии
### NUMBER_OF_USERCALS;
Один пользователь открывает любое количество лицензий. 28 дней таймлимит
Limit on Named User CALs
Ипользуется в QlikView Server
### NUMBER_OF_USERS;
Ипользуется в Qlik NPrinting
### O365SHAREPOINT;
### ODATA;
### OEM_EDITION;
### OEM_PRODUCT_ID;
### OVERAGE_CORE_TIME_UNIT;
### OVERAGE;
### PDF_GENERATION;
Для QlikView Publisher
Разрешает генерацию PDF-файлов паблишером
### PLATFORM_NO_OF_CORES;
### PRODUCT;
### PROFESSIONAL;
Количество лицензий PROFESSION для Qlik Sense при новой модели лицензирования
### QDM_ESSENTIALS;
### QEX_AUTOMATION;
Для Qlik Expressor
### QEX_CONNECTOR_TPT;
Для Qlik Expressor
### QEX_CONNECTOR;
Для Qlik Expressor
### QEX_CONNECTORS;
Для Qlik Expressor
### QEX_DD_UNLOCK;
Для Qlik Expressor
### QEX_EXT_DEVELOPER;
Для Qlik Expressor
### QEX_NUM_CORE;
Для Qlik Expressor
### QEX_REPOSITORY;
Для Qlik Expressor
### QLIKWEBCONNECTORS;
### REQUESTED;
### SLACK;
### SPECIAL_EDITION;
### STOCKS_AND_INDICES_NR_OF_SEATS;
### SUBSCRIPTION;
Лицензии по подписке, имеют дату начала и окончания подписки
### SUGARCRM;
### TEST_EDITION;
### TEXTANALYSER;
### TOKENS;
Количество токенов, применялось до разделения на лицензии PROFESSIONAL и ANALYZER в Qlik Sense
### TWITTER;
### USERCALS;
### WEBCONN;
### WEBCONNECTORS;
### WEBPARTS;
Лицензия для QlikView Webparts
### WORKBENCH;
Лицензия для QlikView Server
Позволяет использовать объекты приложений в произвольных интернет-страницах, или в любом другом приложении

[qlik help](https://help.qlik.com/en-US/qlikview-developer/November2018/Subsystems/QlikViewWorkBench/Content/QV_QVWB/workbench-start.htm)
### WORLD_CURRENCIES_NR_OF_SEATS;
### WORLD_WEATHER_NR_OF_SEATS;
### WORLD_WEATHER;
### X64;
Использовалось до версии QlikView 8.2
### YOUTUBE;
### YOUTUBEANALYTICS;