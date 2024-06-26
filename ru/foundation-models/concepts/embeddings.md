# Векторизация текста

Компьютеры обрабатывают любую информацию в виде чисел. Чтобы эффективно работать с текстами на естественных языках, модели искусственного интеллекта преобразовывают слова, фразы и предложения в числовые векторы фиксированного размера, которые сохраняют характеристики слов и связи между ними.

Процесс преобразования текста в такие векторы называется _векторизацией_, а результирующий вектор — _эмбеддингом_. Эмбеддинги хранят информацию о тексте и позволяют применять математические методы для обработки текстов. Так с помощью эмбеддингов можно классифицировать информацию, сравнивать и сопоставлять тексты или организовать поиск по собственной базе знаний.

## Модели векторного представления текста {#yandexgpt-embeddings}

{{ foundation-models-full-name }} предоставляет две модели векторизации текста. Обращение к модели по API возможно по ее [URI](https://ru.wikipedia.org/wiki/URI), содержащему [идентификатор каталога](../../resource-manager/operations/folder/get-id.md). Опция `/latest` указывает версию вызываемой модели и является опциональной. 

| Назначение | URI | Размерность выходного вектора | Режим работы |
|---|---|---|---|
| Векторизация больших текстов исходных данных, например статей документации. | `emb://<идентификатор_каталога>/text-search-doc/latest` | 256 | Синхронный |
| Векторизация коротких текстов: поисковых запросов, обращений и т.п. | `emb://<идентификатор_каталога>/text-search-query/latest` | 256 | Синхронный |

## Пример использования эмбеддингов {#example}

Примитивный пример показывает, как с помощью эмбеддингов можно найти наиболее близкий ответ на вопрос по базе знаний. В массиве `docs_text` собраны исходные данные для векторизации (база знаний), `query_text` содержит поисковый запрос. После получения эмбеддингов можно вычислить расстояние между каждым вектором в базе знаний и вектором запроса, и найти наиболее близкий текст.

{% list tabs group=programming_language %}

- Python 3 {#python}

  ```python
  import requests
  import numpy as np
  from scipy.spatial.distance import cdist

  FOLDER_ID = "<идентификатор_каталога>"
  IAM_TOKEN = "<IAM-токен>"

  doc_uri = f"emb://{FOLDER_ID}/text-search-doc/latest"
  query_uri = f"emb://{FOLDER_ID}/text-search-query/latest"

  embed_url = "https://llm.api.cloud.yandex.net:443/foundationModels/v1/textEmbedding"
  headers = {"Content-Type": "application/json", "Authorization": f"Bearer {IAM_TOKEN}", "x-folder-id": f"{FOLDER_ID}"}

  doc_texts = [
    """Александр Сергеевич Пушкин (26 мая [6 июня] 1799, Москва — 29 января [10 февраля] 1837, Санкт-Петербург) — русский поэт, драматург и прозаик, заложивший основы русского реалистического направления, литературный критик и теоретик литературы, историк, публицист, журналист.""",
    """Ромашка — род однолетних цветковых растений семейства астровые, или сложноцветные, по современной классификации объединяет около 70 видов невысоких пахучих трав, цветущих с первого года жизни."""
  ]

  query_text = "когда день рождения Пушкина?"

  def get_embedding(text: str, text_type: str = "doc") -> np.array:
      query_data = {
          "modelUri": doc_uri if text_type == "doc" else query_uri,
          "text": text,
      }

      return np.array(
          requests.post(embed_url, json=query_data, headers=headers).json()["embedding"]
      )


  query_embedding = get_embedding(query_text, text_type="query")
  docs_embedding = [get_embedding(doc_text) for doc_text in doc_texts]

  # Вычисляем косинусное расстояние
  dist = cdist(query_embedding[None, :], docs_embedding, metric="cosine")

  # Вычисляем косинусное сходство
  sim = 1 - dist

  # most similar doc text
  print(doc_texts[np.argmax(sim)])
  ```

  Где:

  * `<идентификатор_каталога>` — идентификатор каталога {{ yandex-cloud }}.
  * `<IAM-токен>` — [IAM-токен](../../iam/concepts/authorization/iam-token.md) аккаунта для [аутентификации в API](../api-ref/authentication.md).

  Результат выполнения:

  ```text
  Александр Сергеевич Пушкин (26 мая [6 июня] 1799, Москва — 29 января [10 февраля] 1837, Санкт-Петербург) — русский поэт, драматург и прозаик, заложивший основы русского реалистического направления, литературный критик и теоретик литературы, историк, публицист, журналист.
  ```

{% endlist %}