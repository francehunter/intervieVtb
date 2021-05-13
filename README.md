
1. Методика НТ: какие разделы, сбор целей НТ, анализ системы, определение источников нагрузки и смежных систем
==========

Нагрузочное тестирование - сбор показателей системы при подаче нагрузки в виде внешних запросов с целью установления факта соответствия исследуемой системы заявленным требованиям. Обычно запросы генерируются "вирутальными" пользователями - программами, имитирующими работу пользователя. Запросы желательно должны быть уникальными для избежания искажения возможностей системы за счет кеширования. При этом фискируются различные метрики исследуемой системы.

2. Виды нагрузочных тестов (макс.перф, стабильности, объемное, отказоустойчивости и т.д.), регрессионное в рамках релизов
==========

Производительности=поиск максимальной нагрузки=макс.перф тест - выявление количественных характеристик системы под заданным профилем:
- время отклика
- фактическое количество запросов в секунду

Сресс тест - качественная оценка системы под очень высокой нагрузкой:
- принципиальная возможность работы (сервер не упал, данные не потерялись)
- восстановление после нагрузки (не нужен админ, производительность не деградировала)

Объемный тест - выявление корреляции количественных характеристик системы с объемом обслуживаемых данных. Снимается два слепка характеристик (см. маск. перф.) для условно "небольшого" размера бд и "большого". Делается утверждение о возможности масштабирования.

Стабильности тест - качественная оценка системы в долгосрочной перспективе. Профиль нагрузки условно "средний", время тестирования условно "очень большое":
- сервисы стабильны (память не течет, девайсы не перезагзужаются)
- показатели (см. макс.перф.) не деградируют

Сравнения тест - количественная оценка на разных:
- девайсах
- софте (ОС, версиях бд)

Конфигурационное тестирование - убедиться, что работает на всем предполагаемом множестве комбинаций оборудования и софта.

3. Профиль НТ: что это, как выглядит, как строится, как на его основе скрипты строятся; как из статистики собрать профиль, пошагово (почасовая статистика за год, поминутная за неделю, посекундная за раб.день)
==========

Профиль - требуемая интенсивность запросов. Выглядит как словарь "запрос"->"количество / единица времени". Обычно оформляется в табличном виде заказчиком. Тестировщик добавляет в таблицу еще одну графу "факстической количество / единица времени" и на основании допустимого отклонения делает вывод об успешности проделанного теста.

Уточнить: как из статистики собрать профиль, пошагово.

4. Мониторинг: бизнес-метрик (время отклика, ошибки от системы, интенсивность операций), на уровне "железа" (ЦПУ, ОП, диски) и на уровне программных метрик (очереди, пулы подключений к БД, java heap, threads)
==========

Бизнес-метрики:
- статистика времени отклика на запросы
- количетво запросов в секунду
- ошибки

Аппаратные метрики
- нагрузка и утилизация процессора
- использование памяти, прогрессия файла подкачки, 
- нагрука на сетевые интерфейсы
- производительность работы дисковой подсистемы
- метрики бд

Программные метрики:
- пул коннектов к бд
- поведение сборщика мусора, "утечки" памяти
- потоки (кого ждут)


5. Протоколы: общее понимание, стеки (как пример TCP/IP), связь
==========

Модель OSI:
1) Физический - вирутальные электроны или фотоны двигаются по линиям связи
2) Канальный - выделение начала и конца фрейма, коррекция ошибок, физическая адресация
3) Сетевой - логическая адресация, понятие пакета, IP
4) Транспортный - связь между конечными точками, надежность, TCP, UDP
5) Сеансовый - управление сеансом связи, синронизация, RDP
6) Представление - кодировка, ASCI
7) Прикладной - взаимодействие сети и пользователя, HTTP, WebSock

6. Протокол НТТР (методы, коды ответа, заголовки)
==========

Протокол прикладного уровня для обмена гипертекстом между сервером и клиентами. Под методом понимается префикс в урле, который може принимать ряд занчений: OPTIONS - возможности сервера; GET - получение контента с сервера; HEAD - GET без бади (способ инвалидации кеша); POST - передача контента на сервер; PUT - как POST, только вместо контента передается урл, содержащий контент; PATCH - POST к части контента (гипертекстовый гит); TRACE - возвращает конечный полученный сервером запрос (можно посмотреть, что добавилось к оригиналу от проводящей цепочки).
В ответном сообщении существует понятие кода ответа как ans.split("\n")[1]. Коды - целые числа - разбиты на группы: 1хх - информационные; 2хх - вариации успеха; 3хх - перенаправление; 4хх - ошибки клиента; 5хх - ошибки сервера. 
В тексте запроса и ответа можно выделить понятие заголовка - это часть, располагающаяся между первой строкой и \r\n\r\n. Заголовок построен как словарь "ключ" - "значение". Запарсить можно text.split("\n").split(":") + trim().

7. Технологии REST и SOAP
==========

REST - Representational State Transfer — «передача состояния представления» - архитектурный паттерн проектирования взаимодействия между приложениями.
Необходимые условия реализации:
- клиент-серверная архитектура (сервер хранит данные, клиент шаблонизирует и формирует запросы)
- стейтлес контракт (сервер не хранит состояние клиентов, а получает его из запроса)
- кеширование (хеширование ресурсов приводит к более коротким ответам)
- унификация интерфейса (данные отличаются от представления, сообщения самодостаточны, стандартизация представления)
- лейеринг (клиент абстагирован от бд, архитектуры бекэнда)
Профит:
- простота
- расширяемость
- перформанс на уровне кеша и шаблонизации
- надежность как стейтлес

SOAP - Simple Object Access Protocol - можно определить как антипод REST - для обмена в RPC - подразумевается состояние клиента. Представление данных в виде XML вида 
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
   <soap:Body>
     <getProductDetails xmlns="http://warehouse.example.com/ws">
       <productID>12345</productID>
     </getProductDetails>
   </soap:Body>
</soap:Envelope>
Видим большой оверхед на разметку.

8. Форматы XML и JSON
==========

XML - eXtensible Markup Language - текстовая форма представления данных в виде древовидной структуры. Состоит из элементов. Элемент - комбинация открытого и закрытого тега <elname myattr1="3" myattr2="4">elvalue</elname>. Между тегами расположено значение elvalue, в косых открывающих скобках - множество аттрибутов myattr.
Требования:
- наличие корневого элемента
- клиент-серверная поддержка кодировок UTF-8/16
Для выборок разработан язык запросов XPath, который представляет собой резулярное выражение, возвращающее множество найденных элементов по заданной маске.

JSON - JavaScript Object Notation - текстовая форма представления данных в виде "ключ"->"значение". Значением может быть как скаляр (число, булев тип, строка), так и вектор (массив). Есть бинарное расширение bson (активно используется в нон реляционных бд, например mongo), занчительно меньший оверхед трафика. JavaScript френдли формат и может парситься eval().

9. LoadRunner: протокол web/http (способы записи, параметризация, корреляция и верификация)
==========

Параметризация  - замена т.н. "хардкода" на синтаксис вида ${paramName}. Конечный результат выполнения операции с параметризацией будет зависить от содержимого переменной paramName. Параметры хранятся в файлах dat и могут быть быстро просмотрены из контексного меню "parameter properties".

Верификация - сравнение результатов выполнения запроса с ожидаемым (спецификацией, стандартами, etc). Выполняется функциями web_reg_save_param и web_reg_save_param_ex. Функции работают как экстракторы - извлекают данные из последующих запросов. Рузультатом верификации является успех или не_успех теста.

Запись.
Перейти в Vugen, нажать кнопку "add script". Выбрать браузер и целевой метод, нажать кнопку "play". В открытом браузере совершать юзер активность. Нажать кнопку "stop". В результате будет нагенерен метод, который воспроизводит пользовательскую активность. Запись может добавлять методы к уже существующим скриптам.

10. LoadRunner: Контроллер, мониторинг и запуск тестов (распределенный запуск, отсроченный, collate результатов)
==========

Контроллер - приложение для запуска тестов.

Мониторинг - часть контроллера для сбора различных метрик во время тестов.

Запуск тестов.
Распределенный - нажать в контроллере кнопку "load generators", добавить рабочие станции.

11. LoadRunner: Аналайзер, merge, auto-correlation
==========

Анализатор - гарфическое и табличное представление результатов тестирования - метрик. Можно открыть, нажав кнопку "analyze" в контроллере или автоматически после завершения тестирования.

12. JMeter: сэмплеры, тред-группы, ассерты, экстракторы, параметризация, pasing
==========


Семплер - нод GUI - инициатор и обработчик запроса. Можно выполнять htpp, ftp, smpt etc запросы или писать свои. Результатом запроса является строка с ответом и логическое состяние "успешо/нет".

Тред-группа - нод GUI - пул потоков, каждый из которых выполняет код, скомпилированный из дочерних нодов. Имеет настройки поведения при ошибках семплеров, времени жизни потоков и скорости их создания.

Параметризация - антипод "хардкода" - замена константных частей логики на переменные для:
- быстрой смены объекта тестирования (например, хоста)
- работы со словарями 
- организации кастомной связки между нодами
Параметризация осуществляется как в рутовом элементе Test Plan в секции User Defined Variables, так и в кастомных CSV data reader.

Ассерты - нод GUI - позволяют уточнять булев результат выполнения родитеьлского семплера путем анализа ответа. Есть ассерты по регулярным выражениям, размера и длительности ответа, наличию заголовка и возможность кодить логику проверку на скриптовых языках, например groovy.

Экстракторы - нод GUI - позволяют извлекать часть ответа и заносить их занчение в переменные (см.параметризация). Можно извлекать данные по регулярному выражению, JSON пути, XPath пути, писать кастомные экстракторы на groovy.

Pasing - подход к обеспечению заданной равномерной нагрузки - постоянное количество запросов в единицу времени. Хорошей практикой реализации пейсинга в JMeter является использование нода Constant Throughput Timer, который вызывает Module Controller, который ссылается на Test Fragment.

13. SQL: group by, having, join
==========

GROUP BY - группировка результатов выборки. Влияет на работу агрегатных функций COUNT, MIN, MAX, AVG и SUM. Без применения GROUP BY агрегатные функции будут работать по всем столбцам, указанным в SELECT, иначе - по результатам группровки. Математический аналог для SUM - спектр - SELECT age, SUM(salary) as sum FROM workers GROUP BY age - распределение зарплаты по возрастам.
HAVING - это WHERE для результатов группировки.
JOIN - объединение множества строк из множества таблиц. Результат слияния каждой пары таблиц можно представить в виде
<img src="https://github.com/francehunter/intervieVtb/blob/main/interview/join.png" width="1000" height="200">


14. Oracle: AWR, планы запросов (как получить, как анализировать и какую инфу получать оттуда)
==========

План запросов - алгоритм действия при выборке данных по запросу sql. Пожно посмотреть, если выполнить sql вида explain plan for sqlText, или нажать кнопку "generation execution plan" в IDE. Получаем древовидную структуру. В ней есть номер шага, вид сканирования (по данным или индексу), условная стоимость, объем операции в байтах и количество затронутых строк.

15. БД Postgres: vacuum/autovacuum, сбор статистики
==========

vacuum - очистка. Чистить надо устаревшие версии данных (UPDATE = DELETE + INSERT)/ Таблица разредена на страницы. В страницах хранятся индексы и строки. vacuum сначала вычищает индексы, ссылающиеся на строки за горизонтом видимости, а потом - сами строки.

autovacuum - запуск vacuum через сервис, адаптивный к нагрузке. При autovacuum_enabled=true запускается сервис autovacuum launcher, который планирует работу, а очисткой занимается множество autovacuum worker. Воркеры просматривают таблицы, в которых кол-во удаленных строк больше, чем autovacuum_vacuum_threshold и autovacuum_vacuum_scale_factor. Таблицы с превышениями этих параметров получают vacuum.

Cтатистика 
SELECT * FROM pg_stat_progress_vacuum

Уточнить: еще какая-то статистика нужна?

16. Kafka: что это, как работает и ее метрики (лаги, например)
==========


<img src="https://github.com/francehunter/intervieVtb/blob/main/kafka-diagram.jpg" width="500" height="200">
Kafka - это множество брокеров сообщений, применяемый для обмена данными в микросервисной среде. Получает от "продьюсера" данные вида "ключ->значение" и кладет их а указанную очередь=топик. Топик состоит из партиций=array list. Множество "консамеров" могут эти значения из очереди извлекать.
В ядре системы - Zookeeper - распределенное хранилище данных вида "ключ->значение".
Есть асинхронная репликация "master->slave". При этом консамеры всегда читают из мастера. Мастеров можно переизбирать, когда текущий отвалился.

Метрики брокера:
UnderReplicatedPartitions
IsrShrinksPerSec/IsrExpandsPerSec
ActiveControllerCoun
OfflinePartitionsCount
LeaderElectionRateAndTimeMs
UncleanLeaderElectionsPerSec
TotalTimeMs
PurgatorySize
BytesInPerSec/BytesOutPerSec
RequestsPerSecond

Метрики продьюсера:
compression-rate-avg
response-rate
request-rate
request-latency-avg
outgoing-byte-rate
io-wait-time-ns-avg
batch-size-avg

Метрики консамера:
records-lag
records-lag-max
bytes-consumed-rate
records-consumed-rate
fetch-rate

Метрики хранителя зоопарка:
outstanding_requests
avg_latency
num_alive_connections
followers
pending_syncs
open_file_descriptor_count

 Обище метрики: процессор, диск, сеть, сборщик мусора java.

Подробнее про метрики https://www.datadoghq.com/blog/monitoring-kafka-performance-metrics/
Программно можно достать через public Map metrics().

17. Автоматизация НТ (это в первую очередь про jenkins, maven, git)
==========

git - определяется собственным маном как "stupid content tracker". Реализации: Git, GitHub, GitLab. Данные - дельта+архивы - хранятся в иерерхии веток. Типичный воркфлоу: clone - скопировать репозиторий локально на свой десктоп; pull = fetch + merge - обновить свою локальную версию из удаленного репозитория; чего-то сделать + add - добавить новые файлы; commit - локально сохранить срез данных + прокомментировать его; push - сохранить все коммиты в уделанном репозитории. Опционально: branch - создать новую ветку (новая версия программы = новая ветка тестов); merge - слияние веток, если бранч делался ради лонг ворклоу изолейшн.

jenkins - приложение для CI - Continuous Integration - непрерывной интеграции. Позволяет из окна браузера автоматизировать процессы сборки и отчетности. Сборку можно писать консольными командами. Имеется groovy для реализации не тривиальной логики. В качестве исходников можно забирать репозитории из git, svn. На триггерах можно добавлять тесты. О результатах работы отчитываться на email.

maven - приложение для сборки проектов. Конфигурируется на xml.

graddle - приложение для сборки проектов. Конфигурируется на groovy.


18. Критерии качества НТ. Пример: НТ провели, все хорошо, а на проде - проблемы. В чем могут быть причины?
==========

Критерии:
- матрица трассируемости - таблица покрытия основных фич продукта тестами.

Уточнить: еще критерии?

Проблемы на проде:
- ошибки. Забрать логи, локализовать порождающий компонент системы, воспроизвести, в идеале получить исходники, пофиксить. На практике исходники никто не даст, поэтому завести дефект.
- запрофильное время отклика. Забрать логи, локализовать порождающий компонент системы. Если в логах ведется сбор тайминг обращения к внешним сервисам, то есть смысл сделать сравнительный анализ таймингов с прода с тестовыми заглушками.

Уточнить: еще проблемы и решения?
