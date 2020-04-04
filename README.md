
# Blockchain by Erachain technology - Sidechain
How make Your own blockchain with KYC, exchange, assets, polls etc.

For start Your own blockchain put genesis.json file into application folder. See examples in z_genesis folder.

See https://github.com/erachain/sidechains

### Общее начало - GENESIS блок
Для объединения в одну общую блокчейн-среду необходимо всем задать одинаковый начальный блок - генесиз-блок. Для его создания используется особый файл настроек - `genesis.json`, пример которого можно найти в папке: `z_GENESIS_EXAMPLES`

**!!! ВНИМАНИЕ** - этот файл нужно беречь, так как его утрата приведет к тому что ни одна нода не сможет присоединиться к вашей цепочке. Самое лучшее - это сохранить его в головной цепочке Eracahin.

В файле `genesis.json` необходимо указать данные для вашей цепочки:
+ имя цепочки  
+ время первого блока в формате TIMESTAMP в миллисекундах (GMT). Его можно взять   отсюда [https://www.epochconverter.com/](https://www.epochconverter.com/)
+ список владельцев пакетов форжинга. Причем у каждого держателя можно задать
 форжеров которым будут переведены пакеты форжинга в долг (их можно потом забрать).

Задайте не менее 21 счета в совокупности (на которых будут пакеты форжинга в собственности или в долгу) для старта цепочки.  

При этом первый счет в списке владельцев сможет создать первых 20 персон даже не будучи удостоверенным и удостоверять счета для них.  
Но желательно все же удостоверить свою персону сразу же. И потом уже удостоверять других персон.

Так же по крайней мере один счет должен обладать по крайней мере 3% от общего числа ERA, второй счет по крайней мере 2% и третий по крайней мере 1% для уверенного запуска форжинга.

Помните что пакеты форжинга начинают форжить не сразу, поэтому за раз передавать более чем 10% от всего объема ERA не рекомендуется так как вся цепочка может остановиться.

## Общая сеть узлов - пиры
Для того чтобы ноды (узлы, пиры) увидели друг друга, необходимо задать хотя бы одни общий IP узла, через который ноды будут получать данные о других нодах. Желательно указать несколько начальных нод вашей сети. Их список необходимо задать в файле `peers-side.json`. Пример можно посмотреть в других подобных файлах. Раздайте этот файл на другие ноды вашей сети.  
Не забудьте на локальных машинах узлов сети открыть порт сети **9056** для внешних вызовов. Для справки - порт 9057 используется для доступа к API и blockexplorer. А порт 9058 дает доступ к RPC.

## Общий протокол сети
Все узлы в сети должны работать по одному протоколу. Иначе будет создано ответвление - хардфорк. Параметры протокола хранятся в файле `sidePROTOCOL.json`.  
Пример в файле `sidePROTOCOL_example.json`в папке `z_GENESIS_EXAMPLES`.

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


### Рекомендации распределения пакетов форжинга
Для защиты своего пакета ERA от утраты, например из-за краха виртуальной машины или ее взлома:  
1. создайте на виртуальной машине, которая будет форжить 24/7, новый кошелек с несколькими счетами  
2. настройте запуск ноды с автозапуском форжинга  
3. передавайте пакет ERA в долг на счета ноды, запущенной на виртуалке  

Тогда даже если виртуалку взломают и получат доступ к ее счетам, то максимум что сможет сделать злоумышленник - вернуть вам обратно пакеты ERA и снять нафорженные COMPU, так как активы, переданные в долг, нельзя перевести со счета ни кому кроме как обратно хозяину.

## Если кошелек пустой (Мои Счета)
Если в ноде в ГУИ в закладе Мои Счета не все счета появились или вообще пусто то надо:  
1. в настройках задать Минимльное чило пиров = 0 - чтобы нода не начинала синхронизироваться  
2. забанить в Обзор сети -> Дополнительная Информация все пиры с которых идет синхронизация  
3. в этом случае в Мои Счета кнопка Создать Счет и Синхронизировать Кошелек станут активными  
4. нажать или Добавить Счет или Синхронизировать Кошелек - при синхронизации подтянутся все счета, а при добавить счет добавятся несозданные счета  
5. в настройках опять поставить 10 пиров минимально  
6. подключиться к пирам (правая мышка на пирах в Дополнительная Информация) или перегрузить прогу  

