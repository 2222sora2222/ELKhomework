### Задание 1. Elasticsearch

**Цель:** Установить и запустить Elasticsearch, поменять параметр `cluster_name` на случайный.

**Ход выполнения:**
1. Добавлен официальный репозиторий Elastic (или зеркало) и установлен пакет `elasticsearch`.
2. В файле конфигурации `/etc/elasticsearch/elasticsearch.yml` изменён параметр `cluster.name` на случайное значение: `random-cluster-7f3a9`.
3. Служба перезапущена командой `sudo systemctl restart elasticsearch`.
4. Проверка статуса кластера выполнена командой `curl -X GET 'localhost:9200/_cluster/health?pretty'`.

**Скриншот 1. Вывод команды curl с изменённым cluster_name:**
![Скриншот-1](https://github.com/2222sora2222/ELKhomework/blob/main/img/task1.png)

---

### Задание 2. Kibana

**Цель:** Установить и запустить Kibana, выполнить запрос через Dev Tools.

**Ход выполнения:**
1. Установлен пакет `kibana`, служба запущена и добавлена в автозагрузку.
2. Открыт интерфейс Kibana по адресу `http://localhost:5601/app/dev_tools#/console`.
3. В консоли Dev Tools выполнен запрос `GET /_cluster/health?pretty`.

**Скриншот 2. Интерфейс Kibana Dev Tools с выполненным запросом:**
![Скриншот-2](https://github.com/2222sora2222/ELKhomework/blob/main/img/task2.png)

---

### Задание 3. Logstash

**Цель:** Установить и запустить Logstash и Nginx. С помощью Logstash отправить access-лог Nginx в Elasticsearch.

**Ход выполнения:**
1. Установлены и запущены службы `nginx` и `logstash`.
2. Создан конфигурационный файл `/etc/logstash/conf.d/nginx-to-es.conf`, который читает `/var/log/nginx/access.log` и отправляет данные в Elasticsearch (индекс `nginx-log-%{+YYYY.MM.dd}`).
3. Исправлена ошибка прав доступа (`Permission denied`): пользователь `logstash` добавлен в группу `adm` (`sudo usermod -aG adm logstash`).
4. Logstash перезапущен. В Kibana создан Data View `nginx-log-*`.

**Скриншот 3. Логи Nginx в Kibana (Discover), отправленные через Logstash:**
![Скриншот-3](https://github.com/2222sora2222/ELKhomework/blob/main/img/task3.png)

---

### Задание 4. Filebeat

**Цель:** Установить и запустить Filebeat. Переключить поставку логов Nginx с Logstash на Filebeat.

**Ход выполнения:**
1. Служба Logstash остановлена. Установлен пакет `filebeat`.
2. В файле `/etc/filebeat/filebeat.yml` настроен сбор логов Nginx (`/var/log/nginx/access.log`) и вывод напрямую в Elasticsearch (индекс `filebeat-nginx-%{+yyyy.MM.dd}`).
3. Выполнена команда `filebeat setup`, служба Filebeat запущена.
4. В Kibana создан Data View `filebeat-nginx-*`. В логах видно, что данные поступают от агента `filebeat`.

**Скриншот 4. Логи Nginx в Kibana (Discover), отправленные через Filebeat:**
![Скриншот-4](https://github.com/2222sora2222/ELKhomework/blob/main/img/task4.png)