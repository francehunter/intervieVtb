
1. Методика НТ: какие разделы, сбор целей НТ, анализ системы, определение источников нагрузки и смежных систем


2. Виды нагрузочных тестов (макс.перф, стабильности, объемное, отказоустойчивости и т.д.), регрессионное в рамках релизов


3. Профиль НТ: что это, как выглядит, как строится, как на его основе скрипты строятся; как из статистики собрать профиль, пошагово (почасовая статистика за год, поминутная за неделю, посекундная за раб.день)


4. Мониторинг: бизнес-метрик (время отклика, ошибки от системы, интенсивность операций), на уровне "железа" (ЦПУ, ОП, диски) и на уровне программных метрик (очереди, пулы подключений к БД, java heap, threads)


5. Протоколы: общее понимание, стеки (как пример TCP/IP), связь


6. Протокол НТТР (методы, коды ответа, заголовки)
Протокол прикладного уровня для обмена гипертекстом между сервером и клиентами. Под методом понимается префикс в урле, который може принимать ряд занчений: OPTIONS - возможности сервера; GET - получение контента с сервера; HEAD - GET без бади (способ инвалидации кеша); POST - передача контента на сервер; PUT - как POST, только вместо контента передается урл, содержащий контент; PATCH - POST к части контента (гипертекстовый гит); TRACE - возвращает конечный полученный сервером запрос (можно посмотреть, что добавилось к оригиналу от проводящей цепочки).
В ответном сообщении существует понятие кода ответа как ans.split("\n")[1]. Коды - целые числа - разбиты на группы: 1хх - информационные; 2хх - вариации успеха; 3хх - перенаправление; 4хх - ошибки клиента; 5хх - ошибки сервера. 
В тексте запроса и ответа можно выделить понятие заголовка - это часть, располагающаяся между первой строкой и \r\n\r\n. Заголовок построен как словарь "ключ" - "значение". Запарсить можно text.split("\n").split(":") + trim().

7. Технологии REST и SOAP
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

SOAP - Simple Object Access Protocol - можно определить как антипод REST - для обмена в RPC - подразумевается состояние клиента.

8. Форматы XML и JSON


9. LoadRunner: протокол web/http (способы записи, параметризация, корреляция и верификация)


10. LoadRunner: Контроллер, мониторинг и запуск тестов (распределенный запуск, отсроченный, collate результатов)


11. LoadRunner: Аналайзер, merge, auto-correlation


12. JMeter: сэмплеры, тред-группы, ассерты, экстракторы, параметризация, pasing


13. SQL: group by, having, join


14. Oracle: AWR, планы запросов (как получить, как анализировать и какую инфу получать оттуда)


15. БД Postgres: vacuum/autovacuum, сбор статистики


16. Kafka: что это, как работает и ее метрики (лаги, например)


17. Автоматизация НТ (это в первую очередь про jenkins, maven, git)


18. Критерии качества НТ. Пример: НТ провели, все хорошо, а на проде - проблемы. В чем могут быть причины?
