

# Blockchain by Erachain technology - Sidechain
How make Your own blockchain with KYC, exchange, assets, polls etc.

For start Your own blockchain put `sideGENESIS.json` file into application folder. See examples in `z_GENESIS_EXAMPLES`folder.

See https://github.com/erachain/sidechains

> Файлы с расширением .json содержат в себе данные по формату `JSON`.

### Общее начало - GENESIS блок
Для объединения в одну общую блокчейн-среду необходимо всем задать одинаковый начальный блок - генесиз-блок. Для его создания используется особый файл настроек - `sideGENESIS.json`, пример которого можно найти в папке: `z_GENESIS_EXAMPLES`. Скопируйте файл примера в основную папку, переименуйте его в `sideGENESIS.json` и отредактируйте согласно описанию ниже и подсказкам внутри этого файла. Внимание, на системах Windows, которые скрывают расширения файлов, не сделайте случайно два расширения как sideGENESIS.json.json  

**!!! ВНИМАНИЕ** - этот файл нужно беречь, так как его утрата приведет к тому что ни одна нода не сможет присоединиться к вашей цепочке. Самое лучшее - это сохранить его в головной цепочке Eracahin. Основная цепочка Erachain (MainNet) - та которая работает на узлах с портами сети 904Х. Сохранить можно как файл при создании транзакции "Создать Документ", не забудьте при этом задать заголовок документа чтобы потом по нему быстро находить этот файл.

> Genesis block (генесиз блок) - это первый блок цепочки данных, в котором происходит первоначальное распределение пакетов для форжинга (ERA) по счетам, которые находятся в кошельках узлов (нод), которые должны быть включены и будут собирать блоки цепочки. Изменение хотя бы одного значимого знака в генесиз-блоке приведет к его изменению и он будет относиться к другой цепочке данных.

В файле `sideGENESIS.json` необходимо указать данные для вашей цепочки:
+ имя цепочки  
+ время первого блока в формате TIMESTAMP в миллисекундах (GMT). Его можно взять   отсюда [https://www.epochconverter.com/](https://www.epochconverter.com/)
+ список владельцев пакетов форжинга. Причем у каждого держателя можно задать
 форжеров которым будут переведены пакеты форжинга в долг (их можно потом забрать).

Задайте не менее 21 счета в совокупности (на которых будут пакеты форжинга в собственности или в долгу) для старта цепочки.

### Создание счетов
Счета можно взять из вашего кошелька любой блокчейн-среды Erachain - из тестовой (TestNet), демонстрационной (DemoNet), основной (MainNet) или другого сайдчейна (SideNet). Либо возьмите их у своих партнеров по сайдчейну. Так же вы можете создать их просто запустив ноду c текущими недоредактированными данными в `sideGENESIS.json`. После запуска программы и создания нового кошелька (или восстановления его, см. инструкцию по Erachain) зайдите в закладку Мои Счета и создайте там столько счетов сколько вам надо. Скопируйте их (правая мышка на счете и Скопировать Счет) и сохраните в текстовом файле например. Не забудьте сохранить и секретный мастер ключ кошелька - SEED. После чего закройте программу и удалите папку с данными цепочки `/datachain` - так как в ней уже будет создан генесиз-блок, который нам не нужен.
  
### Начальное распределение пакетов форжинга (активов ERA)
Необходимо делать равномерное распределение количества ERA на начальных форжинговых счетах, особенно если их меньше 30шт. Например если у вас 20 счетов, то распределите по 500 тыс ERA на каждый в генесиз-блоке.

При этом первый счет в списке владельцев сможет создать первых 20 персон даже не будучи удостоверенным и удостоверять счета для них.  
Но желательно все же удостоверить свою персону сразу же. И потом уже удостоверять других персон.

Так же хотя бы один счет должен обладать не менее 3% от общего числа ERA, второй счет не менее 2% и третий по крайней мере 1% для уверенного запуска форжинга.

Помните что пакеты форжинга начинают форжить не сразу, поэтому за раз передавать более чем 10% от всего объема ERA не рекомендуется так как вся цепочка может остановиться.

### Долговые пакеты для форжинга
В среде Erachain можно давать активы в долг. Это очень удобно (см. ниже) - Вы можете задать в генесиз-блоке перевод своих пакетов ERA на  форжинговые счета в долг, - так чтобы ваш основной счет был разгружен и им не надо было форжить, хотя пакет ERA будет находиться у него в собственности и его всегда можно будет вернуть обратно. Для этого используйте список `// debtors`у каждого счета-собственника ERA.

Так же полезно для защиты своего пакета ERA от утраты, например из-за краха виртуальной машины или ее взлома:  
1. создайте на виртуальной машине, которая будет форжить 24/7, новый кошелек с несколькими счетами  
2. настройте запуск ноды с автозапуском форжинга  
3. передавайте пакет ERA в долг на счета ноды, запущенной на виртуалке  

Тогда даже если виртуалку взломают и получат доступ к ее счетам, то максимум что сможет сделать злоумышленник - вернуть вам обратно пакеты ERA и снять нафорженные COMPU, так как активы, переданные в долг, нельзя перевести со счета ни кому кроме как обратно хозяину.

## Общая сеть узлов - пиры
Блокчейн среда будет генерировать блоки только если в сети есть хотя бы 2 узла (ноды), которые друг друга увидят и законнектится друг с другом и имеют одинаковый генесиз-блок.  
Для того чтобы ноды (узлы, пиры) увидели друг друга, необходимо иметь хотя бы одну ноду с возможностью подключения к ней других нод (открыть доступ для внешних вызовов, см. ниже) и ее IP указать в списке начальных нод вашей сети. Чем больше таких нод, тем надежней сеть. Список таких нод задается в файле `peers-side.json`. Пример можно посмотреть в других подобных файлах. Раздайте этот файл на другие ноды вашей сети.  
Для того чтобы нода принимала внешние вызовы, на вычислительной машине (виртуальная машина), на которой она запущена, необходимо открыть порт сети **9056** для внешних вызовов. Для справки - порт 9057 используется для доступа к API и blockexplorer. А порт 9058 дает доступ к RPC. Это делается в файреволе операционной системы вычислительной машины.


### Локальная сеть
Если у вас запущено несколько нод в локальной сети, то для того чтобы они видели друг друга включите поиск нод в локальной сети при старте ноды. Это включается в настройках (File -> Settings -> Main). Либо пропишите их IP явно в файле peers-side.json. Так же чтобы ноды не перебивали друг друга поиском внешних узлов, задайте в настройках "Минимальное число подключений"  на 1-2 меньше чем число нод в локальной сети.

## Общий протокол сети
Все узлы в сети должны работать по одному протоколу. Иначе будет создано ответвление - хардфорк. Параметры протокола хранятся в файле `sidePROTOCOL.json`.  
Пример в файле `sidePROTOCOL_example.json`в папке `z_GENESIS_EXAMPLES`.

## Первый запуск ноды сайдчейна
Для того чтобы узлы увидели друг друга и оказались все в одной цепочке с одинаковым генесиз-блоком - раздайте им свои файлы `sideGENESIS.json`,  `sidePROTOCOL.json` и `peers-side.json`.  
При первом запуске будет сформирован генесиз-блок и записан в пустую базу данных, находящуюся в папке `\datachain`. Поэтому перед запуском нужно удалить эту папку если там осталась старая цепочка. Иначе при старте будет выдана ошибка - неверный генесиз-блок.  
Так же необходимо чтобы несколько нод из списка `peers-side.json` могли принимать внешние подключения. Это как правило могут делать виртальные машины и внешние сервера.

Если после запуска ноды банят друг друга, то на каждой ноде проверьте правильность сборки генесиз-блока. Откройте блокэксплорер (из меню ноды) и удостоверьтесь что подпись у первого блока на всех нодах одинаковая. Возможно что кто-то изменил что-то в файле настроек блока (возможно другая кодировка) и поэтому нода выпала из нужной цепочки и собрала другой генесиз-блок.


### Включение форжинга
Для автоматического включения форжинга при старте ноды необходимо указать пароль доступа к кошельку:  

    java -jar erachain.jar -pass=PASSWORD

Описание других ключей запуска описаны тут:
[https://github.com/erachain/Erachain-Node](https://github.com/erachain/Erachain-Node)

### Автоматическое создание кошелька
Если задать параметр при старте `-seed`, то если в папке нет папки с секретеными ключами - walletKeys, то программа сама создаст новый кошелек, счета в нем и запаролирует заданным паролем. Пример команды:  

    java -jar erachain.jar -pass=PASSWORD -seed=N:SEED:PASSWORD

Где N - число счетов, которое нужно создать, SEED - мастер ключ, PASSWORD - пароль, которым надо запароллировать кошелек. Если он совпадает со значением в `-pass`, то сразу начнется форжинг.  
Вдобавок если ваша нода не будет раздавать RPC, то задайте ключ `-nodatawallet` - это ускорит работу ноды. А если нода не будет раздавать API и blockexplorer (web-server выключен), то можно отказаться от непротокольных индексов задав параметр `-opi` (Only Protocol Indexing) - это ускорит работу ноды в 4-ре раза. Пример:  

    java -jar erachain.jar -pass=123456789 -seed=5:R2we%ga...:123456789 -opi -nodatawallet


### Remote Protocol Control (RPC)
Доступ к RPC дает доступ к кошельку ноды. В настройках по умолчанию доступ к RPC не разрешен. Его можно включить, но оставить права доступа - только локально или для заданных IP = см. вкладку настроек ноды. Если Вы хотите организовать сервер интеграции с блокчейн то в настройках ноды включите RPC. При этом свой сервер сервиса лучше располагать на той же виртуальной машине что и нода блокчейна - тогда можно открыть доступ только для локальных вызовов что защитит от атак извне.  
Пример команды PRC:  
http://127.0.0.1:9058/peers  
или
http://127.0.0.1:9058/addresses?password=1

Так же команды RPC можно давать с помощью CLI (см. ниже)

### API
API тоже позволяет организовать интеграцию с блокчейн, но доступа к вашему кошельку не дает - в нем нет попросту таких команд. Поэтому в настройках ноды можно его включить для всех. Так же включенный API сервер позволяет просматривать блокэксплорер на ноде.

### Command Line Interface (CLI)
Ноду можно запустить в режиме CLI задав ключ при старте -cli

В этом случае нода запускается в усеченном режиме - только чтобы давать команды RPC как через GUI консоль. Для помощи по командам введите команду `help`

Нода в режиме cli посылает запросы другой работающей на этом же кмопьютере ноде, у которой включен RPC сервер.

Пример команды:  

    get peers



## Как быстро создать много счетов
### Вариант 1 - в программе в закладе Счета
Зайдите в программу и во вкладке Мои Счета создайте новые счета кнопкой `Создать Счет`

### Вариант 2 - по восстановлению кошелька
1. Сохраните мастер-ключ (СИД, SEED)  
2. Закройте программу  
3. Удалите паку с ключами кошелька /walletKeys  
4. Запустите программу  
5. Выберите восстановить кошелек
6. Укажите в дополнительном поле **нужное** количество счетов

### Вариант 3 - аргументом при запуске создания кошелька
1. Сохраните мастер-ключ (СИД, SEED)  
2. Закройте программу  
3. Удалите паку с ключами кошелька /walletKeys  
4. Запустите программу  с указанием аргумента -seed=N:SEED:PASSWORD (см. описание аргументов при запуске


## Если кошелек пустой (Мои Счета)
Если в ноде в ГУИ в закладе Мои Счета не все счета появились или вообще пусто то надо:  
1. в настройках задать Минимльное чило пиров = 0 - чтобы нода не начинала синхронизироваться  
2. забанить в Обзор сети -> Дополнительная Информация все пиры с которых идет синхронизация  
3. в этом случае в Мои Счета кнопка Создать Счет и Синхронизировать Кошелек станут активными  
4. нажать или Добавить Счет или Синхронизировать Кошелек - при синхронизации подтянутся все счета, а при добавить счет добавятся несозданные счета  
5. в настройках опять поставить 10 пиров минимально  
6. подключиться к пирам (правая мышка на пирах в Дополнительная Информация) или перегрузить прогу  

