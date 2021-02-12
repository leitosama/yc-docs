# Информация об имеющихся кластерах

Вы можете запросить детальную информацию о каждом созданном вами кластере {{ mkf-name }}.

## Получить список кластеров в каталоге {#list-clusters}

{% list tabs %}

- Консоль управления

  Перейдите на страницу каталога и выберите сервис **{{ mkf-name }}**.


- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы запросить список кластеров {{ KF }} в каталоге по умолчанию, выполните команду:

  ```
  {{ yc-mdb-kf }} cluster list

  +----------------------+----------+---------------------+--------+---------+
  |          ID          |   NAME   |     CREATED AT      | HEALTH | STATUS  |
  +----------------------+----------+---------------------+--------+---------+
  | c9qaruvk2mmaeaf8h0el | kafka750 | 2020-12-18 05:21:27 | ALIVE  | RUNNING |
  | ...                                                                      |
  +----------------------+----------+---------------------+--------+---------+
  ```

- API
  
  Воспользуйтесь методом API [list](../api-ref/Cluster/list.md): передайте значение идентификатора требуемого каталога в параметре `folderId` запроса.
  
  В ответе будут содержаться имена и идентификаторы кластеров.

{% endlist %}

## Получить детальную информацию о кластере {#get-cluster}

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога и выберите сервис **{{ mkf-name }}**.
  1. Нажмите на имя нужного кластера.


- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы получить информацию о {{ KF }}-кластере, выполните команду:

  ```
  {{ yc-mdb-kf }} cluster get <имя или идентификатор кластера>
  ```

  Идентификатор и имя кластера можно запросить со [списком кластеров в каталоге](#list-clusters).

- API
  
  Воспользуйтесь методом API [get](../api-ref/Cluster/get.md): передайте значение идентификатора требуемого кластера в параметре `clusterId` запроса.

  Чтобы узнать идентификатор кластера, [получите список кластеров в каталоге](#list-clusters).

{% endlist %}

## Посмотреть список операций в кластере {#list-operations}

{% include [list-operations-about](../../_includes/mdb/mkf-list-operations-about.md) %}

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога и выберите сервис **{{ mkf-name }}**.
  1. Нажмите на имя нужного кластера.
  1. Перейдите на вкладку **Операции**.

- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы получить список операций, выполните команду:

  ```
  {{ yc-mdb-kf }} cluster list-operations <имя или идентификатор кластера>
  ```

  Идентификатор и имя кластера можно запросить со [списком кластеров в каталоге](#list-clusters).


- API

  Получить список операций можно с помощью метода [listOperations](../api-ref/Cluster/listOperations.md).

{% endlist %}