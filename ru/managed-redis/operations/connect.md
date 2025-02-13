# Подключение к базе данных в кластере {{ RD }}

К хосту базы данных Redis можно подключиться:

* Без использования шифрования (для кластеров с любыми версиями {{ RD }}):

    * Напрямую через порт `6379`.
    * C использованием [{{ RD }} Sentinel](https://redis.io/topics/sentinel) через порт `26379`. Это система управления хостами {{ RD }} с возможностями мониторинга, отправки уведомлений о состояниях хостов, переключения мастера и передачи клиентам актуальных адресов хостов.

        Не все клиентские приложения {{ RD }} поддерживают подключение через Sentinel, однако вы можете подключиться к хосту напрямую. Обратите внимание, что при этом вам придется самостоятельно отслеживать роли всех хостов. Если в прямом подключении нет необходимости, используйте Sentinel для более надежной работы с хостами кластера.

* С использованием шифрования (для кластеров с {{ RD }} версии 6 и старше [c включенным SSL-шифрованием](cluster-create.md#create-cluster)).

   При включенном шифровании соединений можно подключиться только напрямую к хосту {{ RD }} через порт `6380`. Для подключения к нешардированному кластеру на запись рекомендуется использовать [особые FQDN](#special-fqdns).

## Подключение к кластеру {#connect}

Хостам кластера {{ RD }} нельзя назначать публичные IP-адреса. Доступ к хостам возможен только из той же [облачной сети](../../vpc/concepts/network.md), в которой находится хост.

Чтобы подключиться к хосту кластера {{ RD }}:

1. [Создайте виртуальную машину](../../compute/operations/vm-create/create-linux-vm.md) с публичным IP-адресом в той же виртуальной сети, что и хост.
1. Подключитесь к созданной виртуальной машине [через SSH](../../compute/operations/vm-connect/ssh.md) и из нее подключитесь к {{ RD }} с помощью одного из [примеров строк подключений](#connection-string).

## Настройка групп безопасности {#configuring-security-groups}

{% include [sg-rules](../../_includes/mdb/sg-rules-connect.md) %}

Настройки правил будут различаться в зависимости от выбранного способа подключения:

1. [Настройте все группы безопасности](../../vpc/operations/security-group-update.md#add-rule) кластера так, чтобы они разрешали входящий трафик из группы безопасности, в которой находится ВМ, на порт `6379` для подключения напрямую или `26379` при подключении с помощью Sentinel. Если кластер создан с поддержкой SSL-шифрования, то укажите только порт `6380` (подключение по Sentinel не поддерживается для таких кластеров). Для этого в этих группах создайте следующее правило для входящего трафика:

    * протокол: `TCP`;
    * диапазон портов: `6379` (`26379` для Sentinel) или только `6380` для кластера с поддержкой SSL-шифрования;
    * тип источника: `Группа безопасности`;
    * источник: группа безопасности, в которой находится ВМ. Если она совпадает с настраиваемой группой, то укажите **Текущая**.

1. [Настройте группу безопасности](../../vpc/operations/security-group-update.md#add-rule), в которой находится ВМ так, чтобы можно было подключаться к ВМ и был разрешен трафик между ВМ и хостами кластера.

    Пример правил для ВМ:

    * Для входящего трафика:
        * протокол: `TCP`;
        * диапазон портов: `22`;
        * тип источника: `CIDR`;
        * источник: `0.0.0.0/0`.

        Это правило позволяет подключаться к ВМ по протоколу SSH.

    * Для исходящего трафика:
        * протокол: `Any`;
        * диапазон портов: `0-65535`;
        * тип назначения: `CIDR`;
        * назначение: `0.0.0.0/0`.

        Это правило разрешает любой исходящий трафик, что позволяет не только подключаться к кластеру, но и устанавливать сертификаты и утилиты, необходимые для подключения к кластеру.

{% note info %}

Вы можете задать более детальные правила для групп безопасности, например, разрешающие трафик только в определенных подсетях.

Группы безопасности должны быть корректно настроены для всех подсетей, в которых будут размещены хосты кластера. При неполных или некорректных настройках групп безопасности можно потерять доступ к кластеру, если произойдет [ручная](failover.md) или [автоматическая](../concepts/replication.md) смена мастера.

{% endnote %}

Подробнее о группах безопасности см. в разделе [{#T}](../concepts/network.md#security-groups).

## Получение SSL-сертификата {#get-ssl-cert}

Чтобы использовать шифрованное SSL-соединение, получите SSL-сертификат:


```bash
sudo mkdir ~/.redis && \
sudo wget "https://{{ s3-storage-host }}{{ pem-path }}" -O ~/.redis/YandexInternalRootCA.crt && \
sudo chmod 655 ~/.redis/YandexInternalRootCA.crt
```

{% include [ide-ssl-cert](../../_includes/mdb/mdb-ide-ssl-cert.md) %}

## Подключение из графических IDE {#connection-ide}

{% include [ide-environments](../../_includes/mdb/mrd-ide-envs.md) %}

Подключаться из графических IDE к хостам кластера можно только через SSH-туннель с помощью [созданной ВМ](#connect). Перед подключением [подготовьте сертификат](#get-ssl-cert).  

{% list tabs %}

- DBeaver

  Поддержка подключения к {{ RD }}-кластеру доступна только в [коммерческих редакциях DBeaver](https://dbeaver.com/buy/).

  Чтобы подключиться к кластеру:

  1. Создайте новое соединение с БД:
     1. Выберите в меню **База данных** пункт **Новое соединение**.
     1. Выберите из списка БД **{{ RD }}**.
     1. Нажмите кнопку **Далее**.
     1. Укажите параметры подключения на вкладке **Главное**:
        * **Хост** — FQDN хоста-мастера или [особый FQDN](#special-fqdns);
        * **Порт** — `{{ port-mrd }}` для обычного кластера или `{{ port-mrd-tls }}` для кластера с включенным SSL-шифрованием;
        * В блоке **Аутентификация** укажите пароль от кластера.
     1. На вкладке **SSH**:
        1. Включите настройку **Использовать туннель SSH**.
        1. Укажите параметры SSH-туннеля:
           * **Хост/IP** — публичный IP-адрес [ВМ для подключения](#connect);
           * **Имя пользователя** — логин для подключения к ВМ;
           * **Метод аутентификации** — `Публичный ключ`;
           * **Секретный ключ** — путь к файлу закрытого ключа для подключения к ВМ;
           * **Passphrase** — пароль от закрытого ключа.
     1. На вкладке **SSL**:
        1. Включите настройки **Использовать SSL** и **Пропустить валидацию имени хоста**.
        1. В блоке **Способ**:
           1. Включите настройку **Набор сертификатов**.
           1. В поле **Корневой сертификат** укажите путь к файлу [SSL-сертификата для подключения](#get-ssl-cert).
  1. Нажмите кнопку **Тест соединения ...** для проверки соединения с БД. При успешном подключении будет выведен статус подключения, информация о СУБД и драйвере.
  1. Нажмите кнопку **Готово**, чтобы сохранить настройки соединения с БД.

{% endlist %}

## Примеры строк подключения {#connection-string}

{% include [conn-strings-environment](../../_includes/mdb/mdb-conn-strings-env.md) %}

{% include [see-fqdn-in-console](../../_includes/mdb/see-fqdn-in-console.md) %}

{% include [mrd-connection-strings](../../_includes/mdb/mrd-conn-strings.md) %}

## Особые FQDN {#special-fqdns}

Наравне с обычными FQDN, которые можно запросить со [списком хостов в кластере](hosts.md#list), {{ mrd-name }} предоставляет особые FQDN, которые можно использовать только при подключении к нешардированному кластеру.

### Текущий мастер {#fqdn-master}

FQDN вида `c-<идентификатор кластера>.rw.{{ dns-zone }}` в нешардированном кластере всегда указывает на текущий хост-мастер. Идентификатор кластера можно запросить со [списком кластеров в каталоге](cluster-list.md#list-clusters). 

При подключении к этому FQDN разрешено выполнять операции чтения и записи.

Пример подключения с SSL-шифрованием к хосту-мастеру для кластера с идентификатором `c9qash3nb1v9ulc8j9nm`:

```bash
redis-cli -h c-c9qash3nb1v9ulc8j9nm.rw.{{ dns-zone }} \
      -p 6380 \
      --tls \
      --cacert ~/.redis/YandexInternalRootCA.crt \
      -a <пароль {{ RD }}>
```
