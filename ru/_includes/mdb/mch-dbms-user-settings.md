* **Add HTTP CORS header**{#setting-add-http-cors-header} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Включает заголовок CORS в HTTP-ответы. 

    По умолчанию заголовок CORS не включается в HTTP-ответы.

* **Allow DDL**{#setting-allow-ddl} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, будут ли выполняться DDL-запросы (`CREATE`, `ALTER`, `RENAME` и другие).

    По умолчанию выполнение DDL-запросов разрешено.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/permissions-for-queries/#settings_allow_ddl).

    См. также: настройка [Readonly](#setting-readonly).

* **Compile**{#setting-compile} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, компилировать ли запросы при их выполнении. При включенной компиляции структурно идентичные запросы могут выполняться быстрее за счет использования скомпилированных частей запроса.

    Используется вместе с настройкой [Min count to compile](#setting-min-count-to-compile).

    По умолчанию компиляция выключена.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#compile).

* **Compile expressions**{#setting-compile-expressions} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, компилировать ли выражения при выполнении запросов. При включенной компиляции запросы, в которых используются идентичные выражения, могут выполняться быстрее за счет использования скомпилированных выражений.

    Используется вместе с настройкой [Min count to compile expression](#setting-min-count-to-compile-expression).

    По умолчанию компиляция выражений выключена.

* **Connect timeout**{#setting-connect-timeout} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Время ожидания соединения (в миллисекундах).

    Минимальное значение — `1`, по умолчанию — `10000` (10 секунд).

* **DISTINCT overflow mode**{#setting-distinct-overflow-mode} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет поведение {{ CH }} в ситуации, когда количество данных при выполнении запроса `SELECT DISTINCT` [превысило ограничения](https://clickhouse.tech/docs/ru/operations/settings/query-complexity/#ogranicheniia-na-slozhnost-zaprosa) — `throw` (прервать выполнение, вернуть ошибку) или `break` (вернуть неполный результат).

    По умолчанию значение не выбрано (эквивалентно `throw`).

* **Distributed aggregation memory efficient**{#setting-distributed-aggregation-memory-efficient} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, включать ли режим экономии памяти при распределенной агрегации.

    При распределенной обработке запроса [внешняя агрегация](#setting-max-bytes-before-external-group-by) производится на удаленных серверах. Включите эту настройку, чтобы сократить использование оперативной памяти на сервере-инициаторе запроса.

    По умолчанию режим экономии памяти выключен.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/sql-reference/statements/select/group-by/#select-group-by-in-external-memory).

* **Distributed product mode**{#setting-distributed-product-mode} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Изменяет поведение распределенных подзапросов, когда запрос содержит произведение распределённых таблиц:
    * `deny` — запрещает использование таких подзапросов.
    * `local` — заменяет базу данных и таблицу в подзапросе на локальные для конечного сервера (шарда), оставив обычный `IN/JOIN`.
    * `global` — заменяет запрос `IN/JOIN` на `GLOBAL IN/GLOBAL JOIN`.
    * `allow` — разрешает использование таких подзапросов.

    По умолчанию значение не выбрано (эквивалентно `deny`).

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#distributed-product-mode).

* **Empty result for aggregation by empty set**{#setting-empty-result-for-aggregation-by-empty-set} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, в каком формате возвращается результат при агрегации данных без ключей (без `GROUP BY`) для пустого множества (например, `SELECT count(*) FROM table WHERE 0`):
    * опция выключена (по умолчанию) — {{ CH }} вернет результат, состоящий из одной строки со значениями `NULL` для агрегатных функций, в соответствии со стандартом SQL.
    * опция включена — {{ CH }} вернет пустой результат.

* **Enable HTTP compression**{#setting-enable-http-compression} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, будут ли сжаты данные в ответе на HTTP-запрос.

    По умолчанию {{ CH }} хранит данные в сжатом виде. Результат выполнения запроса представлен в несжатом виде. Чтобы {{ CH }} сжимал результат запроса при отправке по HTTP, включите эту настройку и передайте в заголовке `Accept-Encoding` запроса выбранный метод сжатия.

    Поддерживаются следующие методы сжатия: `gzip`, `br` и `deflate`.

    По умолчанию сжатие данных в ответе на HTTP-запрос выключено.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/interfaces/http/).

* **Fallback to stale replicas for distributed queries**{#setting-fallback-to-stale-replicas-for-distributed-queries} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Форсирует запрос в устаревшую реплику в случае, если актуальные данные недоступны.

    {{ CH }} выберет наиболее актуальную из устаревших реплик таблицы. Настройка используется при выполнении запроса `SELECT` из распределенных таблиц, указывающих на реплицированные таблицы.

    По умолчанию форсирование запроса включено. 

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#settings-fallback_to_stale_replicas_for_distributed_queries).

    См. также настройку [Max replica delay for distributed queries](#setting-max-replica-delay-for-distributed-queries).

* **Force index by date**{#setting-force-index-by-date} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Запрещает выполнение запросов, если использовать индекс по дате невозможно. Работает с таблицами семейства [MergeTree](https://clickhouse.tech/docs/ru/engines/table-engines/mergetree-family/mergetree/).

    По умолчанию настройка выключена (выполнение запросов разрешено).

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#settings-force_index_by_date).

* **Force primary key**{#setting-force-primary-key} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Запрещает выполнение запросов, если использовать индекс по первичному ключу невозможно. Работает с таблицами семейства [MergeTree](https://clickhouse.tech/docs/ru/engines/table-engines/mergetree-family/mergetree/).

    По умолчанию настройка выключена (выполнение запросов разрешено).

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#settings-force_primary_key).

* **GROUP BY overflow mode**{#setting-group-by-overflow-mode} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет поведение {{ CH }}, когда количество уникальных ключей при агрегации [превысило ограничение](https://clickhouse.tech/docs/ru/operations/settings/query-complexity/#ogranicheniia-na-slozhnost-zaprosa):
    * `throw` — прервать выполнение, вернуть ошибку.
    * `break` — вернуть неполный результат.
    * `any` — выполнить `GROUP BY` приближенно. Качество такого вычисления зависит от статистических свойств данных.

    По умолчанию значение не выбрано (эквивалентно `throw`).

* **GROUP BY two level threshold**{#setting-group-by-two-level-threshold} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет количество ключей, при достижении которого начинается двухуровневая агрегация.

    Минимальное значение — `0` (не установлено), по умолчанию — `100000`.

* **GROUP BY two level threshold bytes**{#setting-group-by-two-level-threshold-bytes} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет количество байт в агрегате, при достижении которого начинается двухуровневая агрегация.

    Минимальное значение — `0` (не установлено), по умолчанию — `50000000`.

* **HTTP connection timeout**{#setting-http-connection-timeout} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Задает время ожидания установления HTTP-соединения (в миллисекундах).

    Минимальное значение — `1`, по умолчанию — `1000` (одна секунда).

* **HTTP headers progress interval**{#setting-http-headers progress interval} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Задает минимальный интервал (в миллисекундах) между уведомлениями о ходе выполнения запроса с помощью HTTP-заголовка `X-ClickHouse-Progress`.

    Минимальное значение — `1`, по умолчанию — `100`.

* **HTTP receive timeout**{#setting-http-receive-timeout} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Задает время ожидания приема данных через HTTP-соединение (в миллисекундах).

    Минимальное значение — `1`, по умолчанию — `1800000` (30 минут).

* **HTTP send timeout**{#setting-http-send-timeout} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Задает время ожидания отправки данных через HTTP-соединение (в миллисекундах).

    Минимальное значение — `1`, по умолчанию — `1800000` (30 минут).

* **Input format defaults for omitted fields**{#setting-input-format-defaults-for-omitted-fields} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, будут ли пропущенные поля заполнены значениями по умолчанию для типа данных столбца при вставке данных запросом `INSERT`.

    По умолчанию замена включена.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#session_settings-input_format_defaults_for_omitted_fields).

* **Input format values interpret expressions**{#setting-input-format-values-interpret-expressions} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Включает SQL-парсер, если потоковый парсер не может проанализировать данные. Используйте эту настройку, если вставляете в таблицу значения, содержащие выражения SQL.

    Например, потоковый парсер не распознает значение, содержащее в себе выражение `now()`, в то время как при включенном SQL-парсере выражение будет распознано корректно и в качестве значения будет вставлен результат выполнения SQL-функции `now()` (текущая дата и время).

    Эта опция используется только для формата [Values](https://clickhouse.tech/docs/ru/interfaces/formats/#data-format-values) при вставке данных.

    По умолчанию парсер SQL включен.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#settings-input_format_values_interpret_expressions).

* **Insert quorum**{#setting-insert-quorum} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Управляет кворумной записью в кластере {{ CH }}:
    * Если значение меньше 2, то кворумная запись выключена.
    * Если значение больше или равно 2, то кворумная запись включена.

    Кворумная запись гарантирует, что {{ CH }} без ошибок запишет данные в кворум из Insert quorum реплик за время не больше [Insert quorum timeout](#setting-insert-quorum-timeout), а данные не потеряются при выходе из строя одной или нескольких реплик. Все реплики в кворуме консистентны, т.е. содержат данные всех более ранних запросов `INSERT`.

    При чтении данных, записанных с помощью Insert quorum, можно использовать настройку [Select sequential consistency](#setting-select-sequential-consistency).

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#settings-insert_quorum).

* **Insert quorum timeout**{#setting-insert-quorum-timeout} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Задает время ожидания [кворумной записи](#setting-insert-quorum) в миллисекундах. Если время прошло, а запись так и не состоялась, то ClickHouse прервет выполнение запроса `INSERT` и вернет ошибку.

    Минимальное значение — `1000` (1 секунда), по умолчанию — `60000` (1 минута).

* **Join use nulls**{#setting-join-use-nulls} {{ tag-con }} {{ tag-cli }} {{ tag-sql }}

    Управляет поведением запросов `JOIN`. Если настройка включена, то появившиеся при объединении пустые ячейки заполняются значениями `NULL`. В противном случае ячейки заполняются значениями по умолчанию для данного типа поля.

    По умолчанию настройка выключена.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#join_use_nulls).

* **Joined subquery requires alias**{#setting-joined-subquery-requires-alias} {{ tag-con }} {{ tag-cli }} {{ tag-sql }}

    Требует наличия псевдонимов для подзапросов при выполнении операции `JOIN`.

    При включенной настройке подобный запрос не будет выполнен:

    ```sql
    SELECT col1, col2 FROM table1 JOIN (SELECT col3 FROM table2)
    ```

    При этом запрос с заданным псевдонимом будет выполнен успешно:

    ```sql
    SELECT col1, col2 FROM table1 JOIN (SELECT col3 FROM table2) AS MyQuery
    ```

    По умолчанию настройка выключена.

* **Low cardinality allow in native format**{#setting-low-cardinality-allow-in-native-format} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, использовать ли [тип LowCardinality](https://clickhouse.tech/docs/ru/sql-reference/data-types/lowcardinality/) в native-формате:
    * настройка включена (по умолчанию) — использовать.
    * настройка выключена — не использовать:
      * для запроса `SELECT` конвертировать столбцы типа LowCardinality в обычные;
      * для запроса `INSERT` конвертировать обычные столбцы в тип LowCardinality.

    Столбцы этого типа позволяют эффективнее хранить данные в виде хеш-таблиц. Если возможно, {{ CH }} использует столбцы типа LowCardinality.

    Некоторые сторонние клиенты для {{ CH }} не поддерживают столбцы типа LowCardinality и не могут правильно интерпретировать результат запроса, в котором присутствуют столбцы такого типа. Выключение настройки позволяет таким клиентам обрабатывать результаты запросов корректно.

    Официальный клиент {{ CH }} поддерживает работу со столбцами типа LowCardinality.

    По умолчанию настройка включена.

* **Max AST depth**{#setting-max-ast-depth} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальная глубина вложенности синтаксического дерева.

    Для сложных запросов синтаксическое дерево может оказаться слишком глубоким. Настройка позволяет запретить выполнение излишне сложных или неоптимизированных запросов для больших таблиц.

    Например, запрос `SELECT *` в большинстве случаев породит более сложное и глубокое синтаксическое дерево, чем запрос `SELECT ... WHERE ...` с ограничениями и условиями.

    По умолчанию выбрано значение `1000`. Слишком маленькое значение может привести к невозможности выполнения большинства запросов.

* **Max AST elements**{#setting-max-ast-elements} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество элементов синтаксического дерева запроса (количество узлов дерева).

    Для сложных запросов синтаксическое дерево может содержать слишком большое количество элементов. Настройка позволяет запретить выполнение излишне сложных или неоптимизированных запросов для больших таблиц.

    По умолчанию выбрано значение `50000`. Слишком маленькое значение может привести к невозможности выполнения большинства запросов.

* **Max block size**{#setting-max-block-size} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Данные в {{ CH }} обрабатываются по блокам (наборам кусочков столбцов). Настройка задает рекомендованный размер блока (количество строк), который будет загружаться при обработке таблиц. Обработка каждого блока влечет за собой накладные расходы, поэтому слишком маленькое значение настройки может замедлить обработку.

    Минимальное значение — `1`, по умолчанию — `65536`.

* **Max bytes before external `GROUP BY`**{#setting-max-bytes-before-external-group-by} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Данные, накопленные при выполнении операции агрегации `GROUP BY`, некоторое время хранятся в оперативной памяти. Настройка задает порог (в байтах), после которого эти данные сбрасываются на диск для экономии оперативной памяти.

    Настройка полезна, если запросы завершаются с ошибкой из-за того, что для выполнения операции агрегации большого объема данных не хватает оперативной памяти. Задайте ненулевое значение этой настройки, чтобы {{ CH }} сбрасывал данные на диск и успешно выполнял агрегацию в этом случае.

    Минимальное значение — `0` (`GROUP BY` во внешней памяти отключен), по умолчанию — `0`.

    При использовании агрегации во внешней памяти рекомендуется задать значение этой настройки в два раза меньше значения настройки [Max memory usage](#setting-max-memory-usage) (по умолчанию максимальное использование памяти ограничено десятью гигабайтами).

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/sql-reference/statements/select/group-by/#select-group-by-in-external-memory).

    См. также настройку [Distributed aggregation memory efficient](#setting-distributed-aggregation-memory-efficient).

* **Max bytes before external sort**{#setting-max-bytes-before-external-sort} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Настройка аналогична [предыдущей](#setting-max-bytes-before-external-group-by), но применяется для операции сортировки (`ORDER BY`).

* **Max bytes in DISTINCT**{#setting-max-bytes-in-distinct} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальный объем несжатых данных (в байтах), занимаемый хеш-таблицей при использовании `DISTINCT`.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max bytes to read**{#setting-max-bytes-to-read} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальный объем несжатых данных (в байтах), который можно прочитать из таблицы при выполнении запроса.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max bytes to sort**{#setting-max-bytes-to-sort} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальный объем несжатых данных (в байтах), который можно прочитать из таблицы до сортировки. Настройка позволяет ограничить потребление оперативной памяти при сортировке.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max bytes to transfer**{#setting-max-bytes-to-transfer} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальный объем несжатых данных (в байтах), который можно передать на удаленный сервер или сохранить во временную таблицу при использовании `GLOBAL IN`.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max columns to read**{#setting-max-columns-to-read} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество столбцов, которое можно прочитать из таблицы в одном запросе. Запросы, требующие чтения большего количества столбцов, завершатся с ошибкой.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max execution time**{#setting-max-execution-time} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное время выполнения запроса (в миллисекундах).

    Если выполнение запроса находится на одной из стадий сортировки, а также слияния и финализации агрегатных функций, то лимит на максимальное время выполнения запроса не будет проверяться и может быть превышен.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max expanded AST elements**{#setting-max-expanded-ast-elements} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество элементов синтаксического дерева запроса (количество узлов дерева) после раскрытия псевдонимов и звездочки.

    Для сложных запросов синтаксическое дерево может содержать слишком большое количество элементов. Настройка позволяет запретить выполнение излишне сложных или неоптимизированных запросов для больших таблиц.

    По умолчанию выбрано значение `500000`. Слишком маленькое значение может привести к невозможности выполнения большинства запросов.

* **Max INSERT block size**{#setting-max-insert-block-size} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Формировать блоки указанного размера (в байтах), при вставке в таблицу. Настройка действует, только когда сервер сам формирует такие блоки.

    По умолчанию выбрано значение `1048576`.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#settings-max_insert_block_size).

* **Max memory usage**{#setting-max-memory-usage} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальный объем оперативной памяти (в байтах) для выполнения запроса на одном сервере. Настройка не учитывает объем свободной памяти или общий объем памяти на машине. Ограничение действует на один запрос в пределах одного сервера.

    Минимальное значение — `0` (нет ограничения), по умолчанию — `10737418240` (10 ГБ).

    Если вы используете настройки [Max bytes before external `GROUP BY`](#setting-max-bytes-before-external-group-by) или [Max bytes before external sort](#setting-max-bytes-before-external-sort), рекомендуется задать им значения в два раза меньше значения Max memory usage.

* **Max memory usage for user**{#setting-max-memory-usage-for-user} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальный объем оперативной памяти (в байтах) для выполнения запросов пользователя на одном сервере. Настройка не учитывает объем свободной памяти или общий объем памяти на машине.

    Ограничение действует на все запросы пользователя, которые выполняются одновременно в пределах одного сервера, в отличие от [Max memory usage](#setting-max-memory-usage).

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max network bandwidth**{#setting-max-network-bandwidth} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальная скорость обмена данными по сети при выполнении одного запроса (в байтах в секунду).

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max network bandwidth for user**{#setting-max-network-bandwidth-for-user} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальная скорость обмена данными по сети (в байтах в секунду). Эта настройка влияет на все одновременно выполняющиеся запросы пользователя, в отличие от [Max network bandwidth](#setting-max-network-bandwidth).

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max query size**{#setting-max-query-size} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Ограничивает размер (в байтах) наибольшей части запроса, которая может быть передана в оперативную память для разбора с помощью SQL-парсера.

    Минимальное значение — `1`, по умолчанию — `262144`.

* **Max replica delay for distributed queries**{#setting-max-replica-delay-for-distributed-queries} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное отставание реплики (в миллисекундах). Если задержка реплики больше значения настройки, реплика перестает использоваться.

    Минимальное значение — `1000` (1 секунда), по умолчанию — `300000` (5 минут).

    См. также описание настройки [Fallback to stale replicas for distributed queries](#setting-fallback-to-stale-replicas-for-distributed-queries).

* **Max result bytes**{#setting-max-result-bytes} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальный размер результата запроса несжатых данных (в байтах). Ограничение распространяется также на подзапросы и на те части распределенных запросов, которые выполняются на удаленных серверах.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max result rows**{#setting-max-result-rows} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество строк результата. Ограничение распространяется также на подзапросы и на те части распределенных запросов, которые выполняются на удаленных серверах.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max rows in DISTINCT**{#setting-max-rows-in-distinct} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество различных строк при использовании `DISTINCT`.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max rows to group by**{#setting-max-rows-to-group-by} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество уникальных ключей, получаемых в процессе агрегации. Используйте настройку, чтобы ограничить потребление оперативной памяти при агрегации.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max rows to read**{#setting-max-rows-to-read} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество строк, которое можно прочитать из таблицы при выполнении запроса.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max rows to sort**{#setting-max-rows-to-sort} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество строк для сортировки. Используйте эту настройку, чтобы ограничить потребление оперативной памяти при сортировке.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max rows to transfer**{#setting-max-rows-to-transfer} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество строк, которое можно передать на удаленный сервер или сохранить во временную таблицу при использовании `GLOBAL IN`.

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max temporary columns**{#setting-max-temporary-columns} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество временных столбцов, которое единовременно хранится в оперативной памяти при выполнении запроса (с учетом постоянных столбцов).

    Минимальное значение и значение по умолчанию — `0` (нет ограничения). 

* **Max temporary non const columns**{#setting-max-temporary-non-const-columns} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество временных столбцов, которое единовременно хранится в оперативной памяти при выполнении запроса (без учета постоянных столбцов).

    Минимальное значение и значение по умолчанию — `0` (нет ограничения).

* **Max threads**{#setting-max-threads} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальное количество потоков обработки запроса, без учета потоков для чтения данных с удаленных серверов. Настройка относится к потокам, которые используются для параллельного выполнения стадий конвейера запросов.

    Минимальное значение и значение по умолчанию — `0` (вычислять значение автоматически как количество процессорных ядер без учета Hyper-Threading).

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#settings-max_threads).

* **Merge tree max bytes to use cache**{#setting-merge-tree-max-bytes-to-use-cache} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальный размер запроса (в байтах), при котором используется кеш несжатых данных. Кеш не используется для запросов, превышающих указанный размер.

    Настройка используется совместно с [Use uncompressed cache](#setting-use-uncompressed-cache).

    По умолчанию выбрано значение `192x10x1024x1024`.

* **Merge tree max rows to use cache**{#setting-merge-tree-max-rows-to-use-cache} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Максимальный размер запроса (в строках), при котором используется кеш несжатых данных. Кеш не используется для запросов, превышающих указанный размер.

    Настройка используется совместно с [Use uncompressed cache](#setting-use-uncompressed-cache).

    По умолчанию выбрано значение `128x8192`.

* **Merge tree min bytes for concurrent read**{#setting-merge-tree-min-bytes-for-concurrent-read} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Если количество прочитанных из файла байт превышает значение настройки, то {{ CH }} пытается выполнить одновременное чтение из этого файла в несколько потоков.

    Минимальное значение — `1`, по умолчанию — `24x10x1024x1024`.

* **Merge tree min rows for concurrent read**{#setting-merge-tree-min-rows-for-concurrent-read} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Если количество прочитанных из файла строк превышает значение настройки, то {{ CH }} пытается выполнить одновременное чтение из этого файла в несколько потоков.

    Минимальное значение — `1`, по умолчанию — `20x8192`.

* **Min bytes to use Direct I/O**{#setting-merge-tree-min-bytes-to-use-direct-io} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Объем данных (в байтах), необходимый для прямого чтения (Direct I/O) с диска.

    По умолчанию {{ CH }} читает данные не напрямую с диска, а полагается на файловую систему и ее кеш. Такое чтение эффективно при небольших объемах данных. Если данные читаются в больших объемах, эффективнее читать их с диска напрямую, минуя кеш файловой системы.

    Если общий объем всех хранимых данных для чтения превышает заданное значение настройки, тогда {{ CH }} читает данные с диска напрямую.

    Минимальное значение и значение по умолчанию — `0` (прямое чтение отключено).

* **Min count to compile**{#setting-min-count-to-compile} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }} 

    Определяет, после какого количества структурно идентичных запросов начать [компиляцию](#setting-compile).

    Для значения `0` компиляция выполняется синхронно: запрос ожидает окончания компиляции, затем продолжает выполняться. Рекомендуется использовать это значение только в целях тестирования.

    Для всех других значений компиляция выполняется асинхронно, в отдельном потоке: когда результат будет готов, он сразу же будет использован, в том числе уже выполняющимися в данный момент запросами. 

    Минимальное значение — `0`, по умолчанию — `3`.

* **Min count to compile expression**{#setting-min-count-to-compile-expression} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, после какого количества идентичных выражений начать [компиляцию выражений](#setting-compile-expressions).

    Для значения `0` компиляция выполняется синхронно: выражение ожидает окончания компиляции, затем продолжается выполнение запроса. Рекомендуется использовать это значение только в целях тестирования.

    Для всех других значений компиляция выполняется асинхронно, в отдельном потоке: когда результат будет готов, он сразу же будет использован, в том числе уже выполняющимися в данный момент запросами.

    Минимальное значение — `0`, по умолчанию — `3`.

* **Min INSERT block size bytes**{#setting-min-insert-block-size-bytes} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Минимальный размер блока (в байтах), который может быть вставлен в таблицу запросом `INSERT`. Блоки меньшего размера [склеиваются вместе](https://clickhouse.tech/docs/ru/engines/table-engines/mergetree-family/mergetree/#mergetree-data-storage).

    Минимальное значение — `0` (склейка блоков выключена), по умолчанию — `‭268435456‬‬` (256 МБ).

* **Min INSERT block size rows**{#setting-min-insert-block-size-rows} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Минимальный размер блока (в строках), который может быть вставлен в таблицу запросом `INSERT`. Блоки меньшего размера [склеиваются вместе](https://clickhouse.tech/docs/ru/engines/table-engines/mergetree-family/mergetree/#mergetree-data-storage).

    Минимальное значение — `0` (склейка блоков выключена), по умолчанию — `1048576`.

* **Output format JSON quote denormals**{#setting-output-format-json-quote-denormals} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, выводить ли специальные значения для чисел с плавающей запятой (`+nan`, `-nan`, `+inf` и `-inf`) при использовании формата JSON для результата.

    По умолчанию выбрано значение `false` — специальные значения не выводятся.

* **Output format JSON quote_64bit integers**{#setting-output-format-json-quote-64bit-integers} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

  Определяет формат чисел в JSON-выводе. Если опция включена, то при выводе в JSON 64-битные числа (`UInt64` и `Int64`) выводятся в кавычках (из соображений совместимости с большинством реализаций JavaScript), иначе — без кавычек. 

  По умолчанию вывод 64-битных целых чисел в кавычках выключен.

* **Priority**{#setting-priority} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет приоритет запроса:
    * `0` — приоритет не используется.
    * `1` — наивысший приоритет.
    * далее — чем выше число, тем ниже приоритет.

    Настройка выставляется отдельно для каждого запроса.

    Если в {{ CH }} поступает запрос с более высоким приоритетом, выполнение менее приоритетных запросов приостанавливается до завершения поступившего запроса.

    Минимальное значение и значение по умолчанию — `0`.

* **Quota mode**{#setting-quota-mode} {{ tag-con }} {{ tag-cli }} {{ tag-sql }}

    Режим учета потребляемых ресурсов при включенных квотах:
    * `default` — ключи не используются.
    * `keyed` — в параметре пользовательского запроса передается ключ `quota_key`, квота считается по отдельности для каждого значения ключа.
    * `keyed_by_ip` — аналогичен предыдущему, но в качестве ключа выступает IP-адрес пользователя. Рекомендуется использовать этот режим только в тех случаях, если у пользователя нет способов быстро сменить IP-адрес, например, для обхода ограничений квоты. Это относится как к IPv4-, так и IPv6-адресам.

    По умолчанию значение не выбрано (эквивалентно `default`).

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/quotas/).

* **Read overflow mode**{#setting-read-overflow-mode} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет поведение {{ CH }}, когда количество прочитанных данных [превысило одно из ограничений](https://clickhouse.tech/docs/ru/operations/settings/query-complexity/#ogranicheniia-na-slozhnost-zaprosa):
    * `throw` (прервать выполнение, вернуть ошибку);
    * `break` (вернуть неполный результат).

    По умолчанию значение не выбрано (эквивалентно `throw`).

* **Readonly**{#setting-readonly} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Разрешения для запросов на изменение параметров, чтение и запись данных:
    * `0` (по умолчанию) — разрешено выполнять все типы запросов;
    * `1` — разрешено выполнять только запросы на чтение данных;
    * `2` — разрешено выполнять запросы на чтение данных и изменение настроек.

    Настройка не влияет на возможность выполнения DDL-запросов. Чтобы разрешить или запретить выполнение DDL-запросов, используйте настройку [Allow DDL](#setting-allow-ddl). 

    См. также: [пример создания пользователя с правами «только чтение»](../../managed-clickhouse/operations/cluster-users.md#example-create-readonly-user).

* **Receive timeout**{#setting-receive-timeout} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Время ожидания приема данных (в миллисекундах).

    По умолчанию выбрано значение `300000` (5 минут).

* **Replication ALTER partitions sync**{#setting-replication-alter-partitions-sync} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

  Задает условия ожидания завершения асинхронных действий на репликах для запросов `ALTER ... ATTACH DETACH DROP`:
  * `0` — не ждать.
  * `1` — ждать выполнения только у себя (значение по умолчанию).
  * `2` — ждать всех.
 
  Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/sql-reference/statements/alter/#sinkhronnost-zaprosov-alter).
 
* **Result overflow mode**{#setting-result-overflow-mode} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет поведение {{ CH }}, когда объем результата [превысил одно из ограничений](https://clickhouse.tech/docs/ru/operations/settings/query-complexity/#ogranicheniia-na-slozhnost-zaprosa):
    * `throw` (прервать выполнение, вернуть ошибку);
    * `break` (вернуть неполный результат).

    По умолчанию значение не выбрано (эквивалентно `throw`).

* **SELECT sequential consistency**{#setting-select-sequential-consistency} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, нужно ли обеспечивать последовательную консистентность для запросов `SELECT`.

    По умолчанию последовательная консистентность не обеспечивается.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#settings-select_sequential_consistency).

* **Send progress in HTTP headers**{#setting-send-progress-in-http-headers} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Разрешает отправку уведомлений о ходе выполнения запроса с использованием заголовков `X-ClickHouse-Progress`.

    По умолчанию отсылка уведомлений выключена.

* **Send timeout**{#setting-send-timeout} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Время ожидания отправки данных (в миллисекундах).

    По умолчанию выбрано значение `300000` (5 минут).

* **Skip unavailable shards**{#setting-skip-unavailable-shards} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Разрешает тихий пропуск недоступных шардов. Шард считается недоступным, если все его реплики недоступны.

    По умолчанию тихий пропуск недоступных шардов выключен.

* **Sort overflow mode**{#setting-sort-overflow-mode} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет поведение {{ CH }}, когда количество полученных перед сортировкой строк [превысило одно из ограничений](https://clickhouse.tech/docs/ru/operations/settings/query-complexity/#ogranicheniia-na-slozhnost-zaprosa):
    * `throw` (прервать выполнение, вернуть ошибку);
    * `break` (вернуть неполный результат).

    По умолчанию значение не выбрано (эквивалентно `throw`).

* **Timeout overflow mode**{#setting-timeout-overflow-mode} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет поведение {{ CH }}, когда запрос выполняется дольше [max_execution_time](#setting-max-execution-time):
    * `throw` (прервать выполнение, вернуть ошибку);
    * `break` (вернуть неполный результат).

    По умолчанию значение не выбрано (эквивалентно `throw`).

* **Transfer overflow mode**{#setting-transfer-overflow-mode} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет поведение {{ CH }}, когда количество данных для передачи на другой сервер [превысило одно из ограничений](https://clickhouse.tech/docs/ru/operations/settings/query-complexity/#ogranicheniia-na-slozhnost-zaprosa):
    * `throw` (прервать выполнение, вернуть ошибку);
    * `break` (вернуть неполный результат).

    По умолчанию значение не выбрано (эквивалентно `throw`).

* **Transform null in**{#setting-transform-null-in} {{ tag-con }} {{ tag-cli }} {{ tag-sql }}

    При включенной опции сравнение `NULL = NULL` вернет `true` в операторе `IN`.

    По умолчанию настройка выключена.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#transform_null_in).

* **Use uncompressed cache**{#setting-use-uncompressed-cache} {{ tag-con }} {{ tag-cli }} {{ tag-api }} {{ tag-sql }}

    Определяет, использовать ли кеш разжатых блоков. Использование этого кеша может существенно сократить задержку и увеличить пропускную способность при работе с большим количеством коротких запросов (только для таблиц семейства [MergeTree](https://clickhouse.tech/docs/ru/engines/table-engines/mergetree-family/mergetree/)). Включите эту настройку для пользователей, от которых идут частые короткие запросы.

    По умолчанию кеш не используется.

    Подробнее см. [в документации {{ CH }}](https://clickhouse.tech/docs/ru/operations/settings/settings/#setting-use_uncompressed_cache).

    См. также настройки [Merge tree max bytes to use cache](#setting-merge-tree-max-bytes-to-use-cache) и [Merge tree max rows to use cache](#setting-merge-tree-max-rows-to-use-cache).
